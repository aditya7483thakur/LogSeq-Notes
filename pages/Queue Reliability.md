- Why does reliability matter?
	- Queue reliability answers a simple question: how do we make sure work is not lost when something fails?
	- Example flow
	  ```
	  User
	  |
	  v
	  Create Cluster
	  ```
	- The queue receives the job.
	- The worker starts processing.
	- Failures can happen at any point:
		- Server crashes
		- Network timeout
		- OpenStack API fails
- Reliability roadmap
	- Keep this note focused on only these concepts:
		- ACK
		- Delivery guarantees
		- Retry
		- Backoff
		- DLQ
		- Idempotency
	- Nothing more.
- 1. ACK (Acknowledgement)
	- Problem
		- Queue delivers a job to the worker.
		- Worker finishes processing.
		- Queue needs a signal to know the job succeeded.
	- Definition
		- ACK is confirmation that a job was successfully processed.
	- Flow
	  ```
	  Queue
	  |
	  | Deliver Job
	  v
	  Worker
	  
	  Worker finishes
	  
	  Worker
	  |
	  | ACK
	  v
	  Queue
	  ```
	- Why it exists
		- Without ACK, the queue does not know whether the job succeeded.
		- With ACK, the queue can safely remove the job.
	- Interview answer
		- ACK is a signal sent by the consumer to the queue or broker indicating that processing completed successfully.
- 2. Delivery guarantees
	- Why delivery guarantees exist
		- Queue sends a job.
		- Worker processes it.
		- ACK gets lost because of a network failure.
		- The queue now thinks the job was never processed.
		- Delivery guarantees decide what happens next.
	- At-most-once
		- Definition: 0 or 1 delivery.
		- Never retry.
		- Outcome: job may be lost forever.
		- Benefit: no duplicates.
		- Problem: possible data loss.
		- Example use cases:
			- Analytics
			- Metrics
			- Logging
	- At-least-once
		- Definition: 1 or more deliveries.
		- Retry until acknowledged.
		- Outcome: no loss, but duplicates are possible.
		- Example use cases:
			- Payment processing
			- Order creation
		- Example flow
		  ```
		  Queue
		  |
		  v
		  Worker
		  ```
			- Worker finishes.
			- ACK is lost.
			- Queue thinks the job was not processed.
			- Queue resends the job.
			- The job runs twice.
	- Exactly-once
		- Definition: processed exactly once.
		- Reality: extremely difficult and expensive.
		- Interview secret:
			- Most real systems use at-least-once plus idempotency instead of true exactly-once.
	- What to remember
	  | Type | Loss | Duplicates |
	  | --- | --- | --- |
	  | At most once | Possible | No |
	  | At least once | No | Possible |
	  | Exactly once | No | No |
- 3. Retry
	- Why retries exist
		- Temporary failures happen.
		- Retrying later often succeeds.
		- Example: AWS API timeout.
	- Flow
	  ```
	  Job
	  |
	  Fail
	  |
	  Retry
	  |
	  Success
	  ```
	- BullMQ example
		- `attempts: 3` means:
			- Try 1
			- Try 2
			- Try 3
		- After that, the job fails permanently.
- 4. Backoff
	- Problem without backoff
		- Fail
		- Retry
		- Fail
		- Retry
		- Fail
		- Retry
		- This hammers the system.
	- Exponential backoff
		- Example config:
		  ```
		  type: "exponential"
		  delay: 5000
		  ```
		- Meaning:
			- Retry 1 -> 5 sec
			- Retry 2 -> 10 sec
			- Retry 3 -> 20 sec
	- Why it exists
		- Gives failing systems time to recover.
	- Interview answer
		- Exponential backoff increases retry intervals after each failure to avoid overwhelming a struggling dependency.
- 5. Dead letter queue (DLQ)
	- Problem
		- Some jobs never succeed.
		- Example invalid payload:
		  ```
		  {
		  "email": "invalid"
		  }
		  ```
		- No amount of retries will fix this.
	- Without DLQ
		- Retry forever.
		- Wastes resources.
	- With DLQ
	  ```
	  Main Queue
	    |
	  Retry
	    |
	  Fail
	    |
	    v
	   DLQ
	  ```
	- Definition
		- A queue containing permanently failed jobs.
	- Benefits
		- Main queue stays healthy.
		- Failed jobs can be investigated.
		- Jobs can be replayed later.
	- Interview answer
		- DLQ stores messages that exceeded retry limits so they can be analyzed without blocking normal processing.
- 6. Idempotency
	- Why this matters
		- This is probably the most important reliability concept.
		- Duplicate processing is normal in at-least-once systems.
	- Problem
		- Suppose `Create Invoice` runs twice.
		- Result:
			- Invoice 101
			- Invoice 102
		- Customer gets charged twice.
	- Definition
		- An operation is idempotent if running it once and running it multiple times gives the same result.
	- Example
		- Bad:
		  ```
		  balance += 100
		  ```
			- Running twice changes the result.
		- Good:
		  ```
		  Set status = ACTIVE
		  ```
			- Running twice gives the same result.
	- Why idempotency matters
		- At-least-once delivery means duplicates can happen.
		- Instead of trying to eliminate duplicates completely, design consumers to be idempotent.
	- Interview gold statement
		- In distributed systems, duplicates are inevitable. Instead of trying to eliminate duplicates completely, we design consumers to be idempotent.