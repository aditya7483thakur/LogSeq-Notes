- ## Observer Pattern
  collapsed:: true
	- ### Problem it solves
	  
	  When **multiple components need to react to a change**, you don’t want the main class to directly call all of them.
	  
	  Avoids:
	- tight coupling
	- hard-coded notification logic
	- ripple changes when a new listener is added
	  
	  ---
	- ### When to use it
	- One object changes state
	- **Many others need to be notified**
	- Listeners can change over time
	- Event-driven behavior feels natural
	  
	  **LLD smell:**
	  
	  > “When X happens, I have to call A, B, and C”
	  > 
	  
	  ---
	- ### When NOT to use it
	- Only **one dependent**
	- Simple direct call is enough
	- You don’t actually need async / event-style updates
	  
	  **Interview rule:**
	  
	  Don’t use Observer just to look fancy.
	  
	  ---
	- ### Small TypeScript example
	- ### Step 1: Observer interface
	  
	  ```tsx
	  interface Observer {
	  update(data: any): void;
	  }
	  ```
	- ### Step 2: Subject (publisher)
	  
	  ```tsx
	  class Order {
	  private observers: Observer[] = [];
	  
	  attach(observer: Observer) {
	    this.observers.push(observer);
	  }
	  
	  notify(orderId: string) {
	    this.observers.forEach(o => o.update(orderId));
	  }
	  
	  placeOrder(orderId: string) {
	    console.log("Order placed:", orderId);
	    this.notify(orderId);
	  }
	  }
	  ```
	- ### Step 3: Concrete observers
	  
	  ```tsx
	  class EmailService implements Observer {
	  update(orderId: string) {
	    console.log("Email sent for order:", orderId);
	  }
	  }
	  
	  class AnalyticsService implements Observer {
	  update(orderId: string) {
	    console.log("Analytics updated for order:", orderId);
	  }
	  }
	  ```
	- ### Usage
	  
	  ```tsx
	  const order = new Order();
	  
	  order.attach(new EmailService());
	  order.attach(new AnalyticsService());
	  
	  order.placeOrder("ORD-1");
	  ```
	  
	  ---
	- ### Interview-friendly explanation
	  
	  > “Observer lets multiple components react to a change without the source knowing about them directly.”
	  > 
	  
	  ---
	- ### Key intuition to remember
	- **Subject** → state owner
	- **Observers** → react to changes
	- Subject **doesn’t care who listens**
	- Add/remove listeners without modifying core logic
	  
	  ---
	- ### Common interview mistake ❌
	  
	  ❌ Calling observers directly:
	  
	  ```tsx
	  email.send();
	  analytics.track();
	  ```
	  
	  ✅ Let observers subscribe themselves.
