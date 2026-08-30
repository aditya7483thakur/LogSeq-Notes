### SQL vs NoSQL (HLD Perspective)
collapsed:: true
	- ### Why This Topic Exists
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
		- Examples:
		  collapsed:: true
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
	- ### NoSQL Databases
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
	- ### Modern Reality
		- A common misconception:
		  
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
	- ### SQL vs NoSQL Decision Framework
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
	- ### Real World Examples
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
	- ### Why Replication Exists
	  collapsed:: true
		- Replication means maintaining multiple copies of the same data across multiple database servers.
		  
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
	- ### Problems Without Replication
	  collapsed:: true
		- Single Database:
		  
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
	- ### Primary-Replica Architecture
	  collapsed:: true
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
	- ### Replication Logs
	  collapsed:: true
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
	- ### Replication Lag
	  collapsed:: true
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
	- ### Synchronous Replication
	  collapsed:: true
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
	- ### Asynchronous Replication
	  collapsed:: true
		- Most common replication strategy.
		- ### Flow
		  
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
		- ### Benefits
			- ### Faster Writes
			  
			  User receives response immediately.
			  
			  ---
			- ### Better Availability
			  
			  Replica failures do not block writes.
			  
			  ---
			- ### Better Performance
			  
			  Primary is not waiting for replicas.
			  
			  ---
		- ### Drawbacks
		  collapsed:: true
			- ### Replication Lag
			  
			  Replicas may contain stale data.
			  
			  ---
			- ### Potential Data Loss
			  
			  Primary may fail before replicas receive latest updates.
			  
			  ---
		- ### Common Use Cases
		  collapsed:: true
			- Social Media
			- Analytics
			- User Profiles
			- Product Catalogs
			  
			  ---
	- ### Failover
	  collapsed:: true
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
	- ### Interview Cheat Sheet
	  collapsed:: true
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
	- ### Why Sharding Exists
	  collapsed:: true
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
	- ### What Is Sharding
	  collapsed:: true
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
	- ### Replication vs Sharding
	  collapsed:: true
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
	- ### Primary Goal Of Sharding
	  collapsed:: true
		- Sharding exists because eventually one database cannot handle all writes.
		- Example:
		  
		  ```
		  100,000 Writes/sec
		  ```
		- Instead of upgrading a single server forever, we distribute the workload across multiple databases.
		  
		  ---
	- ### Shard Key
	  collapsed:: true
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
	- ### Characteristics Of A Good Shard Key
	  collapsed:: true
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
	- ### Sharding Strategies
	  collapsed:: true
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
	- ### Shard Router
	  collapsed:: true
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
	- ### Hot Shards
	  collapsed:: true
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
	- ### Cross-Shard Queries
	  collapsed:: true
		- Before sharding, queries are simple because everything is in one database.
		- After sharding, the system must know which shard contains the data.
		- If it does not know, it must query multiple shards and merge results.
		  
		  ```
		  Query Shard 1
		  
		  Query Shard 2
		  
		  Query Shard 3
		  ```
		  
		  ---
	- ### Cross-Shard Joins
	  collapsed:: true
		- Example:
		  
		  ```
		  Users → Shard 1
		  
		  Orders → Shard 2
		  ```
		- Now a join between users and orders requires communication between multiple databases.
		- This is much more complex than a single-database join.
		  
		  ---
	- ### Rebalancing
	  collapsed:: true
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
	- ### Real World Example
	  collapsed:: true
		- Imagine a Kubernetes provisioning platform growing to 50 million clusters.
		- A single database can no longer handle all writes.
		- You choose organizationId as the shard key.
		- Hash(organizationId) determines the shard.
		- Benefits:
			- Writes are distributed evenly
			- Better scalability
			- Faster organization-level queries
			  
			  ---
	- ### Tradeoffs
	  collapsed:: true
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
	- ### Interview Cheat Sheet
	  collapsed:: true
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
	- ### Key Takeaways
	  collapsed:: true
		- Sharding distributes data across multiple databases.
		- It primarily solves write scaling and storage scaling.
		- The shard key determines where data is stored.
		- A bad shard key can create hot shards and uneven distribution.
		- Hash-based sharding generally provides better distribution than range-based sharding.
		- Sharding improves scalability but significantly increases system complexity.
		- Replication and sharding solve different problems and are commonly used together in large-scale systems.
- ### Database Partitioning
  collapsed:: true
	- ### Why Partitioning Exists
	  collapsed:: true
		- As data grows, a single table can become extremely large.
		- Example:
		  
		  ```
		  Orders Table
		  
		  1 Billion Rows
		  ```
		- Problems:
			- Queries become slower
			- Indexes become larger
			- Maintenance becomes difficult
			- Backups take longer
			- Data management becomes harder
		- Partitioning solves these problems by dividing a large table into smaller pieces while still appearing as a single table to applications.
		  
		  ---
	- ### What Is Partitioning
	  collapsed:: true
		- Partitioning is the process of splitting a large table into smaller logical parts called partitions.
		- Example:
		  
		  ```
		  Orders Table
		  (1 Billion Rows)
		  ```
		  
		  Becomes:
		  
		  ```
		  Orders
		  
		  ├── Partition 2023
		  ├── Partition 2024
		  └── Partition 2025
		  ```
		- Applications still query the same table name, but internally the database accesses only relevant partitions.
		  
		  ---
	- ### Benefits Of Partitioning
	  collapsed:: true
		- ### Faster Queries
			- Instead of scanning 1 billion rows, the database may scan only the relevant partition.
		- ### Easier Maintenance
			- Backups, archiving, and deleting old data can be performed on specific partitions.
		- ### Better Manageability
			- Large datasets become easier to organize.
			  
			  ---
	- ### Partition Key
	  collapsed:: true
		- A partition key determines which partition stores a row.
		- Common examples:
			- created_at
			- user_id
			- country
			- status
		- Choosing the right partition key is critical.
		  
		  ---
	- ### Characteristics Of A Good Partition Key
	  collapsed:: true
		- ### 1. Matches Query Patterns
			- Example query pattern:
			  
			  ```
			  WHERE created_at >= '2025-01-01'
			  ```
			- Good partition key:
				- created_at
		- ### 2. Distributes Data Reasonably
			- Avoid extremely uneven partition sizes.
		- ### 3. Is Frequently Used In Filters
			- The database can skip irrelevant partitions when the partition key appears in queries.
			  
			  ---
	- ### Partitioning Strategies
	  collapsed:: true
		- ### Horizontal Partitioning
			- Most common form of partitioning.
			- Rows are split across partitions.
			- Example:
			  
			  ```
			  Orders
			  
			  Partition 1
			  Rows 1 - 1M
			  
			  Partition 2
			  Rows 1M - 2M
			  
			  Partition 3
			  Rows 2M - 3M
			  ```
			- All partitions have the same columns.
		- ### Vertical Partitioning
			- Columns are split across tables.
			- Example:
			  
			  ```
			  Users
			  
			  id
			  name
			  email
			  bio
			  profile_picture
			  preferences
			  ```
			  
			  Becomes:
			  
			  ```
			  Users_Basic
			  
			  id
			  name
			  email
			  
			  Users_Profile
			  
			  id
			  bio
			  profile_picture
			  preferences
			  ```
			- Benefits:
				- Smaller rows
				- Faster common queries
				- Better cache efficiency
		- ### Range Partitioning
			- Data is divided using ranges.
			- Example:
			  
			  ```
			  2023 → Partition 1
			  
			  2024 → Partition 2
			  
			  2025 → Partition 3
			  ```
			- Advantages:
				- Easy to understand
				- Excellent for time-series data
			- Disadvantages:
				- Can create uneven partitions
				- Latest partition may receive most writes
		- ### Hash Partitioning
			- A hash function determines the partition.
			- Example:
			  
			  ```
			  hash(userId) % 4
			  ```
			- Result:
			  
			  ```
			  User 1 → Partition 1
			  
			  User 2 → Partition 2
			  
			  User 3 → Partition 3
			  
			  User 4 → Partition 4
			  ```
			- Advantages:
				- Better distribution
				- Reduces hotspots
			- Disadvantages:
				- Harder debugging
				- Range queries become difficult
		- ### List Partitioning
			- Data is partitioned based on predefined values.
			- Example:
			  
			  ```
			  India → Partition 1
			  
			  USA → Partition 2
			  
			  UK → Partition 3
			  ```
			- Advantages:
				- Easy to understand
				- Useful for regional datasets
			- Disadvantages:
				- Uneven distribution can occur
				  
				  ---
	- ### Partition Pruning
	  collapsed:: true
		- Partition pruning is one of the biggest benefits of partitioning.
		- The database skips irrelevant partitions.
		- Without partitioning:
		  
		  ```
		  SELECT *
		  FROM orders
		  WHERE year = 2025
		  ```
		  
		  Database scans all data.
		- With partitioning:
		  
		  ```
		  Orders_2023
		  Orders_2024
		  Orders_2025
		  ```
		  
		  Database only scans Orders_2025.
		  
		  ---
	- ### PostgreSQL Example
	  collapsed:: true
		- Create a partitioned table:
		  
		  ```sql
		  CREATE TABLE orders (
		   order_id BIGINT,
		   order_date DATE,
		   amount DECIMAL
		  ) PARTITION BY RANGE (order_date);
		  ```
		- Create partitions:
		  
		  ```sql
		  CREATE TABLE orders_2024 PARTITION OF orders FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');
		  CREATE TABLE orders_2025 PARTITION OF orders FOR VALUES FROM ('2025-01-01') TO ('2026-01-01');
		  ```
		- Insert rows:
		  
		  ```sql
		  INSERT INTO orders VALUES (1, '2024-06-10', 1000);
		  INSERT INTO orders VALUES (2, '2025-02-10', 2000);
		  ```
		- The database automatically routes rows to the correct partition:
		  
		  ```
		  (1, 2024-06-10) -> orders_2024
		  (2, 2025-02-10) -> orders_2025
		  ```
		- Query the parent table:
		  
		  ```sql
		  SELECT *
		  FROM orders
		  WHERE order_date >= '2024-01-01'
		   AND order_date < '2025-01-01';
		  ```
		- The optimizer reads only orders_2024.
		  
		  ---
	- ### Other Partition Types
	  collapsed:: true
		- ### Range Partitioning
			- `PARTITION BY RANGE (order_date)`
			- Best for time-based or ordered data.
			- Example:
			  
			  ```
			  2020-2022 -> Partition A
			  2023-2025 -> Partition B
			  ```
		- ### List Partitioning
			- `PARTITION BY LIST (country)`
			- Best when rows belong to a known set of categories.
			- Example:
			  
			  ```
			  India -> Partition India
			  USA -> Partition USA
			  UK -> Partition UK
			  ```
		- ### Hash Partitioning
			- `PARTITION BY HASH (customer_id)`
			- The database distributes rows using a hash function.
			- Example:
			  
			  ```
			  hash(customer_id) % 4
			  ```
			- Useful when data is not naturally divided by date or category.
			  
			  ---
	- ### Partitioning Vs Sharding
	  collapsed:: true
		- This is a very common interview question.
		- | Aspect | Partitioning | Sharding |
		  |---|---|---|
		  | Data placement | Inside one database | Across multiple databases |
		  | Main goal | Performance and manageability | Write scaling and storage scaling |
		  | Server count | One database server | Multiple database servers |
		  | Operational impact | Lower complexity | Higher complexity |
		  | Typical use case | Large tables in one system | Very large distributed systems |
		- Technically, sharding is a specialized form of partitioning where partitions are distributed across multiple servers.
		  
		  ---
	- ### Real World Examples
	  collapsed:: true
		- ### E-Commerce Orders
			- Partition by created_at, usually by year.
			- Example:
			  
			  ```
			  Orders_2023
			  Orders_2024
			  Orders_2025
			  ```
			- Example query:
			  
			  ```sql
			  SELECT *
			  FROM orders
			  WHERE created_at >= '2025-01-01'
			   AND created_at < '2026-01-01';
			  ```
			- This query lets the database scan only the 2025 partition.
		- ### Logs
			- Partition by date.
			- Most log systems use time-based partitions.
		- ### Analytics Systems
			- Partition by event date to improve reporting performance.
		- ### Banking Transactions
			- Partition by transaction date for efficient historical queries.
			  
			  ---
	- ### Tradeoffs
	  collapsed:: true
		- ### Advantages
			- Faster queries
			- Partition pruning
			- Easier maintenance
			- Better archival strategy
			- Improved manageability
		- ### Disadvantages
			- More operational complexity
			- Poor partition key can hurt performance
			- Cross-partition queries may become expensive
			- Repartitioning can be difficult
			  
			  ---
	- ### Interview Cheat Sheet
	  collapsed:: true
		- ### Why Partitioning?
			- Large tables become difficult to manage
			- Improves query performance
			- Enables partition pruning
		- ### Types Of Partitioning
			- Horizontal: split rows
			- Vertical: split columns
			- Range: partition by ranges
			- Hash: partition using hash functions
			- List: partition using predefined values
		- ### What Is Partition Pruning?
			- Database scans only relevant partitions instead of the entire dataset.
			  
			  ---
	- ### Key Takeaways
	  collapsed:: true
		- Partitioning splits a large table into smaller logical pieces.
		- It primarily improves performance and manageability.
		- Choosing the correct partition key is critical.
		- Partition pruning is the major performance benefit.
		- Horizontal partitioning splits rows, and vertical partitioning splits columns.
		- Sharding is a distributed form of partitioning.
		- Partitioning solves performance problems, while sharding solves scalability problems.
		- Partitioning and sharding are often used together in large-scale systems.
- ### Database Indexing
  collapsed:: true
	- ### Introduction to Indexing
	  collapsed:: true
		- ## 1. Why Indexing Exists
			- As database tables grow larger, searching for data becomes slower.
			  
			  Without an efficient lookup mechanism, the database may need to examine every row to find the required data.
			  
			  Indexing exists to make data retrieval faster by allowing the database to quickly locate rows instead of scanning the entire table.
			  
			  ---
		- ## 2. Full Table Scan
			- A **Full Table Scan** (or Sequential Scan) occurs when the database checks every row in a table to find the requested data.
			  
			  Example:
			  
			  ```
			  SELECT *
			  FROM users
			  WHERE email = 'john@example.com';
			  ```
			  
			  Without an index, the database scans rows one by one until it finds a match.
			  
			  For small tables this is acceptable, but for large tables it becomes expensive and increases query latency.
			  
			  ---
		- ## 3. What is an Index?
			- An **Index** is a separate data structure maintained by the database that stores indexed column values along with references to the corresponding rows.
			  
			  Instead of searching the entire table, the database searches the index first and then directly retrieves the required row.
			  
			  ---
		- ## 4. Index vs Table
			- ### Table
			  
			  Stores the complete data.
			  
			  Example:
			  
			  ```
			  id
			  name
			  email
			  phone
			  address
			  ```
			- ### Index
			  
			  Stores:
			- Indexed column(s)
			- Reference (Pointer) to the actual row
			  
			  The index is much smaller than the table, making it significantly faster to search.
			  
			  ---
		- ## 5. What is a Pointer?
			- A **Pointer** (also called a **Row Reference**) tells the database where the actual row is stored.
			  
			  Conceptually:
			  
			  ```
			  amit@gmail.com
			      ↓
			  Pointer
			      ↓
			  Actual Row
			  ```
			  
			  Internally, this pointer is typically a reference to the row's physical location (such as its disk location or an internal row identifier).
			  
			  You don't need to know its exact implementation—just think of it as the database saying: 
			  
			  **"The data you're looking for is stored over here."**
			  
			  ---
		- ## 6. How Indexes Improve Performance
			- Without an index:
			  
			  ```
			  Query
			   ↓
			  Scan Entire Table
			   ↓
			  Find Matching Row
			  ```
			  
			  With an index:
			  
			  ```
			  Query
			   ↓
			  Search Index
			   ↓
			  Follow Pointer
			   ↓
			  Fetch Matching Row
			  ```
			  
			  Instead of checking every row, the database uses the index to quickly locate the required data, significantly reducing query time.
			  
			  ---
		- ## 7. Read Path: Without vs With an Index
			- ### Without an Index
			  
			  ```
			  Application
			      │
			      ▼
			  Database
			      │
			      ▼
			  Full Table Scan
			      │
			      ▼
			  Return Matching Row
			  ```
			- ### With an Index
			  
			  ```
			  Application
			      │
			      ▼
			  Database
			      │
			      ▼
			  Search Index
			      │
			      ▼
			  Follow Pointer
			      │
			      ▼
			  Fetch Row
			      │
			      ▼
			  Return Result
			  ```
	- ### Tradeoffs & Best Practices
	  id:: 6a5ceaa9-9883-4923-9924-f928d0080224
	  collapsed:: true
		- ## 1. Read vs Write Tradeoff
			- Indexes significantly improve **read performance** by allowing the database to quickly locate rows.
			  
			  However, every **INSERT**, **UPDATE**, or **DELETE** operation must also update the corresponding indexes.
			  
			  Example:
			  
			  ```
			  INSERT INTO users (...)
			  ```
			  
			  The database must:
			  
			  ```
			  1. Insert the row into the table
			  2. Update every affected index
			  ```
			  
			  Therefore:
			- **More Indexes → Faster Reads**
			- **More Indexes → Slower Writes**
			  
			  Choosing the right number of indexes is a balance between read and write performance.
			  
			  ---
		- ## 2. Storage Overhead
			- Indexes are stored separately from the table and consume additional disk space.
			  
			  For every indexed column, the database maintains an extra data structure.
			  
			  Example:
			  
			  ```
			  Users Table
			      +
			  Email Index
			      +
			  Phone Index
			      +
			  Created_at Index
			  ```
			  
			  As the number of indexes increases:
			- Storage usage increases.
			- Backup size increases.
			- Index maintenance cost increases.
			  
			  ---
		- ## 3. Index Maintenance
			- Indexes are automatically maintained by the database.
			  
			  Whenever data changes, the corresponding indexes must also be updated.
			  
			  Operations that require index maintenance:
			- INSERT
			- UPDATE
			- DELETE
			  
			  Developers do not manually update indexes, but they should understand that maintaining many indexes increases write overhead.
			  
			  ---
		- ## 4. When to Create an Index
			- Indexes are most beneficial for columns that are frequently used in:
			- ### Filtering
			  
			  ```
			  WHERE email = ?
			  ```
			- ### Joins
			  
			  ```
			  JOIN users
			  ON users.id = orders.user_id
			  ```
			- ### Sorting
			  
			  ```
			  ORDER BY created_at
			  ```
			- ### Grouping
			  
			  ```
			  GROUP BY category
			  ```
			  
			  In general, create indexes on columns that are queried frequently.
			  
			  ---
		- ## 5. When NOT to Create an Index
			- Avoid creating indexes when:
				- ### The table is very small
				  A full table scan is often faster than using an index.
				  
				  ---
				- ### The column has very few unique values
				  
				  Example:
				  
				  ```
				  gender
				  
				  Male
				  Female
				  ```
				  
				  Since a large portion of the table matches each value, the index provides little benefit.
				  
				  *(We'll later learn this concept as **Low Selectivity**.)*
				  
				  ---
				- ### The column is rarely queried
				  
				  Creating an index that is almost never used only wastes storage and slows down writes.
				  
				  ---
				- ### The table experiences heavy write traffic
				  
				  If a table receives frequent inserts or updates but very few reads, excessive indexing can reduce overall performance.
	- ### B+ Tree & Index Internals
	  collapsed:: true
		- ### 1. Database Pages & Index Storage
		  collapsed:: true
			- ### What is a Page?
			  collapsed:: true
				- A page is a fixed-size block of storage that a database uses as the basic unit for reading and writing data.
				- Instead of reading one row at a time from disk, the database reads a page containing multiple rows or index entries.
				- Disk / SSD
				- ```
				      ↓
				  Database reads a Page
				      ↓
				  Page contains multiple rows / index entries
				  ```
				- Conceptually:
				- Page
				- ```
				  ┌─────────────────────────────┐
				  │ Row 1                      │
				  │ Row 2                      │
				  │ Row 3                      │
				  │ Row 4                      │
				  │ ...                        │
				  └─────────────────────────────┘
				  ```
				- A page is also the unit commonly cached in memory inside the database's buffer pool/cache.
				- Database performance often depends heavily on how many pages need to be accessed.
			- ### Table Pages vs Index Pages
			  collapsed:: true
				- ### Table Pages
					- Table pages contain the actual table data.
					- Table Page
					- ```
					  ┌──────────────────────────────┐
					  │ id: 1 | name: Aditya         │
					  │ id: 2 | name: Amit           │
					  │ id: 3 | name: Rahul          │
					  └──────────────────────────────┘
					  ```
				- ### Index Pages
					- Index pages contain indexed values and information that helps locate the actual row.
					- Conceptually:
					- Index Page
					- ```
					  email                  → Row Reference
					  amit@gmail.com         → Row X
					  rahul@gmail.com        → Row Y
					  ```
					- The database can:
					- Search Index Page
					- ```
					          ↓
					  Find Indexed Value
					          ↓
					  Get Row Reference
					          ↓
					  Locate Actual Table Row
					  ```
			- ### Row Pointers / References
			  collapsed:: true
				- An index usually does not need to store the complete row.
				- Instead, it stores something that tells the database where the actual row can be found.
				- Conceptually:
				- amit@gmail.com
				- ```
				         ↓
				  Row Reference
				         ↓
				  Actual User Row
				  ```
				- For example:
				- Index
				- email = amit@gmail.com
				- ```
				              ↓
				         Row Reference
				              ↓
				         Table Page
				              ↓
				         Actual Row
				  ```
				- The exact implementation depends on the database and storage engine.
				- It may be:
				- A physical row location
				- A row identifier
				- A primary key value
				- Another internal record reference
				- For understanding indexes, think of it as:
				- "The data you are looking for is over here."
			- ### Why Page Access Matters
			  collapsed:: true
				- Accessing data within a page already loaded into memory is relatively cheap.
				- Accessing a page that is not in memory may require loading it from storage.
				- Conceptually:
				- Best case:
				- Page already in memory
				- ```
				          ↓
				  Fast access
				  More expensive case:
				  ```
				- Page not in memory
				- ```
				          ↓
				  Storage access
				          ↓
				  Load Page into memory
				          ↓
				  Process data
				  ```
				- Therefore, the major goal of a database index is often:
				- Find the required data using as few page accesses as possible.
				- This is the central reason behind B+ Trees.
		- ### 2. Why Tree-Based Indexes Exist
		  collapsed:: true
			- ### Single-Level Index
			  collapsed:: true
				- A simple index can conceptually look like this:
				- Index
				- ```
				  10  → Page A
				  20  → Page B
				  30  → Page C
				  40  → Page D
				  50  → Page E
				  ```
				- For a small dataset, this is manageable.
				- But as the table grows:
				- More Data
				- ```
				      ↓
				  More Index Entries
				      ↓
				  Index becomes large
				  ```
				- Eventually, the entire index cannot fit inside one page.
			- ### Index Becomes Large
			  collapsed:: true
				- Suppose the index itself spans multiple pages:
				- ```
				  Index Page 1 → Keys 1–1000
				  Index Page 2 → Keys 1001–2000
				  Index Page 3 → Keys 2001–3000
				  ...
				  ```
				- Now suppose we search for:
				- 2500
				- The database now has a new problem:
				- Which index page contains 2500?
				- Searching through index pages one by one would again become inefficient.
			- ### Multilevel Indexing
			  collapsed:: true
				- The solution is to create another level that helps locate the correct index page.
				- ```
				                Top-Level Index
				                       ↓
				          ┌────────────┼────────────┐
				          ↓            ↓            ↓
				     Index Page 1  Index Page 2  Index Page 3
				          ↓            ↓            ↓
				       Data Pages    Data Pages    Data Pages
				  ```
				- Example:
				- Top-Level Index
				- ```
				  1000 → Index Page 1
				  2000 → Index Page 2
				  3000 → Index Page 3
				  ```
				- Searching for 2500:
				- Top-Level Index
				- ```
				          ↓
				  2500 belongs between 2000 and 3000
				          ↓
				  Correct Index Page
				          ↓
				  Correct Data Page
				          ↓
				  Actual Row
				  ```
				- This is multilevel indexing.
			- ### Need for Fast Page Navigation
			  collapsed:: true
				- As data grows:
				- Data grows
				- ```
				      ↓
				  Index grows
				      ↓
				  Need multiple index levels
				      ↓
				  Need efficient navigation between pages
				  ```
				- A tree structure naturally solves this:
				- Root Page
				- ```
				      ↓
				  Internal Index Page
				      ↓
				  Internal Index Page
				      ↓
				  Leaf Page
				  ```
				- The goal is:
				- Use each page to eliminate a large portion of the search space.
				- This leads us to tree-based structures.
		- ### 3. Evolution: BST → M-Way Tree → B-Tree
		  collapsed:: true
			- ### Why Not BST?
			  collapsed:: true
				- A Binary Search Tree has at most two children per node.
				- ```
				          50
				         /  \
				       30    70
				      / \    / \
				     20 40  60 80
				  ```
				- For in-memory algorithms, BSTs can be useful.
				- But databases work with pages containing many entries, not individual memory objects.
			- ### Low Fan-out and Tree Height
			  collapsed:: true
				- A BST has:
				- Maximum Children = 2
				- A database page can contain many keys and pointers.
				- A B+ Tree node can conceptually have:
				- [10 | 20 | 30 | 40 | 50 | 60]
				- P1   P2   P3   P4   P5   P6   P7
				- This gives:
			- ### High Fan-out
			  collapsed:: true
				- Fan-out means:
				- The number of children a node can have.
				- Comparison:
				- BST
				- Fan-out = 2
				- ```
				          ↓
				  More levels required
				          ↓
				  Potentially more page accesses
				  B+ Tree
				  ```
				- High Fan-out
				- ```
				          ↓
				  Fewer levels
				          ↓
				  Fewer page accesses
				  ```
			- ### Why BST Is Not Ideal for Database Pages
			  collapsed:: true
				- A BST conceptually stores:
				- One Key
				- ```
				  +
				  Two Child Pointers
				  ```
				- per node.
				- This does not efficiently use a database page.
				- A B+ Tree uses a page as one tree node containing:
				- Many Keys
				- ```
				  +
				  Many Child Pointers
				  ```
				- Conceptually:
				- One Page
				- ```
				  ┌─────────────────────────────────┐
				  │ 10 │ 20 │ 30 │ 40 │ 50 │ 60     │
				  │ ↓    ↓    ↓    ↓    ↓    ↓       │
				  │ Child Pointers                  │
				  └─────────────────────────────────┘
				  ```
				- This drastically reduces tree height.
			- ### M-Way Search Tree
			  collapsed:: true
				- An M-way search tree allows a node to contain multiple keys and multiple children.
				- Example:
				- ```
				               [20 | 50 | 80]
				              /    |    |    \
				             ↓     ↓    ↓     ↓
				  ```
				- <20   20-50 50-80  >80
				- Each key helps divide the search space.
			- ### High Fan-out
			  collapsed:: true
				- Suppose a node contains:
				- [20 | 50 | 80]
				- It can have four children:
				- Child 1 → values < 20
				- Child 2 → values between 20 and 50
				- Child 3 → values between 50 and 80
				- Child 4 → values > 80
				- More keys per page:
				- More Keys
				- ```
				      ↓
				  More Children
				      ↓
				  Higher Fan-out
				      ↓
				  Smaller Tree Height
				  ```
			- ### Why Comparisons Inside a Page Are Cheap
			  collapsed:: true
				- You might think:
				- "If a page contains many keys, don't we need more comparisons?"
				- Yes.
				- But comparisons are generally much cheaper than loading another page.
				- Once a page is already in memory:
				- Read Page
				- ```
				      ↓
				  Compare keys inside memory
				      ↓
				  Choose correct child
				  ```
				- The important optimization is:
				- Do more cheap comparisons inside one page to avoid an expensive additional page access.
				- Therefore:
				- CPU Comparisons
				- ```
				          ↓
				  Usually relatively cheap
				  ```
				- Page / Storage Access
				- ```
				          ↓
				  More expensive
				  ```
				- This is why databases prefer high fan-out trees.
			- ### Why M-Way Tree Is Still Not Enough
			  collapsed:: true
				- A normal M-way search tree can still become unbalanced.
				- For example:
				- ```
				          Root
				         /    \
				        ↓      ↓
				      Short   Very Deep
				  ```
				- Then some searches require:
				- 2 page accesses
				- while others require:
				- 10 page accesses
				- This creates unpredictable performance.
				- We need:
				- High Fan-out
				- ```
				          +
				  Guaranteed Balance
				  ```
				- This leads to the B-Tree.
			- ### B-Tree and Automatic Balancing
			  collapsed:: true
				- A B-Tree is essentially a:
				- Balanced M-way search tree.
				- Important properties:
				- Nodes contain multiple keys.
				- Nodes have multiple children.
				- Keys are sorted.
				- The tree remains balanced.
				- All leaves remain at the same depth.
				- Nodes split when they become too full.
				- Nodes borrow or merge when they become too empty.
				- Core balancing logic:
				- Node becomes full
				- ```
				          ↓
				  Split
				          ↓
				  Separator moves upward
				  Node becomes too empty
				          ↓
				  Borrow from sibling
				          OR
				  Merge with sibling
				  ```
				- This ensures predictable tree height.
		- ### 4. B+ Tree Structure
		  collapsed:: true
			- ### Why B+ Tree?
			  collapsed:: true
				- B+ Tree is an evolution of the B-Tree optimized for database indexing.
				- Its major idea is:
				- Internal Nodes
				- ```
				        ↓
				  Navigation
				  ```
				- Leaf Nodes
				- ```
				        ↓
				  Actual Indexed Entries
				  +
				  Row/Data References
				  ```
			- ### Root Node
			  collapsed:: true
				- The root is the starting point of every search.
				- Example:
				- ```
				                   [30 | 70]
				                  /    |    \
				  ```
				- The keys determine which child page should be visited.
				- For example:
				- Search for 45
				- 30 < 45 < 70
				- ```
				          ↓
				  Go to middle child
				  ```
			- ### Internal Nodes
			  collapsed:: true
				- Internal nodes primarily help navigate the tree.
				- Example:
				- [30 | 70 | 100]
				- These keys divide the search space.
				- Internal nodes conceptually contain:
				- Routing Keys
				- ```
				  +
				  Child Page Pointers
				  ```
				- Their purpose is:
				- Tell the database which page to read next.
			- ### Leaf Nodes
			  collapsed:: true
				- Leaf nodes contain the actual indexed entries.
				- Conceptually:
				- Leaf Page
				- ```
				  10 → Row Reference
				  20 → Row Reference
				  30 → Row Reference
				  ```
				- Every search for an indexed entry ultimately reaches a leaf.
			- ### Row/Data References
			  collapsed:: true
				- Once the correct key is found:
				- B+ Tree Leaf
				- email = amit@gmail.com
				- ```
				          ↓
				  Row Reference
				          ↓
				  Actual Row
				  ```
				- The exact meaning of the reference depends on the database.
			- ### Linked Leaf Nodes
			  collapsed:: true
				- Leaf nodes are linked together in sorted order.
				- ```
				  [10 | 20 | 30]
				          ↔
				  [40 | 50 | 60]
				          ↔
				  [70 | 80 | 90]
				  ```
				- This is extremely important for range queries.
			- ### Why High Fan-out Matters
			  collapsed:: true
				- Because internal nodes contain many keys and child pointers:
				- One Page
				- ```
				      ↓
				  Many Routing Keys
				      ↓
				  Many Children
				      ↓
				  High Fan-out
				      ↓
				  Short Tree
				      ↓
				  Fewer Page Accesses
				  ```
				- This is one of the biggest reasons B+ Trees are effective for databases.
		- ### 5. B+ Tree Operations
		  collapsed:: true
			- ### Search
			  collapsed:: true
				- A B+ Tree search follows:
				- Root
				- ```
				   ↓
				  Internal Node
				   ↓
				  Internal Node
				   ↓
				  Leaf Node
				   ↓
				  Indexed Entry
				  ```
				- At every internal node:
				- Compare Search Key
				- ```
				          ↓
				  Determine Range
				          ↓
				  Follow Correct Child Pointer
				  ```
			- ### Point Lookup
			  collapsed:: true
				- Example:
				- WHERE email = 'amit@gmail.com'
				- The B+ Tree navigates directly toward the leaf containing that key.
				- Conceptually:
				- Root
				- ```
				   ↓
				  Correct Range
				   ↓
				  Correct Internal Page
				   ↓
				  Leaf Page
				   ↓
				  Find Exact Key
				   ↓
				  Row Reference
				  ```
			- ### Range Query
			  collapsed:: true
				- Example:
				- WHERE price BETWEEN 100 AND 500
				- Process:
				- Use Tree
				- ```
				      ↓
				  Find Leaf containing 100
				      ↓
				  Start scanning entries
				      ↓
				  Follow Linked Leaf Pages
				      ↓
				  Stop after 500
				  ```
				- Conceptually:
				- ```
				  [100 | 150 | 200]
				            ↔
				  [250 | 300 | 350]
				            ↔
				  [400 | 450 | 500]
				  ```
				- This is why B+ Trees are excellent for range queries.
			- ### Insert
			  collapsed:: true
				- Process:
				- Find Correct Leaf
				- ```
				          ↓
				  Insert Key in Sorted Order
				  ```
				- If there is space:
				- Done
				- If the page is full:
				- Leaf Page Full
				- ```
				          ↓
				  Split Page
				          ↓
				  Create Two Pages
				          ↓
				  Update Leaf Links
				          ↓
				  Send Separator Key to Parent
				  ```
			- ### Page Split
			  collapsed:: true
				- Example:
				- Before
				- [10 | 20 | 30 | 40]
				- After splitting:
				- [10 | 20] ↔ [30 | 40]
				- A separator key is added to the parent so it can navigate to the new page.
			- ### Split Propagation
			  collapsed:: true
				- Sometimes adding a separator makes the parent full.
				- Leaf Split
				- ```
				      ↓
				  Parent Updated
				      ↓
				  Parent Full
				      ↓
				  Parent Split
				      ↓
				  Separator Moves Up
				  ```
				- This can continue toward the root.
			- ### Root Split
			  collapsed:: true
				- If the root becomes full:
				- Root Splits
				- ```
				          ↓
				  New Root Created
				          ↓
				  Tree Height Increases
				  ```
				- The tree grows upward while maintaining balance.
			- ### Delete
			  collapsed:: true
				- Process:
				- Find Correct Leaf
				- ```
				          ↓
				  Delete Entry
				  ```
				- If the page still has enough entries:
				- Done
				- If it becomes too empty:
				- Rebalancing Required
			- ### Borrow / Merge
			- ### Borrow
			  collapsed:: true
				- The database may borrow an entry from a sibling.
				- Sibling
				- ```
				     ↓
				  Move Entry
				     ↓
				  Rebalance
				  ```
			- ### Merge
			  collapsed:: true
				- If borrowing is not possible:
				- Two Underfilled Pages
				- ```
				          ↓
				  Merge
				          ↓
				  Update Parent
				  ```
				- Core concept:
				- ```
				  Insert
				    ↓
				  Split if Full
				  
				  Delete
				    ↓
				  Borrow or Merge if Too Empty
				  ```
		- ### 6. Comparisons & Design Decisions
		  collapsed:: true
			- ### B-Tree vs B+ Tree
				- | Feature | B-Tree | B+ Tree |
				  | --- | --- | --- |
				  | Internal nodes | May contain data references | Primarily routing information |
				  | Leaf nodes | May contain entries/data | Contains actual indexed entries |
				  | Search | May finish before leaf | Reaches leaf |
				  | Internal node size | Usually larger | Usually more compact |
				  | Fan-out | Usually lower | Usually higher |
				  | Range queries | Good | Excellent |
				  | Sequential scan | Less convenient | Excellent |
				  | Linked leaves | Not inherent in the classic model | Key feature |
			- ### Why B+ Tree is preferred
			  collapsed:: true
				- ```
				  Smaller Internal Entries
				          ↓
				  More Keys per Page
				          ↓
				  Higher Fan-out
				          ↓
				  Shorter Tree
				  ```
				- And:
				- ```
				  Sorted Linked Leaves
				          ↓
				  Efficient Range Queries
				  ```
			- ### B+ Tree vs BST
			  collapsed:: true
				- ### BST
				  collapsed:: true
					- ```
					  Maximum Children = 2
					          ↓
					  Low Fan-out
					          ↓
					  More Tree Levels
					          ↓
					  More Page Traversals
					  ```
					- A BST also does not naturally use database pages efficiently.
				- ### B+ Tree
				  collapsed:: true
					- ```
					  Many Keys per Page
					          ↓
					  Many Children
					          ↓
					  High Fan-out
					          ↓
					  Small Tree Height
					          ↓
					  Fewer Page Accesses
					  ```
				- Key Difference
					- BSTs optimize navigation through individual nodes, while B+ Trees are designed to efficiently use pages containing many keys.
			- ### B+ Tree vs Hash Index
			  collapsed:: true
				- ### Hash Index
				  collapsed:: true
					- ```
					  Value
					    ↓
					  Hash Function
					    ↓
					  Bucket
					    ↓
					  Entry
					  ```
					- Example:
					- ```
					  hash(user_id = 123)
					          ↓
					  Bucket 42
					  ```
					- Excellent for:
					- `WHERE user_id = 123`
				- ### The Problem with Hashing
				  collapsed:: true
					- Hashing does not preserve order.
					- Example:
					- ```
					  10 → Bucket 7
					  20 → Bucket 2
					  30 → Bucket 9
					  40 → Bucket 1
					  ```
					- Even though:
					- `10 < 20 < 30 < 40`
					- the bucket locations are unrelated.
					- Therefore:
					- `WHERE age BETWEEN 20 AND 40`
					- cannot efficiently follow:
					- `20 → 30 → 40`
					- because they may be in unrelated buckets.
				- ### B+ Tree
				  collapsed:: true
					- Values remain ordered:
					- ```
					  [10 | 20 | 30]
					          ↔
					  [40 | 50 | 60]
					  ```
					- Therefore it supports:
					- Equality Lookup     ✓
					- Range Queries       ✓
					- Ordered Scans       ✓
					- Sorting             ✓
					- Sequential Access   ✓
			- ### Equality vs Range Queries
			  collapsed:: true
				- ### Exact Equality
				  collapsed:: true
					- `WHERE user_id = 123`
					- Hash indexes can be excellent.
					- B+ Trees are also efficient.
				- ### Range
				  collapsed:: true
					- `WHERE price BETWEEN 100 AND 500`
					- B+ Trees are much more suitable because they preserve order.
			- ### Important Principle
			  collapsed:: true
				- Do not say:
				- B+ Tree is always faster than Hash Index.
				- Instead:
				- Hash indexes are specialized for exact equality lookups, while B+ Trees are more versatile because they preserve order and efficiently support both point lookups and range queries.
		- ### Key Interview Takeaways
		  collapsed:: true
			- **1.**
			- A database page is the basic unit used for storing, reading, and caching database data and index entries.
			- **2.**
			- Database indexes aim to reduce the number of pages that must be accessed to locate data.
			- **3.**
			- ```
			  Low Fan-out
			      ↓
			  Tall Tree
			      ↓
			  More Page Accesses
			  High Fan-out
			      ↓
			  Short Tree
			      ↓
			  Fewer Page Accesses
			  ```
			- **4.**
			- An M-way tree allows many keys and children per node, making it better suited to page-based storage than a binary search tree.
			- **5.**
			- A B-Tree adds automatic balancing to an M-way search tree.
			- **6.**
			- A B+ Tree separates navigation and indexed entries: internal nodes guide the search, while leaf nodes contain indexed entries and references to the actual data.
			- **7.**
			- Linked leaf nodes make B+ Trees highly efficient for range queries and sequential scans.
			- **8.**
			- B+ Trees are widely used for database indexing because they combine high fan-out, balanced height, efficient page access, equality lookups, and range query support.
		- ### Final Mental Model
		  collapsed:: true
			- Database stores data in Pages
			- ```
			              ↓
			  Need to locate data efficiently
			              ↓
			  Create Index
			              ↓
			  Index becomes too large
			              ↓
			  Multilevel Indexing
			              ↓
			  Need efficient page navigation
			              ↓
			  BST has low fan-out
			              ↓
			  M-Way Tree provides high fan-out
			              ↓
			  Need automatic balancing
			              ↓
			  B-Tree
			              ↓
			  Optimize for database indexing + range scans
			              ↓
			  B+ Tree
			  ```
	- ### Types of Indexes
	  collapsed:: true
		- ### Clustered Index
		  collapsed:: true
			- ### What is it?
			  collapsed:: true
				- A **Clustered Index organizes the actual table data according to the index key**.
				- Rows with nearby key values are stored near each other in the database's storage structure.
				- ```
				  Clustered Index on id
				  
				  Data Pages
				  
				  Page 1 → 1, 2, 3
				  Page 2 → 4, 5, 6
				  Page 3 → 7, 8, 9
				  ```
				- Conceptually:
				- ```
				  Search Key
				    ↓
				  Clustered B+ Tree
				    ↓
				  Actual Table Data
				  ```
			- ### Characteristics
			  collapsed:: true
				- The table data is organized according to the clustered key.
				- Nearby key values are stored near each other.
				- Generally only **one clustered organization** per table.
				- Particularly efficient for:
					- Primary key lookups
					- Range queries
					- Sequential access
			- ### Example
			  collapsed:: true
				- ```
				  SELECT *
				  FROM users
				  WHERE id BETWEEN 100 AND 200;
				  ```
				- The database can find `100` and then efficiently traverse nearby data until `200`.
			- ### Unique vs Non-Unique
			  collapsed:: true
				- A clustered key **does not necessarily have to be unique**.
				- Example:
				- ```
				  department
				  
				  Engineering
				  Engineering
				  Engineering
				  HR
				  HR
				  Sales
				  ```
				- Duplicate values are stored together according to key order. Internally, the database uses additional information to distinguish individual rows.
				- Clustered Index describes **how data is organized**, not whether duplicates are allowed.
		- ### Non-Clustered Index
		  collapsed:: true
			- ### What is it?
			  collapsed:: true
				- A **Non-Clustered Index is a separate data structure from the actual table data**.
				- It stores:
				- ```
				  Indexed Key
				     +
				  Row Locator / Reference
				  ```
				- Conceptually:
				- ```
				  Non-Clustered B+ Tree
				  
				  email
				   ↓
				  Row Locator
				   ↓
				  Actual Table Data
				  ```
			- ### Example
			  collapsed:: true
				- Actual table data:
				- ```
				  id | name   | email
				  ----------------------------
				  1  | Aditya | aditya@email.com
				  2  | Amit   | amit@email.com
				  3  | Rahul  | rahul@email.com
				  ```
				- Non-clustered index on `email`:
				- ```
				  aditya@email.com → Row Locator
				  amit@email.com   → Row Locator
				  rahul@email.com  → Row Locator
				  ```
			- ### Read Flow
			  collapsed:: true
				- ```
				  Query
				  ↓
				  Search Non-Clustered Index
				  ↓
				  Reach Index Leaf Node
				  ↓
				  Get Row Locator
				  ↓
				  Locate Actual Row
				  ```
				- The row locator can vary depending on the database:
				- ```
				  • Physical Row Reference
				  • Primary / Clustered Key
				  • Internal Row Identifier
				  ```
				- If the locator is a clustered/primary key:
				- ```
				  Non-Clustered Index
				        ↓
				  Get Primary / Clustered Key
				        ↓
				  Search Clustered Index
				        ↓
				  Actual Row
				  ```
			- ### Characteristics
			  collapsed:: true
				- Separate from actual table data.
				- Multiple non-clustered indexes can exist on a table.
				- May require an additional table/clustered-index lookup.
				- Useful for frequently queried columns.
		- ### Unique Index
		  collapsed:: true
			- ### What is it?
			  collapsed:: true
				- A **Unique Index prevents duplicate values while also providing index lookup capability**.
				- Example:
				- ```
				  CREATE UNIQUE INDEX idx_users_email
				  ON users(email);
				  ```
				- ```
				  amit@gmail.com
				  rahul@gmail.com
				  aditya@gmail.com
				  ```
				- Trying to insert another:
				- ```
				  amit@gmail.com
				  ```
				- will fail.
			- ### Significance
			  collapsed:: true
				- A unique index provides:
				- ```
				  Fast Lookup
				     +
				  Database-Level Duplicate Prevention
				  ```
				- This is especially important with concurrent requests.
				- ```
				  Server A → Insert email
				  Server B → Insert same email
				  
				  Database Unique Index
				        ↓
				  Only one succeeds
				  ```
			- ### UNIQUE Constraint
			  collapsed:: true
				- When defining:
				- ```
				  CREATE TABLE users (
				    id BIGINT PRIMARY KEY,
				    email VARCHAR(255) UNIQUE
				  );
				  ```
				- the database generally creates and maintains an underlying **unique index or equivalent indexed structure**.
				- Therefore:
				- ```
				  UNIQUE Constraint
				       ↓
				  Unique Index / Equivalent Structure
				       ↓
				  Fast Lookup
				  +
				  Duplicate Prevention
				  ```
				- Creating an additional normal index on the same column is usually redundant.
			- ### Important Distinction
			  collapsed:: true
				- ```
				  Clustered / Non-Clustered
				  → Describes how the index relates to data storage.
				  
				  Unique / Non-Unique
				  → Describes whether duplicate key values are allowed.
				  ```
		- ### Hash Index
		  collapsed:: true
			- ### What is it?
			  collapsed:: true
				- A **Hash Index uses a hash function to map a key to a bucket/location**.
				- ```
				  Key
				  ↓
				  Hash Function
				  ↓
				  Hash Value
				  ↓
				  Bucket
				  ↓
				  Row Reference
				  ```
				- Example:
				- ```
				  SELECT *
				  FROM users
				  WHERE email = 'amit@gmail.com';
				  ```
				- Conceptually:
				- ```
				  hash('amit@gmail.com')
				        ↓
				      Bucket
				        ↓
				  Row Reference
				        ↓
				  Actual Row
				  ```
			- ### Best Use Case
			  collapsed:: true
				- Hash indexes are particularly efficient for **exact equality lookups**:
				- ```
				  WHERE email = 'amit@gmail.com'
				  ```
				- Average lookup:
				- ```
				  O(1)
				  ```
			- ### Limitation: Range Queries
			  collapsed:: true
				- Hashing destroys the natural ordering of values.
				- ```
				  20 → Bucket 7
				  21 → Bucket 91
				  22 → Bucket 3
				  23 → Bucket 65
				  ```
				- Therefore, it cannot efficiently support:
				- ```
				  WHERE age > 25
				  ```
				- ```
				  WHERE age BETWEEN 20 AND 30
				  ```
				- or:
				- ```
				  ORDER BY age
				  ```
			- ### Hash Collision
				- Different keys can produce the same hash and land in the same bucket:
				- ```
				  hash('Amit')  → Bucket 10
				  hash('Rahul') → Bucket 10
				  ```
				- The hash only finds the bucket. The database still compares the actual key to pick the right row.
				- ### Chaining
					- Each bucket holds a small list of `(key, row reference)` pairs.
					- ```
					  Bucket 10
					    → (Amit,  row_A)
					    → (Rahul, row_R)
					  ```
					- Lookup: go to Bucket 10, then walk the list until the key matches.
					- Insert: append to that list.
				- ### Open Addressing
					- The table stores one entry per slot. If Bucket 10 is full, try the next empty slot:
					- ```
					  Bucket 10 → Amit
					  Bucket 11 → Rahul   (overflow from 10)
					  Bucket 12 → empty
					  ```
					- Lookup: start at the hashed bucket and keep probing `10, 11, 12...` until the key is found or an empty slot appears.
				- A few collisions are cheap. Too many means longer lists or longer probe sequences, so lookup drifts away from O(1).
			- ### Hash Index vs B+ Tree
			  collapsed:: true
				- | Feature | Hash Index | B+ Tree Index |
				  | ---- | ---- | ---- |
				  | Equality Lookup | Excellent | Excellent |
				  | Range Queries | ❌ | ✅ |
				  | Ordered Traversal | ❌ | ✅ |
				  | `ORDER BY` | ❌ | ✅ |
				  | General Purpose | Limited | Excellent |
				  | Lookup | ~O(1) average | O(log N) |
			- ### Key Takeaway
			  collapsed:: true
				- ```
				  Hash Index
				  → Best for exact key lookup.
				  
				  B+ Tree
				  → Supports equality
				  + range queries
				  + ordered traversal
				  ```
				- Because databases commonly need multiple query patterns, **B+ Trees are generally more versatile and widely used as general-purpose indexe**
	- ### Composite Indexes
	  collapsed:: true
		- ### What is a Composite Index?
		  collapsed:: true
			- A **Composite Index** is an index created using **multiple columns together**.
			- Example:
			- ```
			  CREATE INDEX idx_user_status
			  ON orders(user_id, status);
			  ```
			- Conceptually, the B+ Tree stores and sorts **combinations of column values**:
			- ```
			  (user_id, status)
			  
			  (101, COMPLETED)
			  (101, PENDING)
			  (101, SHIPPED)
			  
			  (102, COMPLETED)
			  (102, PENDING)
			  ```
			- The ordering is:
			- ```
			  First by user_id
			        ↓
			  Then by status within the same user_id
			  ```
			- A composite index is **not simply two independent indexes combined**.
			- ```
			  INDEX(user_id)
			  +
			  INDEX(status)
			  
			  ≠
			  
			  INDEX(user_id, status)
			  ```
			- The composite index creates one index structure containing the **combined key**.
		- ### Why Use Multiple Columns?
		  collapsed:: true
			- Consider the query:
			- ```
			  SELECT *
			  FROM orders
			  WHERE user_id = 101
			  AND status = 'PENDING';
			  ```
			- With separate indexes:
			- ```
			  INDEX(user_id)
			  INDEX(status)
			  ```
			- the database may need to:
			- ```
			  Search user_id index
			        ↓
			  Get matching row references
			  
			  Search status index
			        ↓
			  Get matching row references
			  
			  Intersect / combine results
			        ↓
			  Fetch matching rows
			  ```
			- With a composite index:
			- ```
			  INDEX(user_id, status)
			  ```
			- the database can navigate directly to the combined key:
			- ```
			  (user_id = 101, status = PENDING)
			  ```
			- Conceptually:
			- ```
			  One B+ Tree
			      ↓
			  Navigate to the specific combination
			      ↓
			  Matching entries
			  ```
			- ### Key Advantage
				- A composite index can reduce the amount of data the database needs to examine for queries filtering on multiple related columns.
		- ### Leftmost Prefix Rule
		  collapsed:: true
			- For an index:
			- ```
			  INDEX(user_id, status)
			  ```
			- the B+ Tree is ordered as:
			- ```
			  user_id
			    ↓
			  status
			  ```
			- Therefore, the index can efficiently support:
			- ```
			  WHERE user_id = 101
			  ```
			- ```
			  WHERE user_id = 101
			  AND status = 'PENDING'
			  ```
			- But generally not an efficient direct lookup for:
			- ```
			  WHERE status = 'PENDING'
			  ```
			- because all `status = PENDING` entries are not necessarily stored together.
			- Example:
			- ```
			  (101, COMPLETED)
			  (101, PENDING)
			  
			  (102, COMPLETED)
			  (102, PENDING)
			  
			  (103, PENDING)
			  ```
			- The index can efficiently use prefixes starting from the **leftmost column**.
			- For:
			- ```
			  INDEX(A, B, C)
			  ```
			- think:
			- ```
			  A              ✅
			  A + B          ✅
			  A + B + C      ✅
			  
			  B              ❌ Generally
			  C              ❌ Generally
			  B + C          ❌ Generally
			  ```
			- ### Key Idea
				- A composite B+ Tree index is primarily useful when the query uses the **leftmost prefix** of the indexed columns.
		- ### Column Order
		  collapsed:: true
			- The order of columns in a composite index matters.
			- These are different indexes:
			- ```
			  INDEX(user_id, status)
			  ```
			- ```
			  INDEX(status, user_id)
			  ```
			- ### `(user_id, status)`
				- Efficiently supports:
				- ```
				  WHERE user_id = ?
				  ```
				- ```
				  WHERE user_id = ?
				  AND status = ?
				  ```
			- ### `(status, user_id)`
				- Efficiently supports:
				- ```
				  WHERE status = ?
				  ```
				- ```
				  WHERE status = ?
				  AND user_id = ?
				  ```
				- Therefore:
				- **The same columns in a different order create a different index structure and support different leading query patterns.**
			- ### Choosing Column Order
			  collapsed:: true
				- Column order should primarily depend on:
				- ```
				  Application Query Patterns
				        ↓
				  Columns used for equality
				        ↓
				  Columns used for ranges
				        ↓
				  Selectivity and data distribution
				  ```
				- A useful starting heuristic:
				- ```
				  Equality Columns
				        ↓
				  Range Columns
				        ↓
				  Other columns based on query requirements
				  ```
				- Example:
				- ```
				  WHERE user_id = 101
				  AND created_at > '2026-01-01'
				  ```
				- A commonly useful index:
				- ```
				  INDEX(user_id, created_at)
				  ```
				- Conceptually:
				- ```
				  Find user_id = 101
				        ↓
				  Within that user's entries,
				  created_at is ordered
				        ↓
				  Find starting date
				        ↓
				  Scan forward
				  ```
		- ### Multiple Composite Indexes
		  collapsed:: true
			- Multiple composite indexes can exist on the same table.
			- Example:
			- ```
			  CREATE INDEX idx_user_status
			  ON orders(user_id, status);
			  
			  CREATE INDEX idx_status_user
			  ON orders(status, user_id);
			  ```
			- These create two separate index data structures.
			- ```
			  orders
			  │
			  ├── idx_user_status
			  │      → (user_id, status)
			  │
			  └── idx_status_user
			       → (status, user_id)
			  ```
			- This may be useful when the application has different query patterns:
			- ```
			  WHERE user_id = ?
			  ```
			- and:
			- ```
			  WHERE status = ?
			  ```
			- However, additional indexes increase:
			- ```
			  Storage Usage
			  +
			  Write Overhead
			  +
			  Index Maintenance Cost
			  ```
			- Therefore, indexes should only be added when justified by actual query patterns.
		- ### How the Database Chooses an Index
		  collapsed:: true
			- When multiple indexes exist, the database does not randomly choose one.
			- ```
			  SQL Query
			    ↓
			  Query Parser
			    ↓
			  Query Optimizer / Planner
			    ↓
			  Generate possible execution plans
			    ↓
			  Estimate cost
			    ↓
			  Choose cheapest plan
			  ```
			- Example:
			- ```
			  WHERE user_id = 101
			  AND status = 'PENDING'
			  ```
			- Possible options:
			- ```
			  Use INDEX(user_id, status)
			            ↓
			  Estimate Cost
			  
			  Use INDEX(status, user_id)
			            ↓
			  Estimate Cost
			  
			  Use Multiple Indexes
			            ↓
			  Estimate Cost
			  
			  Full Table Scan
			            ↓
			  Estimate Cost
			  ```
			- The optimizer considers factors such as:
				- Query conditions
				- Available indexes
				- Data distribution
				- Selectivity
				- Database statistics
				- Number of rows expected to match
				- Cost of fetching actual rows
				- Whether the index covers the query
			- **An existing index is not guaranteed to be used. The database may choose another index or even a full table scan if it estimates that option is cheaper.**
		- ### Real Examples
		  collapsed:: true
			- ### Example 1: User Orders
				- Query:
				- ```
				  SELECT *
				  FROM orders
				  WHERE user_id = 101
				  AND status = 'PENDING';
				  ```
				- Suitable index:
				- ```
				  INDEX(user_id, status)
				  ```
			- ### Example 2: Orders by User and Date
				- Query:
				- ```
				  SELECT *
				  FROM orders
				  WHERE user_id = 101
				  AND created_at > '2026-01-01';
				  ```
				- Suitable index:
				- ```
				  INDEX(user_id, created_at)
				  ```
				- Reason:
				- ```
				  user_id → Equality
				  created_at → Range
				  ```
			- ### Example 3: Searching by Status Alone
				- Query:
				- ```
				  SELECT *
				  FROM orders
				  WHERE status = 'PENDING';
				  ```
				- If the existing index is:
				- ```
				  INDEX(user_id, status)
				  ```
				- it generally cannot provide the normal leftmost-prefix lookup advantage for `status` alone.
				- A separate index may be required:
				- ```
				  INDEX(status)
				  ```
				- or a differently ordered composite index:
				- ```
				  INDEX(status, user_id)
				  ```
				- depending on application query patterns.
		- ### Key Takeaways
		  collapsed:: true
			- ```
			  Composite Index
			        ↓
			  One index structure using multiple columns
			  ```
			- ```
			  INDEX(A, B)
			        ↓
			  Supports A
			  Supports A + B
			  Generally does not efficiently support B alone
			  ```
			- ```
			  Column Order Matters
			  (A, B) ≠ (B, A)
			  ```
			- ```
			  Design Indexes
			  Based on Query Patterns
			  Not Simply Based on Table Columns
			  ```
			- ```
			  More Indexes
			        ↓
			  Faster Reads
			        +
			  More Storage
			        +
			  More Write / Maintenance Overhead
			  ```
			- **A composite index is designed around how columns are queried together. Its column order determines how the B+ Tree is ordered, which directly determines which query patterns can efficiently use the index.**
	- ### Covering Indexes
	  collapsed:: true
		- ### What is a Covering Index?
		  collapsed:: true
			- A **Covering Index** is an index that contains **all the columns required to execute a specific query**.
			- Example index:
			- ```
			  INDEX(user_id, status)
			  ```
			- Query:
			- ```
			  SELECT status
			  FROM orders
			  WHERE user_id = 101;
			  ```
			- The query needs:
			- ```
			  WHERE  → user_id
			  SELECT → status
			  ```
			- Both columns are available in the index:
			- ```
			  (user_id, status)
			  ```
			- Therefore, this index **covers the query**.
			- ```
			  Index
			  ↓
			  Find user_id = 101
			  ↓
			  status is already available
			  ↓
			  Return Result
			  ```
			- **An index is not inherently a covering index. It is covering relative to a particular query.**
			- The same index may cover one query but not another.
			- Example:
			- ```
			  SELECT status
			  FROM orders
			  WHERE user_id = 101;
			  ```
			- ```
			  Covered ✅
			  ```
			- But:
			- ```
			  SELECT amount
			  FROM orders
			  WHERE user_id = 101;
			  ```
			- ```
			  Not Covered ❌
			  ```
			- because `amount` is not available in:
			- ```
			  INDEX(user_id, status)
			  ```
		- ### Index-Only Scan
		  collapsed:: true
			- An **Index-Only Scan** is an execution strategy where the database executes a query using **only the index**, without reading the actual table rows.
			- Example:
			- ```
			  INDEX(user_id, status)
			  ```
			- Query:
			- ```
			  SELECT status
			  FROM orders
			  WHERE user_id = 101;
			  ```
			- Execution:
			- ```
			  Search Index
			     ↓
			  Find user_id = 101
			     ↓
			  Read status from Index
			     ↓
			  Return Result
			  ```
			- No table lookup is required.
			- ### Covering Index vs Index-Only Scan
				- ```
				  Covering Index
				      ↓
				  A property of the Index relative to a Query
				      ↓
				  "The index contains everything required"
				  ```
				- ```
				  Index-Only Scan
				      ↓
				  An execution strategy
				      ↓
				  "The database executes the query using only the index"
				  ```
				- Relationship:
				- ```
				  Index contains all required columns
				            ↓
				  It is a Covering Index for that query
				            ↓
				  Index-Only Scan becomes possible
				            ↓
				  Optimizer decides whether to use it
				  ```
				- **Covering Index = Capability**
				- **Index-Only Scan = Execution**
		- ### Avoiding Table Lookup
		  collapsed:: true
			- With a normal index:
			- ```
			  Index
			  ↓
			  Find Matching Entry
			  ↓
			  Row Reference / Pointer
			  ↓
			  Go to Table/Data Page
			  ↓
			  Fetch Required Column
			  ```
			- Example:
			- ```
			  INDEX(user_id)
			  ```
			- Query:
			- ```
			  SELECT status
			  FROM orders
			  WHERE user_id = 101;
			  ```
			- The index can find the matching rows but does not contain `status`.
			- Therefore:
			- ```
			  Index Search
			    ↓
			  Row Reference
			    ↓
			  Table/Data Page Lookup
			    ↓
			  Read status
			  ```
			- With a covering index:
			- ```
			  INDEX(user_id, status)
			  ```
			- ```
			  Index Search
			    ↓
			  Required Data Already Exists in Index
			    ↓
			  Return Result
			  ```
			- ### Why does this matter?
				- Following row references may require additional data-page accesses.
				- For a query returning many rows:
				- ```
				  Index finds many matching entries
				        ↓
				  Many Row References
				        ↓
				  Potentially many additional Table/Data Page accesses
				  ```
				- A covering index can avoid these additional accesses.
				- **Avoiding table lookups can significantly improve performance, especially when a query returns many matching rows.**
				- Note:
				- ```
				  100,000 matching rows
				  ≠
				  necessarily 100,000 disk reads
				  ```
				- Multiple rows can exist on the same page, and databases also use caching.
				- The key benefit is:
				- **The database avoids additional table/data-page accesses after finding the matching index entries.**
		- ### Real Examples
		  collapsed:: true
			- ### Example 1
				- Index:
				- ```
				  INDEX(user_id, status)
				  ```
				- Query:
				- ```
				  SELECT status
				  FROM orders
				  WHERE user_id = 101;
				  ```
				- Required columns:
				- ```
				  user_id → Filtering
				  status  → Returning
				  ```
				- Both are in the index.
				- ```
				  Covering Index      → Yes ✅
				  Index-Only Scan     → Possible ✅
				  Table Lookup        → Not Required
				  ```
			- ### Example 2
				- Index:
				- ```
				  INDEX(user_id, status)
				  ```
				- Query:
				- ```
				  SELECT amount
				  FROM orders
				  WHERE user_id = 101;
				  ```
				- Required columns:
				- ```
				  user_id → Filtering
				  amount  → Returning
				  ```
				- `amount` is not in the index.
				- ```
				  Covering Index      → No ❌
				  Index-Only Scan     → Not Possible
				  Table Lookup        → Required
				  ```
			- ### Example 3
				- Index:
				- ```
				  INDEX(user_id, created_at, status)
				  ```
				- Query:
				- ```
				  SELECT status
				  FROM orders
				  WHERE user_id = 101
				  AND created_at > '2026-01-01';
				  ```
				- Required columns:
				- ```
				  user_id     → Filtering
				  created_at  → Filtering
				  status      → Returning
				  ```
				- All required columns exist in the index.
				- ```
				  Covering Index      → Yes ✅
				  Index-Only Scan     → Possible ✅
				  Table Lookup        → Not Required
				  ```
		- ### Key Takeaways
		  collapsed:: true
			- ```
			  Normal Index
			    ↓
			  Helps Find Matching Rows
			    ↓
			  May Need Table Lookup
			  ```
			- ```
			  Covering Index
			    ↓
			  Helps Find Matching Rows
			    +
			  Contains Required Query Data
			    ↓
			  Table Lookup Can Be Avoided
			  ```
			- ```
			  Covering Index
			        ↓
			  Property / Capability
			  
			  Index-Only Scan
			        ↓
			  Execution Strategy
			  ```
			- **A covering index contains all the data required for a specific query. An index-only scan is the database execution strategy of answering that query using only the index, without following row references back to the actual table.**
	- ### Index Optimization
	  collapsed:: true
		- ### 6.1 Cardinality
		  collapsed:: true
			- **Cardinality = Number of distinct values in a column.**
			- Example:
			- ```
			  user_id
			  1
			  2
			  3
			  4
			  ...
			  ```
			- Almost every value is different:
			- ```
			  High Cardinality
			  ```
			- Example:
			- ```
			  status
			  
			  PENDING
			  COMPLETED
			  CANCELLED
			  ```
			- Only a few distinct values:
			- ```
			  Low Cardinality
			  ```
			- ### Why it matters
				- Generally:
				- ```
				  High Cardinality
				      ↓
				  More distinct values
				      ↓
				  Can narrow down data effectively
				      ↓
				  Index is often more useful
				  ```
				- ```
				  Low Cardinality
				      ↓
				  Few distinct values
				      ↓
				  A condition may match many rows
				      ↓
				  Index may be less useful
				  ```
				- However:
				- **Cardinality alone does not determine whether an index will be useful. Data distribution and the specific query also matter.**
				- For example, `status` may have low cardinality but:
				- ```
				  PENDING   → 99,998,000 rows
				  COMPLETED → 1,000 rows
				  CANCELLED → 1,000 rows
				  ```
				- An index on `status` can still be useful for:
				- ```
				  WHERE status = 'COMPLETED'
				  ```
		- ### 6.2 Selectivity
		  collapsed:: true
			- **Selectivity describes how effectively a query condition reduces the number of rows.**
			- ```
			  Total Rows
			    ↓
			  Apply WHERE condition
			    ↓
			  Matching Rows
			  ```
			- Example:
			- ```
			  100,000,000 rows
			       ↓
			  WHERE user_id = 101
			       ↓
			  1 row
			  ```
			- This is **highly selective**.
			- Example:
			- ```
			  100,000,000 rows
			       ↓
			  WHERE status = 'PENDING'
			       ↓
			  40,000,000 rows
			  ```
			- This has **low selectivity**.
			- Generally:
			- ```
			  High Selectivity
			      ↓
			  Few matching rows
			      ↓
			  Index is often useful
			  ```
			- ```
			  Low Selectivity
			      ↓
			  Many matching rows
			      ↓
			  Index may be less useful
			  ```
			- ### Cardinality vs Selectivity
				- ```
				  Cardinality
				    ↓
				  How many distinct values exist in a column?
				  ```
				- ```
				  Selectivity
				    ↓
				  How effectively does a specific query condition reduce rows?
				  ```
				- Example:
				- ```
				  status has:
				  
				  PENDING
				  COMPLETED
				  CANCELLED
				  ```
				- ```
				  Cardinality = 3
				  ```
				- But:
				- ```
				  WHERE status = 'PENDING'
				  ```
				- may return:
				- ```
				  40 million rows → Low Selectivity
				  ```
				- while:
				- ```
				  WHERE status = 'CANCELLED'
				  ```
				- may return:
				- ```
				  100 rows → High Selectivity
				  ```
				- **Cardinality describes the column; selectivity describes the effect of a particular query condition.**
		- ### 6.3 Why Indexes Are Ignored
		  collapsed:: true
			- Creating an index does **not guarantee** that the database will use it.
			- ```
			  Index Exists
			     ↓
			  Optimizer compares possible plans
			     ↓
			  Another plan is estimated to be cheaper
			     ↓
			  Index may be ignored
			  ```
			- ### 1. Low Selectivity
				- Example:
				- ```
				  100 million rows
				  
				  WHERE status = 'PENDING'
				       ↓
				  70 million matching rows
				  ```
				- Using an index may require:
				- ```
				  Index Scan
				    ↓
				  Find millions of entries
				    ↓
				  Follow many row references
				    ↓
				  Access many table/data pages
				  ```
				- A sequential table scan may be cheaper.
			- ### 2. Query Returns a Large Percentage of the Table
				- If a query needs most of the table:
				- ```
				  100 million rows
				       ↓
				  80 million rows required
				  ```
				- Using an index may add unnecessary work:
				- ```
				  Read Index
				    +
				  Fetch most of the Table Rows
				  ```
				- Instead:
				- ```
				  Sequentially Scan Table
				  ```
				- may be cheaper.
			- ### 3. Function Applied to Indexed Column
				- Suppose:
				- ```
				  INDEX(email)
				  ```
				- This can efficiently use the index:
				- ```
				  WHERE email = 'amit@gmail.com'
				  ```
				- But:
				- ```
				  WHERE LOWER(email) = 'amit@gmail.com'
				  ```
				- may not efficiently use a normal `INDEX(email)` because the B+ Tree is ordered by the original `email` values, not necessarily `LOWER(email)` values.
				- A possible solution is an expression/functional index where supported:
				- ```
				  INDEX(LOWER(email))
				  ```
			- ### 4. Type Conversion
				- Suppose:
				- ```
				  user_id → INTEGER
				  ```
				- and:
				- ```
				  INDEX(user_id)
				  ```
				- A query like:
				- ```
				  WHERE user_id = 101
				  ```
				- can directly use the indexed values.
				- But:
				- ```
				  WHERE CAST(user_id AS VARCHAR) = '101'
				  ```
				- may prevent efficient use of the normal index because the indexed column is being transformed.
			- ### 5. Leading Wildcard
				- With:
				- ```
				  INDEX(email)
				  ```
				- This can often efficiently use a B+ Tree:
				- ```
				  WHERE email LIKE 'aditya%'
				  ```
				- because values beginning with `aditya` are grouped together in sorted order.
				- But:
				- ```
				  WHERE email LIKE '%gmail.com'
				  ```
				- generally cannot efficiently use a normal B+ Tree index for direct lookup because values are ordered by their beginning, not their ending.
			- ### 6. Outdated or Inaccurate Statistics
				- The optimizer relies on statistics to estimate:
				- ```
				  How many rows will match?
				  How expensive will each plan be?
				  ```
				- If statistics are inaccurate:
				- ```
				  Incorrect Statistics
				        ↓
				  Incorrect Row Estimate
				        ↓
				  Incorrect Cost Estimate
				        ↓
				  Potentially Bad Execution Plan
				  ```
			- ### 7. Another Index or Plan Is Cheaper
				- A table can have multiple indexes:
				- ```
				  INDEX(user_id)
				  
				  INDEX(status)
				  
				  INDEX(user_id, status)
				  ```
				- For a query:
				- ```
				  WHERE user_id = 101
				  AND status = 'PENDING'
				  ```
				- multiple indexes may be usable.
				- The optimizer estimates the cost of each possible plan and chooses the cheapest estimated one.
				- **An index may be ignored simply because another index or execution plan is estimated to be cheaper.**
		- ### 6.4 Optimizer Decisions
		  collapsed:: true
			- The **Query Optimizer** decides the most efficient way to execute a query.
			- SQL specifies:
			- **What data is required**
			- The optimizer decides:
			- **How to retrieve that data**
			- Example:
			- ```
			  SELECT *
			  FROM orders
			  WHERE user_id = 101
			  AND status = 'PENDING';
			  ```
			- The optimizer may consider:
			- ```
			  Plan 1 → Use INDEX(user_id)
			  
			  Plan 2 → Use INDEX(status)
			  
			  Plan 3 → Use INDEX(user_id, status)
			  
			  Plan 4 → Full Table Scan
			  ```
			- ### How the Optimizer Chooses
				- ```
				  SQL Query
				    ↓
				  Generate Possible Execution Plans
				    ↓
				  Check Available Indexes
				    ↓
				  Use Statistics
				    ↓
				  Estimate Matching Rows
				    ↓
				  Estimate Cost
				    ↓
				  Compare Plans
				    ↓
				  Choose Cheapest Estimated Plan
				  ```
				- The optimizer uses statistics/metadata about the table and indexes.
				- Statistics may contain information such as:
				- ```
				  ├── Approximate Number of Rows
				  ├── Number of Distinct Values
				  ├── Value Distribution
				  ├── Histograms
				  ├── NULL Distribution
				  ├── Minimum / Maximum Values
				  ├── Table Size / Number of Pages
				  └── Index Information
				  ```
				- These statistics help estimate:
				- ```
				  Cardinality
				     +
				  Selectivity
				     +
				  Expected Matching Rows
				     +
				  Page Reads
				     +
				  CPU Work
				     +
				  Table Lookups
				  ```
				- The optimizer then compares the estimated costs.
			- ### Estimated Cost vs Actual Cost
				- The optimizer chooses:
				- **The cheapest estimated plan, not necessarily the actual cheapest plan.**
				- ```
				  Best Estimated Plan
				        ≠
				  Always Best Actual Plan
				  ```
				- If statistics are outdated:
				- ```
				  Actual Data
				      ≠
				  Optimizer Statistics
				      ↓
				  Incorrect Estimates
				      ↓
				  Potentially Poor Plan
				  ```
			- ### Optimizer and Covering Indexes
				- The optimizer also checks whether an index covers the query.
				- ```
				  Index Contains:
				  ├── Filtering Columns
				  └── Required Result Columns
				  ```
				- Then:
				- ```
				  Covering Index
				      ↓
				  Index-Only Scan Possible
				      ↓
				  Avoid Table Lookup
				      ↓
				  Potentially Lower Cost
				  ```
		- ### Key Takeaways
		  collapsed:: true
			- ```
			  Cardinality
			    ↓
			  Number of distinct values in a column
			  ```
			- ```
			  Selectivity
			    ↓
			  How effectively a query condition reduces rows
			  ```
			- ```
			  Index Exists
			    ≠
			  Index Will Definitely Be Used
			  ```
			- ```
			  Optimizer
			    ↓
			  Uses Statistics
			    ↓
			  Estimates Rows and Cost
			    ↓
			  Compares Possible Plans
			    ↓
			  Chooses Cheapest Estimated Plan
			  ```
			- **Indexes provide possible paths for retrieving data. The query optimizer uses statistics, cardinality, selectivity, and cost estimation to decide which execution plan it believes will be the most efficient.**
-
- Connection Pooling
  │
  ├── 1. Why Database Connections Are Expensive
  │     ├── What Happens When a Connection Is Created
  │     ├── TCP / Network Setup
  │     ├── Authentication
  │     └── Why Creating One Per Request Is Bad
  │
  ├── 2. What Is Connection Pooling?
  │     ├── Basic Idea
  │     ├── Pool of Reusable Connections
  │     └── Request Lifecycle With a Pool
  │
  ├── 3. Connection Pool Internals
  │     ├── Idle Connections
  │     ├── Active / Borrowed Connections
  │     ├── Acquiring a Connection
  │     ├── Returning a Connection
  │     └── Pool Size
  │
  ├── 4. Pool Exhaustion & Waiting
  │     ├── What Happens When All Connections Are Busy?
  │     ├── Request Queue / Waiting
  │     ├── Connection Timeout
  │     └── Why Increasing Pool Size Is Not Always the Solution
  │
  ├── 5. Connection Pool Sizing
  │     ├── Why Too Few Connections Is Bad
  │     ├── Why Too Many Connections Is Also Bad
  │     ├── Database Connection Limits
  │     └── Multiple Application Instances Problem
  │
  ├── 6. Common Interview Concepts
  │     ├── Why Not Create a Connection Per Request?
  │     ├── Why Not Keep Unlimited Connections?
  │     ├── Connection Pool vs Database Connection Limit
  │     └── Common Real-World Problems
  │
  └── 7. Practical Considerations
        ├── Connection Leaks
        ├── Idle Connection Timeout
        ├── Long-Running Queries
        └── Monitoring Pool Usage
- ## 3. Distributed Transactions ⭐⭐
  
  Not in depth.
  
  Just enough to know:
- Why normal transactions don't work across multiple services
- What 2PC is (high level)
- Why Saga Pattern exists
  
  You don't need to master it for SDE-1.