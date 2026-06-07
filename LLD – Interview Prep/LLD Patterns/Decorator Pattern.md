- ## Decorator Pattern
  collapsed:: true
	- ### Problem it solves
	  
	  You want to **add optional behavior/features** to an object **without**:
	- creating too many subclasses
	- modifying the original class
	  
	  ---
	- ### When to use it
	- Features are **optional**
	- Features can be **combined** in different ways
	- Inheritance would explode into many classes
	  
	  **LLD smell:**
	  
	  > “If I keep using inheritance, I’ll get too many subclasses”
	  > 
	  
	  ---
	- ### When NOT to use it
	- Behavior is **fixed**
	- Only **one variation**
	- You don’t need runtime flexibility
	  
	  **Interview rule:**
	  
	  If inheritance is simple → don’t force Decorator.
	  
	  ---
	- ### Small TypeScript example
	- ### Base interface
	  
	  ```tsx
	  interface Coffee {
	  cost(): number;
	  description(): string;
	  }
	  ```
	  
	  ---
	- ### Concrete component
	  
	  ```tsx
	  class SimpleCoffee implements Coffee {
	  cost(): number {
	    return 100;
	  }
	  
	  description(): string {
	    return "Simple Coffee";
	  }
	  }
	  ```
	  
	  ---
	- ### Decorator (wraps another Coffee)
	  
	  ```tsx
	  class MilkDecorator implements Coffee {
	  constructor(private coffee: Coffee) {}
	  
	  cost(): number {
	    return this.coffee.cost() + 20;
	  }
	  
	  description(): string {
	    return this.coffee.description() + ", Milk";
	  }
	  }
	  ```
	  
	  ---
	- ### Another decorator
	  
	  ```tsx
	  class SugarDecorator implements Coffee {
	  constructor(private coffee: Coffee) {}
	  
	  cost(): number {
	    return this.coffee.cost() + 10;
	  }
	  
	  description(): string {
	    return this.coffee.description() + ", Sugar";
	  }
	  }
	  ```
	  
	  ---
	- ### Usage (runtime composition 🔥)
	  
	  ```tsx
	  let coffee: Coffee = new SimpleCoffee();
	  coffee = new MilkDecorator(coffee);
	  coffee = new SugarDecorator(coffee);
	  
	  console.log(coffee.description()); // Simple Coffee, Milk, Sugar
	  console.log(coffee.cost());        // 130
	  ```
	  
	  ---
	- ## Key intuition (VERY IMPORTANT ⭐)
	  
	  > **Decorator = wrapping, not inheriting**
	  >
	- Same interface
	- Adds behavior
	- Delegates to wrapped object
	  
	  ---
	- ## Why inheritance fails here ❌
	  
	  If you used inheritance:
	- `MilkCoffee`
	- `SugarCoffee`
	- `MilkSugarCoffee`
	- `SugarMilkCoffee`
	  
	  🚨 Class explosion.
	  
	  Decorator avoids this.
	  
	  ---
	- ## Interview-friendly explanation
	  
	  > “Decorator lets us add features dynamically by wrapping objects instead of creating many subclasses.”
	  > 
	  
	  ---
	- ## Mental check
	  
	  Ask:
	  
	  > “Are these features optional and combinable?”
	  > 
	  
	  If yes → Decorator fits
	  
	  If no → inheritance or simple logic is better
	  
	  ---
	- ## Common interview mistake ❌
	  
	  ❌ Using Decorator when a simple flag works
	  
	  ❌ Using Decorator for core business logic
	  
	  Decorator is for **feature layering**, not decisions.
