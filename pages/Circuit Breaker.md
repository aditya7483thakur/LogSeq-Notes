### The Problem 

Suppose you have an e-commerce application.

```
Frontend
    │
    ▼
Order Service
    │
    ▼
Payment Service
```

A customer places an order.

Order Service calls

```
POST /payment
```

Everything works.

---
- ## Now Payment Service goes down.
  
  Order Service doesn't know that.
  
  It still keeps sending requests.
  
  ```
  Order Service
  
  ↓
  
  Payment
  
  ↓
  
  Timeout
  
  ↓
  
  Payment
  
  ↓
  
  Timeout
  
  ↓
  
  Payment
  
  ↓
  
  Timeout
  ```
  
  Now imagine **10,000 users** are placing orders.
  
  Every request waits **30 seconds** before timing out.
  
  ```
  10,000 Requests
  
  ↓
  
  Waiting...
  
  ↓
  
  Waiting...
  
  ↓
  
  Waiting...
  ```
  
  Your Order Service starts running out of:
- Threads
- Database connections
- Memory
  
  Now even healthy APIs stop responding.
  
  One failing service has taken down another.
  
  This is called **cascading failure**.
  
  ---
- # The Solution: Circuit Breaker
  
  Think about the electrical circuit breaker in your house.
  
  Normally:
  
  ```
  Electricity
  
  ↓
  
  Switch
  
  ↓
  
  Appliances
  ```
  
  If too much current flows:
  
  ```
  Circuit Breaker Trips
  
  ↓
  
  Power Stops
  
  ↓
  
  Protects Appliances
  ```
  
  Software works the same way.
  
  Instead of continuously calling a failing service, we temporarily stop calling it.
  
  ---
- # Three States
  
  A Circuit Breaker has only **three states**.
  
  ---
- ## 1. Closed (Normal)
  
  Everything is healthy.
  
  ```
  Order Service
  
  ↓
  
  Payment Service
  
  ↓
  
  Success
  ```
  
  Every request is allowed.
  
  ---
- ## 2. Open (Failure)
  
  Suppose the last 10 requests failed.
  
  ```
  Payment
  
  ×
  
  ×
  
  ×
  
  ×
  
  ×
  
  ×
  
  ×
  
  ×
  
  ×
  
  ×
  ```
  
  Circuit Breaker decides
  
  > 
  
  "Enough."
  
  It opens the circuit.
  
  Now instead of calling Payment Service
  
  ```
  Order Service
  
  ↓
  
  Circuit Breaker
  
  ↓
  
  Immediate Failure
  ```
  
  Notice
  
  It doesn't even try to contact Payment Service.
  
  Instead of waiting 30 seconds,
  
  response comes back in milliseconds.
  
  ---
- ## Why is this useful?
  
  Without Circuit Breaker
  
  ```
  Request
  
  ↓
  
  30 sec timeout
  
  ↓
  
  Request
  
  ↓
  
  30 sec timeout
  ```
  
  With Circuit Breaker
  
  ```
  Request
  
  ↓
  
  Circuit Open
  
  ↓
  
  Return Error Immediately
  ```
  
  Huge difference.
  
  ---
- ## 3. Half Open
  
  Eventually we should check whether Payment Service recovered.
  
  Suppose after 30 seconds
  
  Circuit Breaker says
  
  > 
  
  "I'll allow ONE request."
  
  ```
  Order Service
  
  ↓
  
  Payment Service
  
  ↓
  
  ?
  ```
  
  Two possibilities.
- ### Success
  
  ```
  Payment works
  
  ↓
  
  Close Circuit
  
  ↓
  
  Normal traffic resumes
  ```
  
  ---
- ### Failure
  
  ```
  Still failing
  
  ↓
  
  Open Circuit again
  
  ↓
  
  Wait another 30 sec
  ```
  
  ---
- # Visual Flow
  
  ```
  Success
  Closed ───────────────► Closed
  
    │
    │ Too many failures
    ▼
  
  Open
  
    │
    │ Wait 30 sec
    ▼
  
  Half Open
  
   │        │
  Success    Failure
   │        │
   ▼        ▼
  
  Closed     Open
  ```
  
  ---
- # When should we use it?
  
  Whenever your application depends on another service.
  
  Examples:
- ### Payment Gateway
  
  ```
  Order Service
  
  ↓
  
  Stripe
  ```
  
  ---
- ### Email Service
  
  ```
  User Service
  
  ↓
  
  Email Service
  ```
  
  ---
- ### Recommendation Engine
  
  ```
  Product Service
  
  ↓
  
  ML Recommendation API
  ```
  
  ---
- ### External Weather API
  
  ```
  Travel App
  
  ↓
  
  Weather API
  ```
  
  ---
- ### Internal Microservices
  
  ```
  Inventory
  
  ↓
  
  Pricing
  
  ↓
  
  Shipping
  
  ↓
  
  User
  ```
  
  Basically,
  
  **whenever one service calls another service over the network**, a circuit breaker is a good candidate.
  
  ---
- # How is it implemented?
  
  Imagine this simple logic:
  
  ```javascript
  if (circuitState === "OPEN") {
    return "Payment Service Unavailable";
  }
  
  try {
    callPaymentService();
  
    failures = 0;
  
  } catch {
  
    failures++;
  
    if (failures >= 5) {
        circuitState = "OPEN";
    }
  }
  ```
  
  That's the core idea.
  
  ---
- ## A Better JavaScript Example
  
  ```javascript
  let failures = 0;
  let circuitOpen = false;
  
  async function pay() {
  
    if (circuitOpen) {
        throw new Error("Circuit is OPEN");
    }
  
    try {
  
        await paymentAPI();
  
        failures = 0;
  
    } catch (err) {
  
        failures++;
  
        if (failures >= 5) {
            circuitOpen = true;
  
            setTimeout(() => {
                circuitOpen = false;
            }, 30000);
        }
  
        throw err;
    }
  }
  ```
  
  This is simplified, but it demonstrates the basic flow.
  
  ---
- # Real Implementations
  
  In production, you usually don't write all of this yourself.
  
  Libraries handle it for you.
- ### Java (Spring Boot)
  
  Most common:
- **Resilience4j**
- (Older) Netflix Hystrix
  
  Example:
  
  ```javascript
  @CircuitBreaker(name = "paymentService")
  public PaymentResponse pay() {
    return paymentClient.pay();
  }
  ```
  
  ---
- ### Node.js
  
  Popular libraries include:
- `opossum`
  
  Example:
  
  ```javascript
  const CircuitBreaker = require("opossum");
  
  const breaker = new CircuitBreaker(paymentAPI, {
    timeout: 5000,
    errorThresholdPercentage: 50,
    resetTimeout: 30000
  });
  
  breaker.fire();
  ```
  
  The library tracks failures, changes states (Closed → Open → Half Open), and retries automatically according to its configuration.
  
  ---
- # Advantages
- Prevents cascading failures
- Reduces response time during outages
- Protects downstream services
- Improves system stability
- Allows services time to recover
  
  ---
- # Limitations
- Doesn't fix the failing service
- Thresholds and timeouts need tuning
- Can reject requests while a service has already recovered (until the next half-open probe)
- Adds some implementation complexity
  
  ---
- # Interview Question
  
  **Why not simply keep retrying?**
  
  Imagine:
- Payment Service is down.
- 50,000 requests arrive.
  
  If every request retries 3 times:
  
  ```
  50,000 requests
  
  ↓
  
  150,000 retries
  
  ↓
  
  Payment Service gets overwhelmed
  
  ↓
  
  Recovery becomes even slower
  ```
  
  A Circuit Breaker prevents this retry storm by failing fast once it detects sustained failures.
  
  ---
- ## One important thing to remember
  
  People often confuse **Retry** and **Circuit Breaker**.
  
  They actually complement each other:
  
  ```
  Request
    │
    ▼
  Retry (2–3 quick attempts)
    │
    ▼
  Still failing?
    │
    ▼
  Circuit Breaker opens
    │
    ▼
  Fail fast until the dependency recovers
  ```
  
  **Retry** handles **temporary, short-lived failures** (a transient network hiccup).
  
  **Circuit Breaker** handles **persistent failures** (a service is actually unhealthy).
  
  That's why you'll often see both patterns used together in production systems.