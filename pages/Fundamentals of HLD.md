### Scalability (vertical vs horizontal)
collapsed:: true
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
  collapsed:: true
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
  collapsed:: true
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
- ### Consistency models
  collapsed:: true
	- Consistency models define what a user can expect to see after data is written in a distributed system.
	  
	  Example:
	  
	  ```
	  Primary DB
	     |
	     v
	  Replica DB
	  ```
	  
	  User updates:
	  
	  ```
	  Name = Aditya
	  ```
	  
	  to
	  
	  ```
	  Name = Adi
	  ```
	  
	  Primary updates immediately.
	  
	  Replica may update later.
	  
	  Consistency models define what users should see during this gap.
	  
	  ---
	- ## 1. Strong Consistency
	  collapsed:: true
		- ### Definition
		  
		  After a successful write:
		  
		  ```
		  Every read
		  =
		  Latest value
		  ```
		  
		  No stale data is ever returned.
		  
		  ---
		- ## Example
		  
		  Bank Account:
		  
		  ```
		  Balance = 1000
		  ```
		  
		  Withdraw:
		  
		  ```
		  100
		  ```
		  
		  New balance:
		  
		  ```
		  900
		  ```
		  
		  Immediately after withdrawal:
		  
		  ```
		  Any read
		  ```
		  
		  must return:
		  
		  ```
		  900
		  ```
		  
		  Never:
		  
		  ```
		  1000
		  ```
		  
		  ---
		- ## Where Used
		- Banking
		- Payments
		- Trading Systems
		- Inventory Systems
		  
		  ---
		- ## Why Used
		  
		  Because stale data can cause financial loss or incorrect business operations.
		  
		  ---
		- ## Tradeoff
		  
		  Pros:
		- Always correct
		- No stale reads
		  
		  Cons:
		- Higher latency
		- Lower availability
		- Harder to scale
		  
		  ---
		- ## How It Is Implemented
		- ### Method 1: Read Only From Primary
		  
		  ```
		  Primary
		             |
		      ----------------
		      |              |
		      v              v
		  Replica A      Replica B
		  ```
		  
		  All writes:
		  
		  ```
		  Write -> Primary
		  ```
		  
		  All reads:
		  
		  ```
		  Read -> Primary
		  ```
		  
		  Guarantees latest data.
		  
		  ---
		- ### Method 2: Synchronous Replication
		  
		  Write succeeds only after:
		  
		  ```
		  Primary Updated
		  AND
		  Replica Updated
		  ```
		  
		  Flow:
		  
		  ```
		  Client
		  |
		  Write
		  |
		  Primary
		  |
		  Replica
		  |
		  Success Response
		  ```
		  
		  This ensures every replica has latest data before the request completes.
		  
		  ---
		- ## Interview Example
		  
		  Order Placement
		  
		  ```
		  Customer places order
		  ```
		  
		  Restaurant must immediately see it.
		  
		  Strong Consistency is preferred.
		  
		  ---
	- ## 2. Eventual Consistency
	  collapsed:: true
		- ### Definition
		  
		  After a write:
		  
		  ```
		  Some replicas may be stale
		  ```
		  
		  for a period of time.
		  
		  Eventually:
		  
		  ```
		  All replicas converge
		  ```
		  
		  to the same value.
		  
		  ---
		- ## Example
		  
		  Instagram Like Count
		  
		  Current:
		  
		  ```
		  100 Likes
		  ```
		  
		  You like post.
		  
		  Phone A:
		  
		  ```
		  101 Likes
		  ```
		  
		  Friend's Phone:
		  
		  ```
		  100 Likes
		  ```
		  
		  After a few seconds:
		  
		  ```
		  101 Likes
		  ```
		  
		  for everyone.
		  
		  ---
		- ## Where Used
		- Social Media
		- Analytics
		- Recommendations
		- Notification Counters
		- View Counts
		  
		  ---
		- ## Why Used
		  
		  Because:
		  
		  ```
		  Fast
		  Scalable
		  Highly Available
		  ```
		  
		  are more important than perfect accuracy.
		  
		  ---
		- ## Tradeoff
		  
		  Pros:
		- Highly scalable
		- Lower latency
		- Better availability
		  
		  Cons:
		- Temporary stale data
		- Users may see different values
		  
		  ---
		- ## How It Is Implemented
		  
		  Typical setup:
		  
		  ```
		  Primary
		             |
		             |
		      ----------------
		      |              |
		      v              v
		  Replica A      Replica B
		  ```
		  
		  Write:
		  
		  ```
		  User
		  |
		  Write
		  |
		  Primary
		  ```
		  
		  Immediately return success.
		  
		  Replication happens asynchronously:
		  
		  ```
		  Primary
		  |
		  Later
		  |
		  Replica
		  ```
		  
		  Reads usually go to replicas:
		  
		  ```
		  User
		  |
		  Read
		  |
		  Replica
		  ```
		  
		  This improves scalability.
		  
		  ---
		- ## Interview Example
		  
		  Restaurant List
		  
		  Restaurant updates rating:
		  
		  ```
		  4.5 -> 4.6
		  ```
		  
		  Some users seeing:
		  
		  ```
		  4.5
		  ```
		  
		  for a few seconds is acceptable.
		  
		  Eventual Consistency is preferred.
		  
		  ---
	- ## 3. Read-Your-Writes Consistency
	  collapsed:: true
		- ### Definition
		  
		  After a user performs a write:
		  
		  ```
		  That same user
		  must see the latest value
		  ```
		  
		  immediately.
		  
		  Other users may still see stale data.
		  
		  ---
		- ## Example
		  
		  You update profile:
		  
		  ```
		  Aditya
		  ```
		  
		  to
		  
		  ```
		  Adi
		  ```
		  
		  You refresh profile.
		  
		  You should see:
		  
		  ```
		  Adi
		  ```
		  
		  immediately.
		  
		  Friend may still see:
		  
		  ```
		  Aditya
		  ```
		  
		  for a few seconds.
		  
		  ---
		- ## Why This Exists
		  
		  Without it:
		  
		  ```
		  Click Save
		     |
		  Success Message
		     |
		  Refresh
		     |
		  Old Data Appears
		  ```
		  
		  User thinks:
		  
		  ```
		  Update Failed
		  ```
		  
		  Bad user experience.
		  
		  ---
		- ## Where Used
		- Profile Updates
		- User Settings
		- Dashboards
		- User Preferences
		  
		  ---
		- ## Tradeoff
		  
		  Pros:
		- Excellent user experience
		- Doesn't require full strong consistency
		  
		  Cons:
		- Slightly more application logic
		- More load on primary database
		  
		  ---
		- ## How It Is Implemented
		- ### Method 1: Temporary Primary Reads
		  
		  After a write:
		  
		  ```
		  User
		  |
		  Write
		  |
		  Primary
		  ```
		  
		  Store flag:
		  
		  ```
		  redis.set(
		  `user:${userId}:recent_write`,
		  true,
		  30
		  );
		  ```
		  
		  ---
		  
		  For future reads:
		  
		  ```
		  if(recentWriteExists(userId))
		  {
		   readFromPrimary();
		  }
		  else
		  {
		   readFromReplica();
		  }
		  ```
		  
		  Flow:
		  
		  ```
		  Write
		  |
		  Read Primary
		  |
		  Replicas Catch Up
		  |
		  Read Replica Again
		  ```
		  
		  ---
		- ### Method 2: Session Flag
		  
		  After write:
		  
		  ```
		  session.lastWriteTime = Date.now();
		  ```
		  
		  For next few requests:
		  
		  ```
		  Route Reads
		   |
		  Primary
		  ```
		  
		  After replicas sync:
		  
		  ```
		  Route Reads
		   |
		  Replica
		  ```
		  
		  ---
		- ### Method 3: Version Tracking
		  
		  Write generates:
		  
		  ```
		  Version = 100
		  ```
		  
		  User's next read requires:
		  
		  ```
		  Version >= 100
		  ```
		  
		  If replica only has:
		  
		  ```
		  Version = 99
		  ```
		  
		  route request to primary.
		  
		  ---
		- ## Interview Answer
		  
		  If asked:
		  
		  "How would you implement Read-Your-Writes consistency?"
		  
		  Answer:
		  
		  After a user performs a write, I would temporarily route that user's subsequent reads to the primary database. This can be implemented using a session flag, Redis cache entry, or version tracking. Once replicas are expected to be caught up, reads can return to replicas.
		  
		  ---
	- # Quick Comparison
	  
	  | Model | Latest Data Guaranteed? | Scalability | Typical Use Cases |
	  | ---- | ---- | ---- |
	  | Strong Consistency | Yes | Lower | Banking, Payments |
	  | Eventual Consistency | Eventually | High | Social Media, Analytics |
	  | Read-Your-Writes | For Same User | Medium-High | Profiles, Settings |
	  
	  ---
	- # Interview Rule of Thumb
	  
	  If stale data can cause business problems:
	  
	  ```
	  Use Strong Consistency
	  ```
	  
	  If stale data is acceptable:
	  
	  ```
	  Use Eventual Consistency
	  ```
	  
	  If only the user who updated data must immediately see it:
	  
	  ```
	  Use Read-Your-Writes
	  ```
	- CAP
	  │
	  | ── CP
	  │   └── Usually Strong Consistency
	  │
	  └── AP
	      ├── Eventual Consistency
	      ├── Read-Your-Writes
	      ├── Monotonic Reads
	      └── Other weaker models
- ### Caching
  collapsed:: true
	- ## 1. Why Cache Exists
	  
	  A cache is a fast storage layer (usually in-memory) placed between the application and the database.
	  
	  ```text
	  Application
	     |
	   Cache
	     |
	  Database
	  ```
	  
	  ---
	- ### Problems Without Cache
	  
	  Every request hits the database.
	  
	  Example:
	  
	  ```text
	  GET /profile/123
	  ```
	  
	  ```text
	  Application
	     |
	  Database
	  ```
	  
	  If the same profile is requested 100,000 times:
	  
	  ```text
	  100,000 Database Queries
	  ```
	  
	  This increases:
	  
	  * Database load
	  * Response time
	  * Infrastructure cost
	  
	  ---
	- ## Benefits of Cache
		- ### Reduce Database Load
		  
		  Instead of querying DB repeatedly:
		  
		  ```text
		  Request
		   |
		  Cache
		   |
		  Return
		  ```
		  
		  Database is bypassed.
		  
		  ---
		- ### Improve Latency
		  
		  Approximate numbers:
		  
		  ```text
		  Redis Cache   -> ~1 ms
		  
		  Database      -> ~10-100 ms
		  ```
		  
		  Cache is significantly faster because data is stored in memory.
		  
		  ---
	- ## 2. Cache Hit
		- Requested data exists in cache.
		- ## Flow
		  
		  ```text
		  Request
		   |
		  Cache
		   |
		  Found
		   |
		  Return
		  ```
		  
		  Database is never queried.
		  
		  ---
		- ## Example
		  
		  ```text
		  profile:123
		  ```
		  
		  already exists in Redis.
		  
		  Request:
		  
		  ```text
		  GET /profile/123
		  ```
		  
		  returns directly from cache.
		  
		  ---
		- ## Benefit
		  
		  * Fastest response
		  * No DB load
		  
		  ---
	- ## 3. Cache Miss
		- Requested data does not exist in cache.
		- ## Flow
		  
		  ```text
		  Request
		   |
		  Cache
		   |
		  Not Found
		   |
		  Database
		   |
		  Store In Cache
		   |
		  Return
		  ```
		  
		  ---
		- ## Example
		  
		  ```text
		  GET /profile/123
		  ```
		  
		  Cache:
		  
		  ```text
		  profile:123
		  ```
		  
		  does not exist.
		  
		  Application queries DB, stores result in cache, then returns response.
		  
		  ---
	- ## 4. Cache Aside (Most Important)
		- Most common caching strategy.
		  Application manages cache explicitly.
		- ## Read Flow
		  
		  ```text
		  Request
		   |
		  Cache
		   |
		  Found?
		  /     \
		  Yes     No
		  |       |
		  Return   DB
		          |
		      Store In Cache
		          |
		       Return
		  ```
		  
		  ---
		- ## Example
		  
		  ```javascript
		  let user = await redis.get(userId);
		  
		  if(user)
		  {
		   return user;
		  }
		  
		  user = await db.findUser(userId);
		  
		  await redis.set(userId, user);
		  
		  return user;
		  ```
		  
		  ---
		- ## Write Flow
		  
		  Update database first.
		  
		  Then invalidate cache.
		  
		  ```text
		  Update Request
		      |
		  Update DB
		      |
		  Delete Cache
		  ```
		  
		  ---
		- ## Example
		  
		  ```javascript
		  await db.updateUser(...);
		  
		  await redis.del(userId);
		  ```
		  
		  Next read becomes:
		  
		  ```text
		  Cache Miss
		  ```
		  
		  and fresh data is loaded.
		  
		  ---
		- ## Why Cache Aside Is Popular
		  
		  * Simple
		  * Flexible
		  * Application has full control
		  * Most commonly used in real systems
		  
		  ---
	- ## 5. Read Through
		- Application only interacts with cache.
		  Cache automatically loads data from DB when needed.
		- ## Flow
		  
		  ```text
		  Application
		      |
		    Cache
		      |
		  Cache Miss
		      |
		  Database
		      |
		  Store In Cache
		      |
		  Return
		  ```
		  
		  ---
		- ## Key Difference
		  
		  Application does NOT query DB directly.
		  
		  Cache layer handles:
		  
		  * Cache lookup
		  * DB lookup
		  * Cache population
		  
		  ---
		- ## Benefit
		  
		  Cleaner application code.
		  
		  ---
		- ## Drawback
		  
		  More complex cache infrastructure.
		  
		  ---
	- ## 6. Write Through
		- Cache and database are updated together.
		- ## Flow
		  
		  ```text
		  Application
		      |
		    Cache
		      |
		  Database
		  ```
		  
		  ---
		- ## Example
		  
		  ```text
		  Update User
		     |
		  Update Cache
		     |
		  Update DB
		  ```
		  
		  ---
		- ## Benefit
		  
		  Cache remains fresh.
		  
		  Reads almost always hit cache.
		  
		  ---
		- ## Drawback
		  
		  Slower writes.
		  
		  Every write updates:
		  
		  ```text
		  Cache
		  +
		  Database
		  ```
		  
		  ---
		- # Cache Aside vs Read Through vs Write Through
		  
		  | Strategy      | Who Loads Data? | Who Updates Cache?  |
		  | ------------- | --------------- | ------------------- |
		  | Cache Aside   | Application     | Application         |
		  | Read Through  | Cache Layer     | Cache Layer         |
		  | Write Through | Cache Layer     | Cache + DB Together |
		  
		  ---
	- ## 7. Cache Invalidation
		- Removing or updating stale data from cache when underlying data changes.
		- ## Why Needed?
		  
		  Suppose:
		  
		  Database:
		  
		  ```text
		  Name = Adi
		  ```
		  
		  Cache:
		  
		  ```text
		  Name = Aditya
		  ```
		  
		  Now cache contains stale data.
		  
		  ---
		- ## Example
		  
		  User updates profile.
		  
		  ```text
		  Update User
		      |
		  Update DB
		      |
		  Delete Cache
		  ```
		  
		  ---
		  
		  Next request:
		  
		  ```text
		  Cache Miss
		  ```
		  
		  Fresh data comes from DB.
		  
		  ---
		- ## Why Is Cache Invalidation Hard?
		  
		  Because multiple copies of data exist:
		  
		  ```text
		  Database
		  Cache
		  Replicas
		  ```
		  
		  All copies must remain synchronized.
		  
		  Failures can occur between:
		  
		  ```text
		  Update DB
		  Delete Cache
		  ```
		  
		  which may leave stale cache entries.
		  
		  ---
		- ## Interview Fact
		  
		  One of the most famous statements in Computer Science:
		  
		  > There are only two hard things in Computer Science:
		  >
		  > Cache invalidation and naming things.
		  
		  ---
	- ## 8. Tradeoffs
		- ## Advantages
			- ### Faster Reads
			  
			  Frequently accessed data is served from memory.
			  
			  ---
			- ### Reduced Database Load
			  
			  Fewer database queries.
			  
			  ---
			- ### Better Scalability
			  
			  Database can support more users.
			  
			  ---
			- ### Lower Latency
			  
			  Users get responses faster.
			  
			  ---
		- ## Disadvantages
			- ### Stale Data
			  
			  Cache may not reflect latest database state.
			  
			  ---
			- ### Cache Invalidation Complexity
			  
			  Keeping cache synchronized with DB is difficult.
			  
			  ---
			- ### Additional Infrastructure
			  
			  Need Redis, Memcached, or another cache system.
			  
			  ---
			- ### More Operational Complexity
			  
			  Monitoring and debugging become harder.
			  
			  ---
	- ## 9. Interview Cheat Sheet
		- ## When Should I Use Cache?
		  
		  Use caching when:
		  
		  * Reads are much higher than writes.
		  * Same data is requested frequently.
		  * Database becomes a bottleneck.
		  
		  Examples:
		  
		  * User Profiles
		  * Product Catalogs
		  * Restaurant Listings
		  * Dashboard Data
		  * Configuration Data
		  
		  ---
		- ## When Should I Avoid Cache?
		  
		  Avoid caching when:
		  
		  * Data changes constantly.
		  * Strong consistency is required.
		  * Stale data is unacceptable.
		  
		  Examples:
		  
		  * Bank Balance
		  * Payment Processing
		  * Critical Transactions
		  
		  ---
		- # Key Takeaways
		  
		  1. Cache improves read performance and reduces DB load.
		  2. Cache Hit = data found in cache.
		  3. Cache Miss = data loaded from DB.
		  4. Cache Aside is the most common strategy.
		  5. Read Through lets cache fetch data automatically.
		  6. Write Through updates cache and DB together.
		  7. Cache invalidation is required to prevent stale data.
		  8. Caching improves performance but introduces consistency challenges.
- ### Load Balancer
  collapsed:: true
	- ## 1. Why Load Balancer Exists
		- As traffic grows, a single server becomes a bottleneck.
		  
		  Without Load Balancer:
		  
		  ```
		  Users
		   |
		   v
		  Server 1
		  ```
		  
		  Problems:
		- CPU exhaustion
		- Memory exhaustion
		- Slow responses
		- Single point of failure
		  
		  ---
		  
		  Load Balancer allows traffic to be distributed across multiple servers.
		  
		  ```
		  Users
		                   |
		                   v
		          +----------------+
		          | Load Balancer  |
		          +----------------+
		            /     |      \
		           /      |       \
		          v       v        v
		  
		      Server1  Server2  Server3
		  ```
		  
		  ---
		- ## A. Scalability
		  
		  More traffic?
		  
		  Add more servers.
		  
		  ```
		  Server1
		  Server2
		  Server3
		  Server4
		  ```
		  
		  Load Balancer distributes traffic automatically.
		  
		  ---
		- ## B. High Availability
		  
		  If one server crashes:
		  
		  ```
		  Server1 ✅
		  Server2 ❌
		  Server3 ✅
		  ```
		  
		  Load Balancer stops sending traffic to Server2.
		  
		  Application remains available.
		  
		  ---
		- ## C. Performance
		  
		  Instead of:
		  
		  ```
		  1000 Requests
		      |
		   Server1
		  ```
		  
		  Traffic becomes:
		  
		  ```
		  333 -> Server1
		  333 -> Server2
		  334 -> Server3
		  ```
		  
		  Each server handles less work.
		  
		  Response times improve.
		  
		  ---
	- ## 2. What Is A Load Balancer?
		- A Load Balancer is a component that distributes incoming requests across multiple servers.
		- ## Diagram
		  
		  ```
		  Client
		   |
		   v
		  Load Balancer
		   |
		   +--> Server1
		   |
		   +--> Server2
		   |
		   +--> Server3
		  ```
		  
		  ---
		- ## Responsibilities
		- Distribute traffic
		- Detect unhealthy servers
		- Improve availability
		- Improve scalability
		- Prevent server overload
		  
		  ---
	- ## 3. Health Checks
		- ## Problem
		  
		  Suppose:
		  
		  ```
		  Server1 ✅
		  Server2 ❌
		  Server3 ✅
		  ```
		  
		  Without health checks:
		  
		  Users may be routed to Server2.
		  
		  Requests fail.
		  
		  ---
		- ## Solution
		  
		  Load Balancer periodically checks:
		  
		  ```
		  GET /health
		  ```
		  
		  Expected response:
		  
		  ```
		  {
		  "status": "ok"
		  }
		  ```
		  
		  ---
		- ## Flow
		  
		  ```
		  Load Balancer
		      |
		  Health Check
		      |
		      v
		  Server
		  ```
		  
		  ---
		  
		  If health check fails:
		  
		  ```
		  Server Removed
		  From Rotation
		  ```
		  
		  Traffic is routed only to healthy servers.
		  
		  ---
	- ## 4. Distribution Algorithms
		- Load Balancer needs a strategy to decide:
		  
		  ```
		  Which server gets the next request?
		  ```
		  
		  ---
		- ## A. Round Robin
		  
		  Most common algorithm.
		  
		  Requests are distributed sequentially.
		  
		  ---
		  
		  Example:
		  
		  ```
		  Request1 -> Server1
		  Request2 -> Server2
		  Request3 -> Server3
		  Request4 -> Server1
		  Request5 -> Server2
		  Request6 -> Server3
		  ```
		  
		  ---
		- ### Benefits
		- Very simple
		- Fair distribution
		  
		  ---
		- ### Drawbacks
		  
		  Assumes all servers have equal capacity.
		  
		  ---
		- ## B. Least Connections
		  
		  Send traffic to the server with the fewest active connections.
		  
		  ---
		  
		  Example:
		  
		  ```
		  Server1 -> 100 Connections
		  
		  Server2 -> 15 Connections
		  
		  Server3 -> 20 Connections
		  ```
		  
		  Next request:
		  
		  ```
		  → Server2
		  ```
		  
		  ---
		- ### Benefits
		  
		  Works well when request duration varies.
		  
		  ---
		- ### Drawbacks
		  
		  Slightly more complex.
		  
		  ---
		- ## C. Weighted Round Robin
		  
		  Some servers are more powerful.
		  
		  Example:
		  
		  ```
		  Server1 Weight = 3
		  
		  Server2 Weight = 1
		  ```
		  
		  Traffic:
		  
		  ```
		  75% -> Server1
		  25% -> Server2
		  ```
		  
		  ---
		- ### Benefits
		  
		  Utilizes stronger machines effectively.
		  
		  ---
		- ### Drawbacks
		  
		  Weights need tuning.
		  
		  ---
	- ## 5. Benefits
		- ## Scalability
		  
		  Add more servers horizontally.
		  
		  ---
		- ## High Availability
		  
		  Server failures do not bring down the system.
		  
		  ---
		- ## Better Performance
		  
		  Traffic is spread across servers.
		  
		  ---
		- ## Easier Maintenance
		  
		  Can remove one server for deployment.
		  
		  Users remain unaffected.
		  
		  ---
		- ## Fault Tolerance
		  
		  Single server failures become less impactful.
		  
		  ---
	- ## 6. Tradeoffs
		- ## Additional Infrastructure
		  
		  Need:
		  
		  ```
		  Load Balancer
		  ```
		  
		  in addition to application servers.
		  
		  ---
		- ## Cost
		  
		  Managed load balancers cost money.
		  
		  ---
		- ## Additional Complexity
		  
		  Need:
		- Health checks
		- Monitoring
		- Configuration
		  
		  ---
		- ## Potential Bottleneck
		  
		  If the Load Balancer fails:
		  
		  ```
		  Users
		  |
		  Load Balancer ❌
		  ```
		  
		  Entire application becomes unavailable.
		  
		  Modern systems use redundant load balancers.
		  
		  ---
	- ## 7. L4 vs L7 Load Balancer
		- ### Layer 4 Load Balancer
		  collapsed:: true
			- ## What Is Layer 4?
			  
			  Layer 4 = Transport Layer (TCP/UDP)
			  
			  Layer 4 LB routes traffic using:
			  
			  ```
			  IP Address
			  Port
			  TCP Connection
			  ```
			  
			  It does NOT inspect HTTP requests.
			  
			  ---
			- ## Diagram
			  
			  ```
			  Client
			   |
			  TCP Connection
			   |
			  Layer 4 LB
			   |
			   +--> Server1
			   +--> Server2
			  ```
			  
			  ---
			- ## Characteristics
			- Very fast
			- Low overhead
			- High throughput
			  
			  ---
			- ## Example
			  
			  ```
			  AWS Network Load Balancer
			  ```
			  
			  ---
			- ## Limitation
			  
			  Cannot inspect:
			  
			  ```
			  /api/users
			  
			  /api/orders
			  ```
			  
			  It only sees:
			  
			  ```
			  IP
			  Port
			  Protocol
			  ```
			  
			  ---
		- ### Layer 7 Load Balancer
		  collapsed:: true
			- ## What Is Layer 7?
			  
			  Layer 7 = Application Layer (HTTP/HTTPS)
			  
			  Layer 7 LB understands:
			  
			  ```
			  URL
			  Headers
			  Cookies
			  HTTP Method
			  ```
			  
			  ---
			- ## Example
			  
			  ```
			  /api/users
			  ```
			  
			  and
			  
			  ```
			  /api/orders
			  ```
			  
			  can be routed differently.
			  
			  ---
			- ## Diagram
			  
			  ```
			  Client
			   |
			  HTTP Request
			   |
			  Layer 7 LB
			   |
			   +--> User Service
			   |
			   +--> Order Service
			  ```
			  
			  ---
			- ## Example Rules
			  
			  ```
			  /api/users
			      |
			      +--> User Service
			  
			  /api/orders
			      |
			      +--> Order Service
			  ```
			  
			  ---
			- ## Characteristics
			- Smart routing
			- SSL termination
			- Header-based routing
			- Cookie-based routing
			  
			  ---
			- ## Example
			  
			  ```
			  AWS Application Load Balancer
			  NGINX
			  HAProxy
			  ```
			  
			  ---
			- ## Limitation
			  
			  More processing overhead compared to Layer 4.
			  
			  ---
		- ### Layer 4 vs Layer 7
		  
		  | Feature | Layer 4 | Layer 7 |
		  | ---- | ---- | ---- |
		  | Works On | TCP/UDP | HTTP/HTTPS |
		  | Sees URL? | No | Yes |
		  | Sees Headers? | No | Yes |
		  | Routing Flexibility | Low | High |
		  | Performance | Faster | Slightly Slower |
		  | Complexity | Lower | Higher |
		  
		  ---
	- ## 8. Interview Questions
		- ### Why do we need a Load Balancer?
		  
		  To distribute traffic across multiple servers, improve scalability, increase availability, and prevent server overload.
		  
		  ---
		- ### Why not DNS Round Robin?
		  
		  DNS Round Robin distributes traffic but:
		- Doesn't know server health
		- Doesn't know server load
		- Cannot perform intelligent routing
		- Affected by DNS caching
		  
		  Load Balancers make real-time routing decisions.
		  
		  ---
		- ### When would you choose Layer 4?
		  
		  When maximum performance and throughput are more important than request-level routing.
		  
		  Example:
		  
		  ```
		  Database Traffic
		  TCP Services
		  ```
		  
		  ---
		- ### When would you choose Layer 7?
		  
		  When routing decisions depend on:
		  
		  ```
		  URL
		  Headers
		  Cookies
		  ```
		  
		  Example:
		  
		  ```
		  Microservices
		  Web Applications
		  ```
		  
		  ---
		- # Key Takeaways
		- Load Balancer distributes traffic across multiple servers.
		- Main goals:
			- Scalability
			- High Availability
			- Better Performance
		- Health checks remove failed servers.
		- Common algorithms:
			- Round Robin
			- Least Connections
			- Weighted Round Robin
		- Layer 4 operates at TCP/UDP level.
		- Layer 7 operates at HTTP level.
		- Most modern web applications use Layer 7 Load Balancers.
		- Load Balancers are fundamental building blocks of scalable systems.
- ### Rate Limiting
  collapsed:: true
	- ## 1. Why Rate Limiting Exists
		- Rate Limiting restricts how many requests a client can make within a given time period.
		  
		  Without rate limiting, a single user, bot, or buggy application can overwhelm the system and affect all users.
		- ## A. Abuse Prevention
		  
		  Protects APIs from:
		- Brute force attacks
		- Scrapers
		- Bots
		- DDoS-style traffic
		  
		  Example:
		  
		  ```
		  Login API
		  
		  Limit:
		  5 requests/minute
		  ```
		  
		  Prevents password guessing attacks.
		  
		  ---
		- ## B. Infrastructure Protection
		  
		  Without limits:
		  
		  ```
		  1 User
		   |
		  100,000 Requests
		   |
		  Server Crash
		  ```
		  
		  Rate limiting prevents excessive load from reaching application servers, databases, caches, and downstream services.
		  
		  ---
		- ## C. Fair Usage
		  
		  Ensures no single client consumes a disproportionate amount of system resources.
		  
		  Example:
		  
		  ```
		  100 requests/minute/user
		  ```
		  
		  All users receive a fair share of resources.
		  
		  ---
	- ## 2. Core Components
		- Every rate limiter requires three things.
		- ## A. Identity
		  
		  Who are we limiting?
		  
		  Examples:
		  
		  ```
		  User ID
		  IP Address
		  API Key
		  Tenant ID
		  ```
		  
		  ---
		- ## B. Limit
		  
		  Maximum allowed requests.
		  
		  Examples:
		  
		  ```
		  100 requests/minute
		  
		  1000 requests/hour
		  
		  10 requests/second
		  ```
		  
		  ---
		- ## C. Time Window
		  
		  The duration over which requests are counted.
		  
		  Examples:
		  
		  ```
		  Per Second
		  
		  Per Minute
		  
		  Per Hour
		  
		  Per Day
		  ```
		  
		  ---
	- ## 3. Rate Limiting Algorithms [[Rate Limiter]]
		- Detailed notes maintained separately.
		- ## Fixed Window
		  
		  Tracks requests within a fixed time interval.
		  
		  Example:
		  
		  ```
		  100 requests/minute
		  ```
		  
		  Counter resets every minute.
		- ### Pros
		- Simple
		- Easy to implement
		- ### Cons
		- Boundary burst problem
		  
		  ---
		- ## Sliding Window
		  
		  Counts requests over the previous rolling time window.
		  
		  Example:
		  
		  ```
		  Last 60 seconds
		  ```
		  
		  instead of:
		  
		  ```
		  Current minute
		  ```
		- ### Pros
		- More accurate
		- Eliminates boundary issue
		- ### Cons
		- Higher memory usage
		  
		  ---
		- ## Sliding Window Counter
		  
		  Hybrid approach.
		  
		  Combines:
		  
		  ```
		  Fixed Window
		  +
		  Sliding Window
		  ```
		  
		  Provides near-sliding-window accuracy with lower resource consumption.
		- ### Pros
		- Good balance
		- Widely used
		- ### Cons
		- Slight approximation
		  
		  ---
		- ## Token Bucket
		  
		  Requests consume tokens from a bucket.
		  
		  Tokens replenish at a fixed rate.
		- ### Pros
		- Allows short traffic bursts
		- Flexible
		- ### Cons
		- More complex than Fixed Window
		  
		  ---
		- ## Leaky Bucket
		  
		  Requests enter a queue and leave at a constant rate.
		  
		  Traffic becomes smooth and predictable.
		- ### Pros
		- Smooth traffic flow
		- Good for downstream protection
		- ### Cons
		- Bursts may be delayed or dropped
		  
		  ---
	- ## 4. Redis Usage
		- ## Why Redis Is Commonly Used For Rate Limiting
		  
		  In a distributed system:
		  
		  ```
		  Load Balancer
		                   |
		        ---------------------
		        |         |         |
		       App1      App2      App3
		  ```
		  
		  Requests from the same user may hit different servers.
		  
		  ---
		- ### Problem
		  
		  If counters are stored in server memory:
		  
		  ```
		  App1 -> Counter = 50
		  
		  App2 -> Counter = 20
		  
		  App3 -> Counter = 30
		  ```
		  
		  Each server has a different view.
		  
		  Rate limiting becomes inaccurate.
		  
		  ---
		- ### Solution
		  
		  Use a shared store.
		  
		  ```
		  Load Balancer
		                   |
		        ---------------------
		        |         |         |
		       App1      App2      App3
		                  |
		                  v
		               Redis
		  ```
		  
		  All servers update the same counter.
		  
		  ---
		- ## Why Redis?
		- ### In-Memory
		  
		  Very fast.
		  
		  Typically:
		  
		  ```
		  Sub-millisecond
		  to
		  few milliseconds
		  ```
		  
		  operations.
		  
		  ---
		- ### Atomic Operations
		  
		  Redis supports atomic commands such as:
		  
		  ```
		  INCR
		  INCRBY
		  EXPIRE
		  ```
		  
		  Useful for safely incrementing counters.
		  
		  ---
		- ### TTL Support
		  
		  Redis can automatically expire keys.
		  
		  Example:
		  
		  ```
		  user:123:requests
		  ```
		  
		  expires after:
		  
		  ```
		  60 seconds
		  ```
		  
		  No manual cleanup required.
		  
		  ---
		- ### Shared Across Multiple Servers
		  
		  Perfect for horizontally scaled systems.
		  
		  ---
		- ## Typical Flow
		  
		  ```
		  Request
		   |
		  Check Redis Counter
		   |
		  Limit Exceeded?
		   |
		  Yes ---> 429 Too Many Requests
		  
		  No
		   |
		  Increment Counter
		   |
		  Allow Request
		  ```
		  
		  ---
	- ## 5. Real World Examples
		- ## Login API
		  
		  Prevent brute force attacks.
		  
		  ```
		  5 attempts/minute
		  ```
		  
		  ---
		- ## OTP API
		  
		  Prevent SMS abuse.
		  
		  ```
		  3 OTP requests/hour
		  ```
		  
		  ---
		- ## Public APIs
		  
		  Protect infrastructure.
		  
		  ```
		  100 requests/minute/API Key
		  ```
		  
		  ---
		- ## Payment APIs
		  
		  Prevent accidental duplicate requests.
		  
		  ```
		  Limited requests per customer
		  ```
		  
		  ---
		- ## AI APIs
		  
		  Examples:
		  
		  ```
		  Requests Per Minute (RPM)
		  
		  Tokens Per Minute (TPM)
		  ```
		  
		  ---
	- ## 6. Tradeoffs
		- ## Benefits
			- ### Abuse Prevention
			  
			  Protects against malicious traffic.
			  
			  ---
			- ### Infrastructure Protection
			  
			  Prevents server overload.
			  
			  ---
			- ### Fair Resource Usage
			  
			  Ensures equitable access.
			  
			  ---
			- ### Cost Control
			  
			  Reduces unnecessary infrastructure consumption.
			  
			  ---
			- ### Improved Reliability
			  
			  Protects downstream services and databases.
			  
			  ---
		- ## Drawbacks
			- ### Additional Infrastructure
			  
			  Often requires Redis or another distributed store.
			  
			  ---
			- ### Operational Complexity
			  
			  Need monitoring and tuning.
			  
			  ---
			- ### False Positives
			  
			  Legitimate users may occasionally hit limits.
			  
			  ---
			- ### User Experience Impact
			  
			  Overly aggressive limits can frustrate users.
			  
			  ---
	- ## 7. Interview Cheat Sheet
		- ### Why Do We Need Rate Limiting?
			- Prevent abuse
			- Protect infrastructure
			- Ensure fair usage
			- Control costs
			  
			  ---
		- ### What Are The Core Components?
		  
		  ```
		  Identity
		  Limit
		  Time Window
		  ```
		  
		  ---
		- ### Why Redis For Rate Limiting?
			- Fast
			- Shared across servers
			- Supports TTL
			- Atomic counters
			  
			  ---
		- ### What Happens When Limit Is Exceeded?
		  
		  Return:
		  
		  ```
		  429 Too Many Requests
		  ```
		  
		  ---
		- ### Common Algorithms
			- Fixed Window
			- Sliding Window
			- Sliding Window Counter
			- Token Bucket
			- Leaky Bucket
			  
			  ---
		- # Key Takeaways
			- Rate Limiting controls request volume over time.
			- It protects systems from abuse and overload.
			- Identity, limit, and time window form the foundation of every rate limiter.
			- Redis is the most common backing store in distributed systems.
			- Token Bucket and Sliding Window Counter are commonly used in production systems.
			- Exceeded requests typically return HTTP 429.
			- Rate Limiting is a core building block in scalable distributed systems.