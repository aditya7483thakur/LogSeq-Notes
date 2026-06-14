### Scalability (vertical vs horizontal)
	- #### What is scalability?
	  
	  **Ability of a system to handle increasing load without breaking.**
	  
	  Example:
	- 100 users → works fine
	- 1,000,000 users → crashes ❌
	  
	  A scalable system:
	- Handles growth
	- Maintains performance
	  
	  Types of scaling
	  
	  1. Vertical scaling (Scale Up)
		- Increase power of a single machine
		- Example: 8GB RAM → 64GB RAM, 4 CPU → 32 CPU
		- Pros: Simple, no code changes
		- Cons: Finite limit, expensive, single point of failure
		  
		  2. Horizontal scaling (Scale Out)
		- Add more machines
		- Example: 1 server → 10 servers → 100 servers
		- Pros: Near-infinite scaling, fault tolerant, industry standard
		- Cons: More complex, requires distributed system design
		  
		  ---
	- ## Interview insight (very important)
	- If you say “I will scale vertically” — interviewer may think beginner ❌
	- If you say “We scale horizontally with load balancing” — shows stronger understanding ✅
- ### Latency vs Throughput
	- #### Latency
	  Time taken for a single request to complete (e.g., API response time = 100 ms).
	- Think: “How fast is one request?”
	-
	- #### Throughput
	  Number of requests handled per unit time (e.g., 10,000 requests/sec).
	- Think: “How many requests can the system handle?”
	-
	- ### Key difference
	  
	  | Latency | Throughput |
	  | --- | --- |
	  | Time per request | Requests per second |
	  | Focus: speed | Focus: capacity |
	- ### Trade-offs
	  
	  You often cannot optimize both latency and throughput simultaneously.
	  
	  Example:
	- Case 1: Fast but limited — Latency: 50 ms ✅, Throughput: 100 RPS ❌
	- Case 2: High capacity but slower — Latency: 300 ms ❌, Throughput: 10,000 RPS ✅
	  
	  ---
- ### CAP theorem
	- In a distributed system, during a network partition you must choose between Consistency (C) and Availability (A). Partition tolerance (P) is mandatory.
	- ### Key terms
		- Consistency (C): All nodes return the same data at the same time. No stale data; strong guarantee. May reject requests during uncertainty.
		- Availability (A): System always responds; responses may be stale temporarily.
		- Partition tolerance (P): System continues to operate even if network between nodes fails.
		  
		  **Core rule:** During a partition, choose either Consistency OR Availability.
	- ### CP vs AP
		- CP (Consistency + Partition tolerance)
			- Guarantees synchronized data
			- May reject requests during partitions
			- Use when correctness is critical (e.g., banking, payments)
		- AP (Availability + Partition tolerance)
			- Always responds; may return temporarily inconsistent data
			- Uses eventual consistency
			- Use when availability is more important (e.g., social feeds, messaging)
	- #### Important clarification
	  
	  Consistency ≠ correctness of data; it means the same data across all nodes at the same time.
	- #### Eventual consistency
	  
	  Data becomes consistent after some time (e.g., message delivered later, like count updates after delay).
	  
	  ---
	- ## Interview thinking
	  
	  When designing systems, decide based on:
	- Is failure acceptable?
	- Is stale data acceptable?
	  
	  Example decisions:
	  
	  | System | Choice | Reason |
	  | --- | --- | --- |
	  | Banking | CP | Cannot show wrong balance |
	  | WhatsApp | AP | Message delay OK, failure not |
	  | Food ordering | CP | Duplicate/wrong orders not allowed |
	  | Restaurant listing | AP | Slightly outdated data OK |
	  
	  ---
-