### Problem it solves
	- You don't want **object creation logic (`new`) scattered everywhere**, especially when creation depends on some condition or type.
-
- ### When to use it
	- Object creation depends on **input / type / config**
	- You see `new X()`, `new Y()`, `new Z()` spread across code
	- You want to **centralize creation logic**
	- You're already using **Strategy** and need a clean way to choose one
	- **LLD smell:**
		- > "Which class should I create here?"
-
- ### When NOT to use it
	- Only **one concrete class**
	- Object creation is **simple and stable**
	- Factory would just wrap `new` without logic (useless abstraction)
	- **Interview rule:**
		- If creation logic is trivial → skip Factory.
-
- ### Small TypeScript example
	- **Without Factory (messy):**
	  ```tsx
	  const method = "CARD";

	  let strategy;
	  if (method === "CARD") {
	    strategy = new CardPayment();
	  } else if (method === "UPI") {
	    strategy = new UpiPayment();
	  }
	  ```
	- **With Factory (clean):**
	  ```tsx
	  class PaymentStrategyFactory {
	    static getStrategy(method: string): PaymentStrategy {
	      switch (method) {
	        case "CARD":
	          return new CardPayment();
	        case "UPI":
	          return new UpiPayment();
	        default:
	          throw new Error("Invalid payment method");
	      }
	    }
	  }
	  ```
	- **Usage:**
	  ```tsx
	  const strategy = PaymentStrategyFactory.getStrategy("CARD");
	  const paymentService = new PaymentService(strategy);
	  paymentService.pay(500);
	  ```
-
- ### Interview-friendly explanation
	- > "Factory centralizes object creation so the rest of the system doesn't depend on concrete classes."
-
- ### Key intuition to remember
	- **Strategy = behavior**
	- **Factory = creation**
	- Factory often **returns a Strategy**
	- Keeps `new` in **one place**
-
- ### Common interview mistake ❌
	- ❌ Saying: "Factory is mandatory with Strategy"
	- ✅ Correct: "Factory is helpful when strategy selection logic grows"
-
- ### Mental model
	- Strategy answers: **"How should it behave?"**
	- Factory answers: **"Which object should I create?"**
-
- ### Quick self-check
	- If you're writing:
	  ```tsx
	  new Something()
	  ```
	- Ask:
		- > "Will I have multiple variants of this later?"
	- If yes → Factory might help.
-
- ### Strategy + Factory (together)
	- ### Problem being solved
		- We have **multiple behaviors** (Strategy)
		- Which behavior to use depends on **input / config** (Factory)
		- We don't want `if-else` spread across business logic
-
- ### Strategy: define interchangeable behavior
	- ```tsx
	  interface PaymentStrategy {
	    pay(amount: number): void;
	  }

	  class CardPayment implements PaymentStrategy {
	    pay(amount: number): void {
	      console.log("Paid via Card:", amount);
	    }
	  }

	  class UpiPayment implements PaymentStrategy {
	    pay(amount: number): void {
	      console.log("Paid via UPI:", amount);
	    }
	  }
	  ```
-
- ### Factory: decide which strategy to create
	- ```tsx
	  class PaymentStrategyFactory {
	    static getStrategy(method: string): PaymentStrategy {
	      switch (method) {
	        case "CARD":
	          return new CardPayment();
	        case "UPI":
	          return new UpiPayment();
	        default:
	          throw new Error("Unsupported payment method");
	      }
	    }
	  }
	  ```
-
- ### Context: use the strategy (no `if-else` here)
	- ```tsx
	  class PaymentService {
	    constructor(private paymentStrategy: PaymentStrategy) {}

	    pay(amount: number) {
	      this.paymentStrategy.pay(amount);
	    }
	  }
	  ```
-
- ### Usage (runtime decision)
	- ```tsx
	  const methodFromAPI = "CARD"; // dynamic input

	  const strategy = PaymentStrategyFactory.getStrategy(methodFromAPI);
	  const paymentService = new PaymentService(strategy);

	  paymentService.pay(500);
	  ```
-
- ### Mental model (VERY important)
	- **Strategy** → *How should it behave?*
	- **Factory** → *Which one should I create?*
	- **Context** → *Uses behavior, doesn't decide*
	- 👉 Each class has **one reason to change**
	- (SRP ✔️)
