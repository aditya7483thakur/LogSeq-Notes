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
-
- 1. Replication (VERY IMPORTANT)
  2. Read Replicas
  3. Primary-Replica Architecture
  4. Sharding
  5. Partitioning
  6. Indexing