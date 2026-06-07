## Strategy Pattern
	- ### Problem it solves
	  
	  You have **multiple ways to do the same thing**, and using `if / else` or `switch` will keep growing and touching the same class again and again.
	  
	  ---
	- ### When to use it
	- Behavior changes based on **type / mode / condition**
	- You see or expect **more cases in future**
	- You want to follow **Open–Closed Principle**
	- You want to **unit test behaviors independently**
	  
	  **LLD smell:**
	  
	  > “This if-else block will grow”
	  > 
	  
	  ---
	- ### When NOT to use it
	- Only **1–2 cases** and unlikely to grow
	- Behavior is **truly static**
	- You’re adding it “just in case” (overengineering)
	  
	  **Interview rule:**
	  
	  Simple `if-else` is better than forced Strategy.
	  
	  ---
	- ### Small TypeScript example
	  
	  **Without Strategy (problematic):**
	  
	  ```tsx
	  class PaymentService {
	  pay(amount: number, method: string) {
	    if (method === "CARD") {
	      console.log("Paying by card");
	    } else if (method === "UPI") {
	      console.log("Paying by UPI");
	    }
	  }
	  }
	  
	  ```
	  
	  ---
	  
	  **With Strategy (clean & extensible):**
	  
	  ```tsx
	  interface PaymentStrategy {
	  pay(amount: number): void;
	  }
	  
	  class CardPayment implements PaymentStrategy {
	  pay(amount: number) {
	    console.log("Paying by card:", amount);
	  }
	  }
	  
	  class UpiPayment implements PaymentStrategy {
	  pay(amount: number) {
	    console.log("Paying by UPI:", amount);
	  }
	  }
	  
	  class PaymentService {
	  constructor(private strategy: PaymentStrategy) {}
	  
	  pay(amount: number) {
	    this.strategy.pay(amount);
	  }
	  }
	  
	  ```
	  
	  **Usage:**
	  
	  ```tsx
	  const payment = new PaymentService(new CardPayment());
	  payment.pay(500);
	  
	  ```
	  
	  ---
	- ### Interview-friendly explanation (1–2 lines)
	  
	  > “Strategy lets us move changing behavior into separate classes so we can add new behaviors without modifying existing code.”
	  > 
	  
	  ---
	- ### Key intuition to remember
	- **Context** → uses the strategy
	- **Strategy** → defines behavior
	- Swap behavior **at runtime**
	- You already use this in:
		- Validators
		- Pricing rules
		- Payment methods
		- Sorting logic
		  
		  ---
	- ### Quick check (self-test)
	  
	  If you see:
	  
	  ```tsx
	  if (type === A) ...
	  elseif (type === B) ...
	  
	  ```
	  
	  Ask yourself:
	  
	  > “Is this behavior or data?”
	  > 
	  
	  If **behavior** → Strategy might fit.
