### SQL vs NoSQL (HLD Perspective)
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
- ### Indexing
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
			  collapsed:: true
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
				  collapsed:: true
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
-
- Indexing
  │
  ├── 1. Introduction to Indexing
  │     ├── Why Indexing Exists
  │     ├── Full Table Scan
  │     ├── What is an Index?
  │     ├── Index vs Table
  │     ├── How Indexes Improve Performance
  │     └── Read Path With vs Without Index
  │
  ├── 2. Tradeoffs & Best Practices
  │     ├── Read vs Write Tradeoff
  │     ├── Storage Overhead
  │     ├── Index Maintenance
  │     ├── When to Create an Index
  │     └── When NOT to Create an Index
  │
  ├── 3. B+ Tree (Index Internals)
  │     ├── Why B+ Trees?
  │     ├── Structure
  │     ├── Search
  │     ├── Insert
  │     ├── Delete
  │     └── Why Not BST / Hash Table?
  │
  ├── 4. Types of Indexes
  │     ├── Clustered Index
  │     ├── Non-Clustered Index
  │     ├── Unique Index
  │     ├── Composite Index
  │     ├── Covering Index
  │     └── Hash Index (Brief)
  │
  ├── 5. Composite & Covering Indexes
  │     ├── Leftmost Prefix Rule
  │     ├── Column Order
  │     ├── Covering Index
  │     └── Real Examples
  │
  ├── 6. Index Optimization
  │     ├── Cardinality
  │     ├── Selectivity
  │     ├── Why Indexes Are Ignored
  │     └── Optimizer Decisions
  │
  ├── 7. Common Interview Questions
  │
  └── 8. Key Takeaways
	- ├── 3. B+ Tree (Index Internals)
	  │     ├── Why B+ Trees?
	  │     ├── Structure
	  │     ├── Search
	  │     ├── Insert
	  │     ├── Delete
	  │     └── Why Not BST / Hash Table?
- ## 1. Indexing ⭐⭐⭐⭐⭐ 
  
  This is one of the most important topics.
  
  You should understand:
- Why indexes exist
- B+ Trees
- Clustered vs Non-clustered Index
- Composite Indexes
- Covering Indexes
- Index Selectivity
- Why too many indexes are bad
- When indexes are not used
  
  This is asked in almost every backend interview.
  
  ---
- ## 2. Connection Pooling ⭐⭐⭐
  
  Small topic.
  
  30–40 minutes.
  
  But interviewers love asking:
  
  > 
  
  Why don't we open a new DB connection for every request?
  
  ---
- ## 3. Distributed Transactions ⭐⭐
  
  Not in depth.
  
  Just enough to know:
- Why normal transactions don't work across multiple services
- What 2PC is (high level)
- Why Saga Pattern exists
  
  You don't need to master it for SDE-1.