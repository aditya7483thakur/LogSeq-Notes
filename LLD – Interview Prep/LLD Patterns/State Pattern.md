- ## State Pattern
  collapsed:: true
	- ## Overview
	  
	  This one shows up naturally in **workflows, lifecycles, and status-driven behavior** (order status, ticket status, payment status).
	  
	  ---
	- ## Problem it solves
	  
	  When an object’s **behavior changes based on its current state**, and using `if / else` or `switch` keeps growing and becoming messy.
	  
	  ---
	- ## Bad Design (State logic inside one class)
	  
	  ```tsx
	  class Order {
	  status: "CREATED" | "PAID" | "SHIPPED";
	  
	  constructor() {
	   this.status = "CREATED";
	  }
	  
	  next() {
	   if (this.status === "CREATED") {
	     console.log("Processing payment");
	     this.status = "PAID";
	   } else if (this.status === "PAID") {
	     console.log("Shipping order");
	     this.status = "SHIPPED";
	   } else {
	     throw new Error("Order already shipped");
	   }
	  }
	  }
	  ```
	  
	  ---
	- ## Why this is a problem
	- Too many `if / else`
	- Adding a new state → modify this class
	- Violates **Open–Closed Principle**
	- Hard to test individual state behavior
	  
	  🚨 **LLD smell:**
	  
	  > “Behavior changes based on state, but logic is centralized”
	  > 
	  
	  ---
	- ## Core Idea (State Pattern)
	  
	  > **Move state-specific behavior into separate classes**
	  > 
	  
	  Each state:
	- knows what it can do
	- decides the next state
	- encapsulates its own behavior
	  
	  ---
	- ## Good Design (State Pattern)
	  
	  ---
	- ## 1️⃣ State Interface
	  
	  ```tsx
	  interface OrderState {
	  next(order: Order): void;
	  }
	  ```
	  
	  ---
	- ## 2️⃣ Concrete States
	  
	  ```tsx
	  class CreatedState implements OrderState {
	  next(order: Order): void {
	   console.log("Payment processed");
	   order.setState(new PaidState());
	  }
	  }
	  ```
	  
	  ```tsx
	  class PaidState implements OrderState {
	  next(order: Order): void {
	   console.log("Order shipped");
	   order.setState(new ShippedState());
	  }
	  }
	  ```
	  
	  ```tsx
	  class ShippedState implements OrderState {
	  next(_: Order): void {
	   throw new Error("Order already shipped");
	  }
	  }
	  ```
	  
	  ---
	- ## 3️⃣ Context (Holds current state)
	  
	  ```tsx
	  class Order {
	  private state: OrderState;
	  
	  constructor() {
	   this.state = new CreatedState();
	  }
	  
	  setState(state: OrderState) {
	   this.state = state;
	  }
	  
	  next() {
	   this.state.next(this);
	  }
	  }
	  ```
	  
	  ---
	- ## Usage
	  
	  ```tsx
	  const order = new Order();
	  
	  order.next(); // Payment processed
	  order.next(); // Order shipped
	  order.next(); // Error
	  ```
	  
	  ---
	- ## What improved? (intuition)
	  
	  | Before ❌ | After ✅ |
	  | --- | --- |
	  | Large conditional logic | Small focused classes |
	  | Hard to extend | Easy to add states |
	  | One class knows all states | Each state knows itself |
	  | Hard to test | Test states independently |
	  
	  ---
	- ## Key Intuition ⭐ (MOST IMPORTANT)
	  
	  > **State = “Behavior changes because the object changed its state”**
	  > 
	  
	  Not flags.
	  
	  Not conditionals.
	  
	  **Objects representing states.**
	  
	  ---
	- ## When to use it
	- Order lifecycle
	- Ticket status
	- Payment flow
	- Approval workflows
	- Finite state machines
	  
	  ---
	- ## When NOT to use it
	- Only 2 states
	- Simple conditional is enough
	- State logic won't grow
	  
	  > **Interview rule:**
	  > 
	  > 
	  > Don’t use State for 2 `if` conditions.
	  > 
	  
	  ---
	- ## Common Interview Traps ❌
	- Confusing State with Strategy
	- Letting context decide transitions
	- Using enums instead of state objects
	- Overengineering small flows