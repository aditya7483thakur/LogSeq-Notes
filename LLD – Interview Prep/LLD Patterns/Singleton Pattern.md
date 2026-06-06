### Problem it solves
	- Ensure **only one instance** of a class exists across the application and provide a **global access point** to it.
	- Used when a **shared resource** must be centrally controlled.
-
- ### Bad Design
	- ### Example (Async-unsafe Singleton)
	  ```tsx
	  class DatabaseConnection {
	    private static instance: DatabaseConnection | null = null;

	    private constructor() {}

	    static async getInstance(): Promise<DatabaseConnection> {
	      if (!DatabaseConnection.instance) {
	        await new Promise((r) => setTimeout(r, 100));
	        DatabaseConnection.instance = new DatabaseConnection();
	      }
	      return DatabaseConnection.instance;
	    }
	  }
	  ```
-
- ### Why this is a problem
	- ### Java (Multithreading)
		- Multiple threads may pass `if (instance == null)` simultaneously
		- Results in **multiple instances**
		- Breaks Singleton guarantee
	- ### JavaScript (Async/Await)
		- JS is single-threaded
		- BUT `await` introduces a **logical race**
		- Multiple async calls can pass the null check before instance is created
	- 🚨 **Singleton is broken in both cases**
-
- ### Good Design (Without Thread / Async Safety)
	- > Works ONLY in single-threaded, synchronous scenarios
	- ```tsx
	  class Logger {
	    private static instance: Logger;

	    private constructor() {}

	    static getInstance(): Logger {
	      if (!Logger.instance) {
	        Logger.instance = new Logger();
	      }
	      return Logger.instance;
	    }
	  }
	  ```
	- ❌ **Still unsafe** in Java multithreading and JS async code
-
- ### Thread Safety Issue Explained (Java)
	- ### Problem
	  ```java
	  if (instance == null) {
	    instance = new Logger();
	  }
	  ```
	- Two threads:
		- Both see `instance == null`
		- Both create new objects ❌
-
- ### Async/Await Race Condition Explained (JavaScript)
	- ### Problem
	  ```tsx
	  if (!instance) {
	    await asyncTask();
	    instance = new Obj();
	  }
	  ```
	- Multiple callers:
		- All pass `if` before `await` finishes
		- All create instances ❌
-
- ### Good Design WITH Safety (Java + JS)
	- ### ✅ Java - Thread-Safe Singleton (Double-Checked Locking)
	  ```java
	  class Logger {
	    private static volatile Logger instance;

	    private Logger() {}

	    public static Logger getInstance() {
	        if (instance == null) {
	            synchronized (Logger.class) {
	                if (instance == null) {
	                    instance = new Logger();
	                }
	            }
	        }
	        return instance;
	    }
	  }
	  ```
	- ✔ Thread safe
	- ✔ Performance optimized
	- ### ✅ JavaScript / TypeScript - Async-Safe Singleton (Promise Lock)
	  ```tsx
	  class DatabaseConnection {
	    private static instancePromise: Promise<DatabaseConnection> | null = null;

	    private constructor() {}

	    static getInstance(): Promise<DatabaseConnection> {
	      if (!DatabaseConnection.instancePromise) {
	        DatabaseConnection.instancePromise = (async () => {
	          await new Promise((r) => setTimeout(r, 100));
	          return new DatabaseConnection();
	        })();
	      }
	      return DatabaseConnection.instancePromise;
	    }
	  }
	  ```
	- ✔ Async safe
	- ✔ Prevents race conditions
	- ✔ Only one instance created
-
- ### Usage
	- ### Java
	  ```java
	  Logger logger = Logger.getInstance();
	  logger.log("Hello");
	  ```
	- ### TypeScript
	  ```tsx
	  const db = await DatabaseConnection.getInstance();
	  db.query("SELECT * FROM users");
	  ```
-
- ### Key Intuition ⭐ (MOST IMPORTANT)
	- > **Singleton is controlled global state, not a magic single object**
	- ### Mental model
		- Java → **lock the critical section**
		- JS → **share the initialization promise**
-
- ### When to use it
	- Logger
	- Configuration
	- Cache manager
	- Connection pool
	- Shared, read-heavy resources
-
- ### When NOT to use it
	- Business/domain logic
	- Request/user-specific data
	- When Dependency Injection is sufficient
	- When testing requires multiple instances
	- > **Interview-safe line:**
		- > "I avoid Singleton unless a truly shared instance is required."
-
- ### Common Interview Traps ❌
	- Making everything Singleton
	- Ignoring thread / async safety
	- Using Singleton instead of DI
	- Hiding dependencies inside `getInstance()`
	- Saying "JS is single-threaded so Singleton is always safe" ❌
-
- ### Bonus Interview Point ⭐
	- In Node.js:
	  ```tsx
	  export const logger = new Logger();
	  ```
	- Module caching already gives Singleton behavior.
