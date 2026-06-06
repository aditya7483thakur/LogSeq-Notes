### Problem it solves
	- You have **multiple steps/handlers** that must process a request **one after another**, and you don't want:
		- big `if / else` blocks
		- one class knowing all steps
		- rigid order hard-coded in one place
-
- ### ❌ Bad Design
	- ### All logic inside one method
	  ```tsx
	  class RequestHandler {
	    handle(req: any) {
	      if (!req.user) {
	        throw new Error("Unauthenticated");
	      }

	      if (!req.user.isAdmin) {
	        throw new Error("Unauthorized");
	      }

	      if (req.body.size > 1024) {
	        throw new Error("Payload too large");
	      }

	      console.log("Request processed");
	    }
	  }
	  ```
-
- ### Why this is a problem
	- Violates **Single Responsibility**
	- Adding/removing a step means **editing this class**
	- Order is **hard-coded**
	- Logic becomes unreadable as steps grow
	- 🚨 **Classic LLD smell**
		- > "This method keeps growing line by line"
-
- ### Good Design (Chain of Responsibility)
	- ### Core idea ⭐
		- > "Each handler does ONE check and passes control to the next."
	- ### Step 1: Base Handler
	  ```tsx
	  abstract class Handler {
	    private next: Handler | null = null;

	    setNext(handler: Handler): Handler {
	      this.next = handler;
	      return handler;
	    }

	    handle(req: any): void {
	      if (this.next) {
	        this.next.handle(req);
	      }
	    }
	  }
	  ```
	- ### Step 2: Concrete Handlers
		- ### Authentication
		  ```tsx
		  class AuthHandler extends Handler {
		    handle(req: any): void {
		      if (!req.user) {
		        throw new Error("Unauthenticated");
		      }
		      super.handle(req);
		    }
		  }
		  ```
		- ### Authorization
		  ```tsx
		  class AuthorizationHandler extends Handler {
		    handle(req: any): void {
		      if (!req.user.isAdmin) {
		        throw new Error("Unauthorized");
		      }
		      super.handle(req);
		    }
		  }
		  ```
		- ### Payload Validation
		  ```tsx
		  class PayloadHandler extends Handler {
		    handle(req: any): void {
		      if (req.body.size > 1024) {
		        throw new Error("Payload too large");
		      }
		      super.handle(req);
		    }
		  }
		  ```
	- ### Step 3: Build the chain
	  ```tsx
	  const auth = new AuthHandler();
	  const authorization = new AuthorizationHandler();
	  const payload = new PayloadHandler();

	  auth.setNext(authorization).setNext(payload);

	  auth.handle(request);
	  ```
-
- ### What just improved? (intuition)
	- | Before ❌ | After ✅ |
	  | --- | --- |
	  | One giant method | Small focused classes |
	  | Hard to extend | Easy to add/remove steps |
	  | Tight coupling | Loose coupling |
	  | Rigid order | Configurable chain |
-
- ### Key Intuition ⭐ (MOST IMPORTANT)
	- > **Chain of Responsibility = pass the request, not the responsibility**
	- Each handler:
		- decides **only its concern**
		- either stops the chain or forwards it
-
- ### When to use it
	- Validation pipelines
	- Middleware (Express, Nest)
	- Approval flows
	- Filters / processing steps
-
- ### When NOT to use it
	- Only one step
	- Order never changes
	- Simple `if` is clearer
	- **Interview rule:**
		- Don't introduce a chain for 2 checks.
-
- ### Common Interview Traps ❌
	- Putting business logic inside handlers
	- Making handlers dependent on each other
	- Forgetting to forward the request
	- Using chain when simple composition works
-
- ### Interview One-liner (perfect)
	- > "Chain of Responsibility lets us process a request through a sequence of independent handlers without coupling them together."
