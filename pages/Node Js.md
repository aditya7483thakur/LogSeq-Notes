## Architecture Of Node JS
	- ```javascript
	  Operating System
	  │
	  └── Node Process
	        │
	        ├── Main JavaScript Thread ⭐
	        │      │
	        │      ├── Express/NestJS
	        │      ├── BullMQ Worker Objects
	        │      ├── HTTP Callbacks
	        │      ├── Promise Callbacks
	        │      ├── Redis Callbacks
	        │      └── Timer Callbacks
	        │
	        ├── libuv Thread Pool (Default: 4 Threads)
	        │      ├── File System
	        │      ├── Crypto
	        │      ├── DNS
	        │      └── Native Blocking Tasks
	        │
	        ├── V8 GC Thread(s)
	        ├── V8 Compiler Thread(s)
	        └── Worker Thread(s) (Optional - Created by You)
	  ```
	- # One thing that trips up many developers 
	  
	  Suppose you write:
	  
	  ```
	  for (let i = 0; i < 1_000_000_000; i++) {}
	  ```
	  
	  A common misconception is:
	  
	  "The libuv threads will help."
	  
	  **They won't.**
	  
	  That loop is pure JavaScript.
	  
	  It executes entirely on the **Main JavaScript Thread**.
	  
	  The libuv thread pool **cannot take over arbitrary JavaScript execution**.
	  
	  If you want that loop to run in parallel, you must explicitly create a **Worker Thread** (using the `worker_threads` module) or move the work to another Node.js process.
	-
	- ###  How these works
		- ### Process
			- Created by the Operating System.
			- Has its own memory, event loop, and threads.
		- ### Main JavaScript Thread
			- Exactly one by default.
			- Executes **all JavaScript**.
		- ### libuv Thread Pool
			- 4 threads by default (configurable).
			- Handles blocking native operations.
			- **Does not execute your JavaScript.**
		- ### V8 Background Threads
			- Used internally for garbage collection and JIT compilation.
			- Not used for your application logic.
		- ### Worker Threads
			- Created explicitly by your code.
			- Execute JavaScript in parallel with the main thread.
			- Used for CPU-intensive work.
		- ### BullMQ Worker
			- Just a JavaScript object.
			- Registers callbacks for Redis jobs.
			- Its callbacks are executed on the **Main JavaScript Thread**, unless you explicitly offload work to Worker Threads.
-