### SQL vs NoSQL (HLD Perspective)
collapsed:: true
	- ## Why This Topic Exists
		- In HLD, the question is not:
		  
		  "What database should I learn?"
		  
		  The question is:
		  
		  "What storage system best fits my requirements?"
		  
		  Every database choice is a tradeoff between:
		- Consistency
		- Scalability
		- Query flexibility
		- Relationships
		- Operational complexity
		  
		  ---
	- ## SQL Databases
		- Examples:
			- PostgreSQL
			- MySQL
			- Oracle
			- SQL Server
			  
			  ---
		- Core Philosophy
			- SQL databases optimize for:
				- Strong consistency
				- Transactions
				- Relationships
				- Complex queries
				  
				  ---
		- Best For
			- Systems where correctness is more important than scale.
			- Examples:
				- Banking
				- Payments
				- Inventory Management
				- Order Management
				- Financial Systems
				  
				  ---
		- Strengths
			- ### 1. Strong Consistency
			  
			  After a successful write:
			  
			  ```
			  All subsequent reads see latest data
			  ```
			  
			  Very important for financial systems.
			  
			  ---
			- ### 2. ACID Transactions
			  
			  Multiple operations succeed together or fail together.
			  
			  Example:
			  
			  ```
			  Transfer ₹100
			  
			  Debit Account A
			  Credit Account B
			  ```
			  
			  Never allow:
			  
			  ```
			  Debit Success
			  Credit Failed
			  ```
			  
			  ---
			- ### 3. Relationships
			  
			  Natural support for:
			  
			  ```
			  Users
			  |
			  Orders
			  |
			  Payments
			  ```
			  
			  using:
			- Foreign Keys
			- Joins
			  
			  ---
			- ### 4. Complex Queries
			  
			  Example:
			  
			  ```
			  Find top 10 users
			  with highest orders
			  during last month
			  ```
			  
			  SQL excels here.
			  
			  ---
		- Weaknesses
			- ### 1. Horizontal Scaling Is Harder
			  
			  As data grows:
			  
			  ```
			  1 Server
			  ```
			  
			  becomes:
			  
			  ```
			  10 Servers
			  ```
			  
			  Relationships and joins become harder across machines.
			  
			  ---
			- ### 2. Schema Changes
			  
			  Changing structure often requires migrations.
			  
			  Example:
			  
			  ```
			  Add New Column
			  ```
			  
			  must be coordinated.
			  
			  ---
	- # NoSQL Databases
		- Examples:
			- MongoDB
			- Cassandra
			- DynamoDB
			- Couchbase
			  
			  ---
		- Core Philosophy
			- NoSQL databases optimize for:
				- Scalability
				- Distribution
				- High throughput
				- Flexible data models
				  
				  ---
		- Best For
			- Systems where scale is more important than strict consistency.
			- Examples:
				- Social Media
				- Analytics
				- Event Logging
				- Recommendation Systems
				- User Activity Tracking
				  
				  ---
		- Strengths
			- ### 1. Easy Horizontal Scaling
			  
			  Data can be distributed across many machines.
			  
			  ```
			  Shard 1
			  Shard 2
			  Shard 3
			  Shard 4
			  ```
			  
			  Built with large-scale systems in mind.
			  
			  ---
			- ### Flexible Schema
			  
			  Documents may have different fields.
			  
			  Useful when data evolves rapidly.
			  
			  ---
			- ### 2. High Write Throughput
			  
			  Optimized for massive ingestion workloads.
			  
			  Examples:
			- User Events
			- Click Streams
			- Logs
			  
			  ---
			- ### 3. Natural Data Locality
			  
			  Frequently accessed data can often live together in one document.
			  
			  This reduces cross-server communication.
			  
			  ---
		- Weaknesses
			- ### 1. Relationships Are Harder
			  
			  NoSQL generally avoids joins.
			  
			  Developers often duplicate data.
			  
			  ---
			- ### 2. Complex Queries Can Be Harder
			  
			  Cross-entity analytics may become difficult.
			  
			  ---
			- ### 3. Consistency Tradeoffs
			  
			  Many NoSQL systems prioritize:
			  
			  ```
			  Availability
			  ```
			  
			  over:
			  
			  ```
			  Strong Consistency
			  ```
			  
			  during failures.
			  
			  ---
	- # Modern Reality
	  
	  A common misconception:
	  
	  ```
	  SQL = Transactions
	  
	  NoSQL = No Transactions
	  ```
	  
	  This is outdated.
	  
	  Modern MongoDB supports:
	- ACID Transactions
	- Multi-document Transactions
	  
	  However:
	  
	  SQL databases were designed around transactions from day one.
	  
	  NoSQL databases were designed around scale and distribution.
	  
	  ---
	- ## SQL vs NoSQL Decision Framework
		- Ask these questions:
		- ## Do I Need Strong Transactions?
		  
		  Example:
		  
		  ```
		  Bank Transfer
		  Payment System
		  ```
		  
		  Choose:
		  
		  ```
		  SQL
		  ```
		  
		  ---
		- ## Do I Need Complex Relationships?
		  
		  Example:
		  
		  ```
		  Users
		  Orders
		  Payments
		  Invoices
		  ```
		  
		  Choose:
		  
		  ```
		  SQL
		  ```
		  
		  ---
		- ## Do I Need Massive Horizontal Scale?
		  
		  Example:
		  
		  ```
		  Billions of Events
		  Millions of Writes/sec
		  ```
		  
		  Choose:
		  
		  ```
		  NoSQL
		  ```
		  
		  ---
		- ## Is Eventual Consistency Acceptable?
		  
		  Example:
		  
		  ```
		  Likes
		  Views
		  Recommendations
		  ```
		  
		  Choose:
		  
		  ```
		  NoSQL
		  ```
		  
		  ---
	- ## Real World Examples
		- ## Banking Application
		  
		  Storage:
		  
		  ```
		  SQL
		  ```
		  
		  Reason:
		  
		  ```
		  Transactions
		  Consistency
		  Relationships
		  ```
		  
		  ---
		- ## Instagram Likes
		  
		  Storage:
		  
		  ```
		  NoSQL
		  ```
		  
		  Reason:
		  
		  ```
		  Massive Scale
		  High Write Throughput
		  Eventual Consistency Acceptable
		  ```
		  
		  ---
		- ## E-Commerce Platform
		  
		  Most real systems use both.
		- ### Orders
		  
		  ```
		  SQL
		  ```
		  
		  Need:
		  
		  ```
		  Transactions
		  Consistency
		  ```
		  
		  ---
		- ### Product Catalog
		  
		  ```
		  NoSQL
		  ```
		  
		  Need:
		  
		  ```
		  Fast Reads
		  Flexible Data
		  Large Scale
		  ```
		  
		  ---
		- ### Event Logs
		  
		  ```
		  NoSQL
		  ```
		  
		  Need:
		  
		  ```
		  High Throughput
		  ```
		  
		  ---
- ### Database Replication
  collapsed:: true
	- ## Why Replication Exists
	  
	  Replication means maintaining multiple copies of the same data across multiple database servers.
	  
	  ```
	  Primary
	                |
	        ----------------
	        |              |
	        v              v
	     Replica A     Replica B
	  ```
	  
	  All nodes contain the same data.
	  
	  ---
	- ## Problems Without Replication
	  
	  Single Database:
	  
	  ```
	  Application
	     |
	     v
	  Database
	  ```
	  
	  Issues:
	- Single Point of Failure
	- Limited Read Capacity
	- Poor Disaster Recovery
	  
	  ---
	- ## 1. High Availability
	  
	  Suppose:
	  
	  ```
	  Primary Database 💥
	  ```
	  
	  fails.
	  
	  Without replication:
	  
	  ```
	  Entire Application Down
	  ```
	  
	  With replication:
	  
	  ```
	  Replica
	   ↓
	  Promoted to Primary
	  ```
	  
	  Application continues operating.
	  
	  ---
	- ## Key Idea
	  
	  Replication improves system availability by providing backup copies of data.
	  
	  ---
	- ## 2. Read Scaling
	  
	  Most systems are read-heavy.
	  
	  Example:
	  
	  ```
	  Instagram
	  
	  95% Reads
	  5% Writes
	  ```
	  
	  Without replication:
	  
	  ```
	  Reads
	  |
	  Primary
	  ```
	  
	  Primary becomes overloaded.
	  
	  ---
	  
	  With replication:
	  
	  ```
	  Primary
	                   |
	         ------------------
	         |       |        |
	         v       v        v
	  
	      R1       R2       R3
	  ```
	  
	  Reads are distributed across replicas.
	  
	  ---
	  
	  Benefits:
	- More throughput
	- Lower load on primary
	- Better scalability
	  
	  ---
	- ## 3. Disaster Recovery
	  
	  Suppose:
	  
	  ```
	  Primary Database Corrupted
	  ```
	  
	  or
	  
	  ```
	  Datacenter Failure
	  ```
	  
	  Replica still contains data.
	  
	  System can recover faster.
	  
	  ---
	- ## Primary-Replica Architecture
		- Most common replication architecture.
		- ## Architecture
		  
		  ```
		  Writes
		                    |
		                    v
		  
		              Primary DB
		                    |
		          --------------------
		          |                  |
		          v                  v
		  
		      Replica A         Replica B
		  
		          ↑                  ↑
		  
		         Reads             Reads
		  ```
		  
		  ---
		- ## Rules
			- ### Writes
			  
			  Always go to:
			  
			  ```
			  Primary
			  ```
			  
			  ---
			- ### Reads
			  
			  Can go to:
			  
			  ```
			  Primary
			  OR
			  Replica
			  ```
			  
			  depending on architecture.
			  
			  ---
		- ## Why Single Writer?
			- If multiple replicas accept writes:
			  
			  ```
			  Replica A = Adi
			  
			  Replica B = Aditya
			  ```
			  
			  Conflict occurs.
			  
			  Primary-Replica avoids this problem.
			  
			  ---
		- ## Read Replicas
			- Read Replicas are replicas used specifically for serving read traffic.
			- ## Example
			  
			  Without replicas:
			  
			  ```
			  1000 Reads/sec
			      |
			   Primary
			  ```
			  
			  ---
			  
			  With replicas:
			  
			  ```
			  1000 Reads/sec
			  
			  333 -> Replica A
			  333 -> Replica B
			  334 -> Replica C
			  ```
			  
			  ---
		- ## Benefits
			- ### Read Scaling
			  
			  Increase read capacity without increasing write complexity.
			  
			  ---
			- ### Reduced Primary Load
			  
			  Primary focuses on:
			  
			  ```
			  Writes
			  ```
			  
			  Replicas focus on:
			  
			  ```
			  Reads
			  ```
			  
			  ---
		- ## Common Use Cases
		- Profile Pages
		- Product Catalogs
		- Dashboards
		- Reporting Systems
		  
		  ---
	- ## Replication Logs
		- Replication is usually log-based.
		  
		  Primary records every change.
		  
		  Replicas replay those changes.
		  
		  ---
		- ## PostgreSQL
		  
		  Uses:
		  
		  ```
		  WAL
		  ```
		  
		  Write Ahead Log.
		  
		  ---
		- ## MySQL
		  
		  Uses:
		  
		  ```
		  Binlog
		  ```
		  
		  Binary Log.
		  
		  ---
		- ## MongoDB
		  
		  Uses:
		  
		  ```
		  Oplog
		  ```
		  
		  Operation Log.
		  
		  ---
		- ## Simplified Flow
		  
		  ```
		  Write
		  |
		  Primary
		  |
		  Log Entry Created
		  |
		  Replica Reads Log
		  |
		  Replica Replays Change
		  ```
		  
		  ---
	- ## Replication Lag
		- One of the most important concepts in distributed systems.
		- ## Definition
		  
		  Delay between:
		  
		  ```
		  Primary Updated
		  ```
		  
		  and
		  
		  ```
		  Replica Updated
		  ```
		  
		  ---
		- ## Example
		  
		  Timeline:
		  
		  ```
		  T=0ms
		  
		  Primary = Adi
		  ```
		  
		  ---
		  
		  ```
		  T=50ms
		  
		  Replica A = Adi
		  ```
		  
		  ---
		  
		  ```
		  T=100ms
		  
		  Replica B = Adi
		  ```
		  
		  ---
		  
		  During this window:
		  
		  ```
		  Primary = Adi
		  
		  Replica = Aditya
		  ```
		  
		  ---
		- ## Problems
		  
		  User updates profile.
		  
		  Immediately refreshes page.
		  
		  Request hits replica.
		  
		  User sees:
		  
		  ```
		  Old Data
		  ```
		  
		  ---
		  
		  This is the reason:
		- Eventual Consistency exists
		- Read-Your-Writes exists
		  
		  ---
	- ## Synchronous Replication
		- Primary waits for replica acknowledgement before returning success.
		- ## Flow
		  
		  ```
		  Write
		  |
		  Primary
		  |
		  Replica Updated
		  |
		  ACK
		  |
		  Success Response
		  ```
		  
		  ---
		- ## Benefits
			- ### Strong Consistency
			  
			  Replicas always contain latest data.
			  
			  ---
			- ### No Data Loss
			  
			  Primary and replicas stay synchronized.
			  
			  ---
		- ## Drawbacks
			- ### Higher Latency
			  
			  User waits for replicas.
			  
			  ---
			- ### Reduced Availability
			  
			  If replica is unreachable:
			  
			  ```
			  Write may fail
			  ```
			  
			  ---
		- ## Common Use Cases
			- Banking
			- Financial Systems
			- Critical Transactions
			  
			  ---
	- ## Asynchronous Replication
		- Most common replication strategy.
		- ## Flow
		  
		  ```
		  Write
		  |
		  Primary
		  |
		  Success Response
		  |
		  Replica Updated Later
		  ```
		  
		  ---
		- ## Benefits
			- ### Faster Writes
			  
			  User receives response immediately.
			  
			  ---
			- ### Better Availability
			  
			  Replica failures do not block writes.
			  
			  ---
			- ### Better Performance
			  
			  Primary is not waiting for replicas.
			  
			  ---
		- ## Drawbacks
			- ### Replication Lag
			  
			  Replicas may contain stale data.
			  
			  ---
			- ### Potential Data Loss
			  
			  Primary may fail before replicas receive latest updates.
			  
			  ---
		- ## Common Use Cases
			- Social Media
			- Analytics
			- User Profiles
			- Product Catalogs
			  
			  ---
	- ## Failover
		- Failover is the process of replacing a failed primary with a replica.
		- ## Before Failure
		  
		  ```
		  Primary
		   |
		  Replica A
		  Replica B
		  ```
		  
		  ---
		- ## Primary Failure
		  
		  ```
		  Primary 💥
		  ```
		  
		  ---
		- ## After Failover
		  
		  ```
		  Replica A
		  (New Primary)
		     |
		  Replica B
		  ```
		  
		  ---
		  
		  Application traffic is redirected to the new primary.
		  
		  ---
		- ## Goal
		  
		  Maintain availability despite failures.
		  
		  ---
		- ## Tradeoffs
			- ## Benefits
				- ### High Availability
				  
				  Application survives database failures.
				  
				  ---
				- ### Read Scaling
				  
				  Replicas handle large read traffic.
				  
				  ---
				- ### Disaster Recovery
				  
				  Additional copies of data exist.
				  
				  ---
				- ### Better Reliability
				  
				  Reduced risk of downtime.
				  
				  ---
			- ## Drawbacks
				- ### Replication Lag
				  
				  Replicas may be stale.
				  
				  ---
				- ### Increased Complexity
				  
				  Need:
				- Replication setup
				- Monitoring
				- Failover handling
				  
				  ---
				- ### Data Loss Risk
				  
				  Possible with asynchronous replication.
				  
				  ---
				- ### Higher Infrastructure Cost
				  
				  Multiple database servers required.
				  
				  ---
	- # Interview Cheat Sheet
		- ## Why Replication?
		- High Availability
		- Read Scaling
		- Disaster Recovery
		  
		  ---
		- ## Why Read Replicas?
		  
		  Increase read throughput without increasing write complexity.
		  
		  ---
		- ## What Is Replication Lag?
		  
		  Delay between primary update and replica update.
		  
		  ---
		- ## Synchronous vs Asynchronous
		- ### Synchronous
		  
		  Pros:
		- Strong Consistency
		- No Data Loss
		  
		  Cons:
		- Higher Latency
		- Lower Availability
		  
		  ---
		- ### Asynchronous
		  
		  Pros:
		- Faster
		- More Available
		  
		  Cons:
		- Replication Lag
		- Possible Data Loss
		  
		  ---
		- ## What Is Failover?
		  
		  Promotion of a replica to primary when primary fails.
		  
		  ---
		- # Key Takeaways
		- Replication keeps multiple copies of data.
		- Primary handles writes.
		- Replicas commonly handle reads.
		- Read replicas improve scalability.
		- Replication lag introduces eventual consistency.
		- Synchronous replication prioritizes consistency.
		- Asynchronous replication prioritizes performance and availability.
		- Failover improves system resilience.
		- Replication is the foundation for read scaling in modern systems.
- ### Database Sharding
  collapsed:: true
	- ## Why Sharding Exists
		- Replication solves read scaling and high availability.
		- Sharding is needed when a single primary still becomes the write bottleneck.
		- As traffic grows:
		  
		  ```
		  More Users
		    ↓
		  More Writes
		    ↓
		  Primary DB Bottleneck
		  ```
		- A single database eventually hits limits in:
			- CPU
			- Memory
			- Storage
			- Disk I/O
			- Network throughput
		- Sharding solves:
			- Write scaling
			- Storage scaling
		- It does this by distributing data across multiple database servers.
		  
		  ---
	- ## What Is Sharding
		- Sharding is the process of splitting data across multiple database servers.
		- Each shard stores only a subset of the total data.
		- Example:
		  
		  ```
		  Single Database
		  
		  Users 1 - 10M
		  ```
		  
		  Becomes:
		  
		  ```
		  Shard 1
		  Users 1 - 3M
		  
		  Shard 2
		  Users 3M - 6M
		  
		  Shard 3
		  Users 6M - 10M
		  ```
		- Result:
			- Storage is distributed
			- Writes are distributed
			- Load is distributed
			  
			  ---
	- ## Replication vs Sharding
		- Replication means same data, multiple copies.
		- Sharding means different data, different databases.
		- Replication is mainly for:
			- Read scaling
			- High availability
			- Disaster recovery
		- Sharding is mainly for:
			- Write scaling
			- Storage scaling
			  
			  ---
	- ## Primary Goal Of Sharding
		- Sharding exists because eventually one database cannot handle all writes.
		- Example:
		  
		  ```
		  100,000 Writes/sec
		  ```
		- Instead of upgrading a single server forever, we distribute the workload across multiple databases.
		  
		  ---
	- ## Shard Key
		- The shard key is the field used to decide which shard stores a record.
		- Without a shard key, the system cannot decide where data should go.
		- Example shard keys:
			- userId
			- tenantId
			- organizationId
			- customerId
		- Example:
		  
		  ```
		  Shard 1
		  Shard 2
		  Shard 3
		  ```
		  
		  Rule:
		  
		  ```
		  userId % 3
		  ```
		  
		  User 10 goes to Shard 1, user 11 goes to Shard 2, and user 12 goes to Shard 3.
		  
		  ---
	- ## Characteristics Of A Good Shard Key
		- ### 1. High Cardinality
			- Cardinality means the number of unique values.
			- Good shard keys:
				- userId
				- organizationId
				- customerId
			- Bad shard keys:
				- gender
				- status
				- country
		- ### 2. Even Distribution
			- Goal:
			  
			  ```
			  Shard 1 = 33%
			  
			  Shard 2 = 33%
			  
			  Shard 3 = 34%
			  ```
			- Avoid:
			  
			  ```
			  Shard 1 = 90%
			  
			  Shard 2 = 5%
			  
			  Shard 3 = 5%
			  ```
		- ### 3. Matches Query Patterns
			- If most requests are for all clusters of a specific organization, then organizationId is a good shard key.
			  
			  ---
	- ## Sharding Strategies
		- ### Range-Based Sharding
			- Data is divided by ranges.
			- Example:
			  
			  ```
			  1 - 1M     → Shard 1
			  
			  1M - 2M    → Shard 2
			  
			  2M - 3M    → Shard 3
			  ```
			- Advantages:
				- Easy to understand
				- Easy to implement
			- Disadvantages:
				- Can create hot shards
				- If IDs continuously increase, the latest shard gets most writes
		- ### Hash-Based Sharding
			- A hash function determines the shard.
			- Example:
			  
			  ```
			  hash(userId) % 3
			  ```
			- Result:
			  
			  ```
			  User 1 → Shard 1
			  
			  User 2 → Shard 2
			  
			  User 3 → Shard 3
			  
			  User 4 → Shard 1
			  ```
			- Advantages:
				- Better distribution
				- Reduced hot shards
				- Better write balancing
			- Disadvantages:
				- Harder rebalancing
				- Range queries become difficult
				  
				  ---
	- ## Shard Router
		- Applications usually do not manually determine shards.
		- A router handles it.
		- Responsibilities:
			- Route reads
			- Route writes
			- Determine the target shard
			  
			  ```
			  Application
			    |
			  Shard Router
			    |
			  -------------------
			  |        |        |
			  v        v        v
			  
			  S1       S2       S3
			  ```
			  
			  ---
	- ## Hot Shards
		- A hot shard receives significantly more traffic than the others.
		- Example:
		  
		  ```
		  Shard 1
		  95% Traffic
		  
		  Shard 2
		  3% Traffic
		  
		  Shard 3
		  2% Traffic
		  ```
		- This usually indicates a bad shard key.
		- Why hot shards are dangerous:
			- Even if most shards are healthy, one overloaded shard can become the bottleneck for the entire system.
			  
			  ---
	- ## Cross-Shard Queries
		- Before sharding, queries are simple because everything is in one database.
		- After sharding, the system must know which shard contains the data.
		- If it does not know, it must query multiple shards and merge results.
		  
		  ```
		  Query Shard 1
		  
		  Query Shard 2
		  
		  Query Shard 3
		  ```
		  
		  ---
	- ## Cross-Shard Joins
		- Example:
		  
		  ```
		  Users → Shard 1
		  
		  Orders → Shard 2
		  ```
		- Now a join between users and orders requires communication between multiple databases.
		- This is much more complex than a single-database join.
		  
		  ---
	- ## Rebalancing
		- Rebalancing is the process of moving data when the number of shards changes.
		- Example:
		  
		  ```
		  3 Shards
		  ```
		  
		  becomes:
		  
		  ```
		  4 Shards
		  ```
		- Data must be moved from one shard to another.
		- Why rebalancing is difficult:
			- Large data movement
			- Operational risk
			- Possible performance impact
			- Temporary system instability
			  
			  ---
	- ## Real World Example
		- Imagine a Kubernetes provisioning platform growing to 50 million clusters.
		- A single database can no longer handle all writes.
		- You choose organizationId as the shard key.
		- Hash(organizationId) determines the shard.
		- Benefits:
			- Writes are distributed evenly
			- Better scalability
			- Faster organization-level queries
			  
			  ---
	- ## Tradeoffs
		- ### Advantages
			- Write scaling
			- Storage scaling
			- Horizontal growth
			- Better throughput
			- Supports massive datasets
		- ### Disadvantages
			- Operational complexity
			- Rebalancing challenges
			- Cross-shard queries
			- Cross-shard joins
			- Hot shards
			- Shard key selection becomes critical
			  
			  ---
	- ## Interview Cheat Sheet
		- ### Replication Solves
			- Read scaling
			- High availability
		- ### Sharding Solves
			- Write scaling
			- Storage scaling
		- ### Most Important Design Decision
		  
		  ```
		  Choosing The Shard Key
		  ```
		- ### Good Shard Keys
			- userId
			- tenantId
			- organizationId
			- customerId
		- ### Common Problems
			- Hot shards
			- Cross-shard queries
			- Cross-shard joins
			- Rebalancing
			  
			  ---
	- ## Key Takeaways
		- Sharding distributes data across multiple databases.
		- It primarily solves write scaling and storage scaling.
		- The shard key determines where data is stored.
		- A bad shard key can create hot shards and uneven distribution.
		- Hash-based sharding generally provides better distribution than range-based sharding.
		- Sharding improves scalability but significantly increases system complexity.
		- Replication and sharding solve different problems and are commonly used together in large-scale systems.
-