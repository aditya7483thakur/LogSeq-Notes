- 1) What is a Message Queue?
	- Definition
		- A Message Queue is a temporary storage layer between a producer and a consumer.
		- Flow
		  ```
		  Producer
		    |
		    v
		  Queue
		    |
		    v
		  Consumer
		  ```
		- Producer creates work.
		- Consumer performs work.
		- Queue stores work until a consumer is ready.
	- Real example (BullMQ case)
		- Kubernetes control-plane creation can involve many tasks:
			- Create VPC
			- Create subnets
			- Create IAM roles
			- Create nodes
			- Configure networking
			- Wait for health checks
		- Instead of doing all of this in a single API request, enqueue the job.
		- Pattern
		  ```
		  API
		   |
		   v
		  BullMQ Queue
		   |
		   +--> Worker A
		   +--> Worker B
		   +--> Worker C
		  ```
		- Result: API responds quickly; workers handle heavy tasks asynchronously.
- 2) Why do message queues exist?
	- Interview focus: this is asked very often.
	- Problem 1: Synchronous processing
		- Without queue
		  ```
		  User
		   |
		   v
		  Order Service
		   |
		   +--> Email
		   +--> Analytics
		   +--> Notification
		  ```
		- Problems
			- Slow response time
			- User waits
			- One slow dependency slows everything
		- With queue
		  ```
		  User -> Order Service -> Queue
		                          |
		                          +--> Email Worker
		                          +--> Analytics Worker
		                          +--> Notification Worker
		  ```
		- Result: return immediately, process in background.
	- Problem 2: Traffic spikes
		- Normal load: `100 jobs/min`
		- Spike load: `50,000 jobs/min`
		- Without queue: workers crash.
		- With queue: queue absorbs burst and smooths worker load.
	- Problem 3: Reliability
		- If email service is down:
			- Without queue: message can be lost
			- With queue: job is stored and retried later
	- Problem 4: Decoupling
		- Producer does not need to know each consumer.
		- Queue acts as the contract between producers and consumers.
- 3) Core components
	- Producer
		- Creates messages/jobs.
		- Example
		  ```ts
		  await queue.add("create-cluster", payload);
		  ```
		- Responsibility: submit work, not execute work.
	- Queue
		- Stores messages/jobs.
		- Acts as a buffer.
		- Responsibility: keep work safe until processed.
	- Consumer / Worker
		- Processes jobs from queue.
		- Example
		  ```ts
		  new Worker("create-cluster", async (job) => {
		    // create kubernetes resources
		  });
		  ```
		- Responsibility: execute work.
	- Broker
		- System managing queue internals.
		- Examples
			- BullMQ -> Redis
			- RabbitMQ
			- Kafka
		- Responsibility
			- Store jobs
			- Deliver jobs
			- Handle retries
- 4) Event vs Command
	- Command
		- Tells someone what to do.
		- Examples: `CreateCluster`, `SendEmail`, `GenerateInvoice`
		- BullMQ commonly carries commands.
		  ```ts
		  queue.add("send-email");
		  ```
	- Event
		- Describes something that already happened.
		- Examples: `OrderCreated`, `UserRegistered`, `PaymentSucceeded`
		- Meaning: "This happened. Any interested consumer can react."
- 5) Work Queue vs Pub/Sub
	- Work Queue
		- One job is processed by one worker.
		- Example: generate PDF.
	- Pub/Sub
		- One event can be consumed by many subscribers.
		- Example: `OrderCreated` triggers email, analytics, notifications.
- 6) Queue patterns you actually use
	- Background processing
		- API enqueues work; worker processes later.
		- Common use cases
			- Emails
			- Notifications
			- Image processing
	- Fan out
		- One event triggers multiple independent consumers.
		- Example: order created -> email + analytics + fraud detection.
	- Pipeline
		- Work is split into stages.
		- Example
		  ```
		  Create Cluster -> Configure Network -> Create Nodes -> Health Check
		  ```
- 7) Benefits of message queues
	- Scalability: add more workers horizontally.
	- Reliability: jobs survive crashes and can be retried.
	- Decoupling: services remain independent.
	- Faster APIs: users do not wait for heavy background tasks.
	- Spike handling: queue buffers sudden load.
- 8) Trade-offs
	- Benefits
		- Async processing
		- Reliability
		- Scalability
		- Decoupling
	- Costs
		- More infrastructure
		- Harder debugging and tracing
		- Eventual consistency complexity
		- Retry/idempotency handling required
		- Monitoring and alerting required
- Interview one-liner
	- "A message queue decouples producers from consumers, smooths traffic spikes, improves reliability with retries, and enables async scaling at the cost of operational complexity and eventual consistency."