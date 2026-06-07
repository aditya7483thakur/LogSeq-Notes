### Problem it solves
	- You want to **use an existing class**, but its **interface doesn't match** what your code expects.
	- In simple words:
		- > "This class does the job, but its method names / shape don't fit."
-
- ### ❌ BAD DESIGN (tight coupling to external code)
	- ### Existing / third-party class (you can't change this)
	  ```tsx
	  class StripePayment {
	    makePayment(amountInCents: number) {
	      console.log("Paid using Stripe:", amountInCents);
	    }
	  }
	  ```
	- ### Your code depends directly on it ❌
	  ```tsx
	  class PaymentService {
	    pay(amount: number) {
	      const stripe = new StripePayment();
	      stripe.makePayment(amount * 100);
	    }
	  }
	  ```
	- ### Why this is bad
		- Your code is **locked to Stripe**
		- If Stripe API changes → your code breaks
		- Switching to Razorpay = rewrite logic
	- 🚨 Strong coupling.
-
- ### ✅ GOOD DESIGN (Adapter)
	- ### Step 1: Define what YOUR system expects
	  ```tsx
	  interface PaymentGateway {
	    pay(amount: number): void;
	  }
	  ```
	- ### Step 2: Adapter wraps the external class
	  ```tsx
	  class StripeAdapter implements PaymentGateway {
	    constructor(private stripe: StripePayment) {}
	  
	    pay(amount: number): void {
	      this.stripe.makePayment(amount * 100);
	    }
	  }
	  ```
	- ### Step 3: Your core logic depends on the interface only
	  ```tsx
	  class PaymentService {
	    constructor(private gateway: PaymentGateway) {}
	  
	    pay(amount: number) {
	      this.gateway.pay(amount);
	    }
	  }
	  ```
	- ### Usage
	  ```tsx
	  const stripeGateway = new StripeAdapter(new StripePayment());
	  const paymentService = new PaymentService(stripeGateway);
	  
	  paymentService.pay(500);
	  ```
-
- ### What changed? (THIS is the intuition)
	- | Without Adapter ❌ | With Adapter ✅ |
	  | --- | --- |
	  | Core knows Stripe | Core knows interface |
	  | Hard to switch | Easy to swap |
	  | External API leaks inside | External API isolated |
	  | Tight coupling | Loose coupling |
-
- ### One-line intuition (memorize ⭐)
	- > **Adapter = "Make incompatible interfaces work together."**
	- Even simpler:
		- > **"Wrap what you can't change."**
-
- ### Interview-friendly explanation
	- > "Adapter lets us integrate third-party or legacy code without changing our core logic."
-
- ### When to use Adapter
	- Third-party SDKs
	- Legacy code
	- Different APIs doing same job
	- Migrating systems gradually
-
- ### When NOT to use it
	- You control both sides
	- Interface already matches
	- Adapter adds no transformation
	- Then just call it directly.
-
- ### Very important interview note ⚠️
	- ❌ Don't confuse Adapter with Factory
	- **Factory** → who creates the object
	- **Adapter** → how interfaces are matched
	- They solve **different problems**.