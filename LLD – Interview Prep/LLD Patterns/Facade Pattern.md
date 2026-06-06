### Problem it solves
	- When a system has **many complex subsystems**, clients shouldn't need to:
		- know the order of calls
		- know internal dependencies
		- deal with multiple objects directly
	- **Facade provides a simple, unified interface** over a complex subsystem.
-
- ### Bad Design (Client talks to everything)
	- ```tsx
	  class PaymentService {
	    pay() { console.log("Payment done"); }
	  }

	  class InventoryService {
	    reserve() { console.log("Inventory reserved"); }
	  }

	  class NotificationService {
	    send() { console.log("Notification sent"); }
	  }

	  // Client code
	  class OrderController {
	    placeOrder() {
	      const payment = new PaymentService();
	      payment.pay();

	      const inventory = new InventoryService();
	      inventory.reserve();

	      const notification = new NotificationService();
	      notification.send();
	    }
	  }
	  ```
-
- ### Why this is a problem
	- Client knows **too much**
	- Order of calls is **hard-coded**
	- Any change in workflow → change client
	- Hard to reuse logic
	- Violates **Law of Demeter**
	- 🚨 **LLD smell:**
		- > "Controller orchestrates everything itself"
-
- ### Good Design (Facade)
	- ### Core idea ⭐
		- > **Hide complexity behind a simple interface**
	- ### Facade Class
	  ```tsx
	  class OrderFacade {
	    private payment = new PaymentService();
	    private inventory = new InventoryService();
	    private notification = new NotificationService();

	    placeOrder() {
	      this.payment.pay();
	      this.inventory.reserve();
	      this.notification.send();
	    }
	  }
	  ```
	- ### Client Code (Clean)
	  ```tsx
	  class OrderController {
	    placeOrder() {
	      const orderFacade = new OrderFacade();
	      orderFacade.placeOrder();
	    }
	  }
	  ```
-
- ### What improved? (intuition)
	- | Before ❌ | After ✅ |
	  | --- | --- |
	  | Client knows all services | Client knows one facade |
	  | Hard-coded workflow | Workflow centralized |
	  | Hard to change | Easy to modify |
	  | No abstraction | Clean API |
-
- ### Key Intuition ⭐ (MOST IMPORTANT)
	- > **Facade = "One simple door to a complex system"**
	- Client talks to **one object**, not many.
-
- ### When to use it
	- Complex subsystems
	- Multi-step workflows
	- Controllers getting bloated
	- External APIs look messy
	- You want a clean public API
-
- ### When NOT to use it
	- System is already simple
	- No orchestration needed
	- Facade would just forward calls without value
	- > **Interview rule:**
		- If Facade doesn't reduce mental load, don't add it.
-
- ### Common Interview Traps ❌
	- Thinking Facade replaces subsystems (it doesn't)
	- Putting business logic inside Facade
	- Confusing Facade with Adapter
	- Creating Facade too early
