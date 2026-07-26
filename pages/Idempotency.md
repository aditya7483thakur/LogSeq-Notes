- **Idempotency is the property of an operation where performing the same operation multiple times produces the same final state as performing it once.**
  
  In distributed systems, idempotency ensures that **retries do not create duplicate side effects.**
  
  ---
- # Why do we need it?
  
  Distributed systems are unreliable.
  
  Things can fail due to:
- Network timeout
- Server crash
- Client retry
- Load balancer retry
- Message broker redelivery
  
  If the same request is executed multiple times, it may lead to:
- Duplicate payments
- Duplicate orders
- Duplicate emails
- Duplicate ticket bookings
  
  Idempotency prevents these duplicate side effects.
  
  ---
- # Problem Statement
  
  Imagine an e-commerce application.
  
  User buys a laptop costing **₹1000**.
  
  ```
  Client
   |
  POST /payments
   |
  Backend
   |
  Payment Gateway
  ```
  
  The payment succeeds.
  
  Before the backend sends the response, the network connection drops.
  
  The client never receives the success response.
  
  ```
  Payment Success
  
  ↓
  
  Network Failure
  
  ↓
  
  Client thinks payment failed
  
  ↓
  
  Retries
  ```
  
  Without idempotency, the backend processes the payment again.
  
  Result:
  
  ```
  ₹1000 charged
  
  ↓
  
  Retry
  
  ↓
  
  ₹1000 charged again ❌
  ```
  
  ---
- # Solution
  
  Every **business operation** receives a unique **Idempotency Key**.
  
  Example
  
  ```
  Idempotency-Key:
  550e8400-e29b-41d4-a716-446655440000
  ```
  
  The key identifies the **operation**, not the HTTP request.
  
  The same key is reused **only when retrying the same operation**.
  
  ---
- # Request Flow
- ## First Request
  
  ```
  Client
  
  ↓
  
  POST /payments
  
  Idempotency-Key = ABC123
  
  ↓
  
  Backend
  
  ↓
  
  Key exists?
  
  ↓
  
  NO
  
  ↓
  
  Process payment
  
  ↓
  
  Store key
  
  ↓
  
  Return success
  ```
  
  Database
  
  | Key | Payment ID | Status |
  | ---- | ---- | ---- |
  | ABC123 | PAY001 | SUCCESS |
  
  ---
- ## Retry
  
  Client sends
  
  ```
  POST /payments
  
  Idempotency-Key = ABC123
  ```
  
  Backend
  
  ```
  Key exists?
  
  ↓
  
  YES
  
  ↓
  
  Return previous response
  
  ↓
  
  Do NOT process payment again
  ```
  
  No duplicate payment occurs.
  
  ---
- # Backend Algorithm
  
  ```
  Receive Request
  
  ↓
  
  Check Idempotency Key
  
  ↓
  
  Exists?
  
  YES ─────────► Return Stored Response
  
  NO
  
  ↓
  
  Execute Business Logic
  
  ↓
  
  Store Result
  
  ↓
  
  Return Response
  ```
  
  ---
- # JavaScript Example (Express + In-Memory Store)
  
  **Note:** This uses a `Map` only for learning. In production you'd typically use Redis or a database.
- ```javascript
  const express = require("express");
  
  const app = express();
  app.use(express.json());
  
  const idempotencyStore = new Map();
  
  app.post("/payment", (req, res) => {
  
    const key = req.headers["idempotency-key"];
  
    if (!key) {
        return res.status(400).json({
            message: "Missing Idempotency Key"
        });
    }
  
    // Already processed
    if (idempotencyStore.has(key)) {
  
        console.log("Returning cached response");
  
        return res.json(idempotencyStore.get(key));
    }
  
    // Simulate payment processing
    console.log("Charging customer...");
  
    const payment = {
        paymentId: Math.floor(Math.random() * 10000),
        status: "SUCCESS"
    };
  
    // Save response
    idempotencyStore.set(key, payment);
  
    res.json(payment);
  
  });
  
  app.listen(3000);
  ```
  
  ---
- ## First Request
  
  ```
  POST /payment
  
  Headers
  
  Idempotency-Key: abc123
  ```
  
  Console
  
  ```
  Charging customer...
  ```
  
  Response
  
  ```
  {
  "paymentId": 5872,
  "status": "SUCCESS"
  }
  ```
  
  ---
- ## Retry
  
  Same request
  
  ```
  POST /payment
  
  Headers
  
  Idempotency-Key: abc123
  ```
  
  Console
  
  ```
  Returning cached response
  ```
  
  Response
  
  ```
  {
  "paymentId": 5872,
  "status": "SUCCESS"
  }
  ```
  
  Notice
  
  No new payment is created.
  
  ---
- # Where is the key stored?
  
  Development
- Memory (`Map`)
  
  Production
- Redis
- PostgreSQL
- MySQL
- DynamoDB
  
  Redis is commonly used because:
- Extremely fast
- Supports expiration (TTL)
- Suitable for temporary idempotency keys
  
  ---
- # Where does the key come from?
  
  Usually the client generates it.
  
  Example
  
  ```javascript
  import { v4 as uuid } from "uuid";
  
  const key = uuid();
  
  fetch("/payment", {
    method: "POST",
    headers: {
        "Idempotency-Key": key
    }
  });
  ```
  
  **Important:**
  
  The frontend **must reuse the same key when retrying the same payment**.
  
  A **new payment** gets a **new key**.
  
  ---
- # Real-World Examples
- ### Payments
  
  ```
  POST /payments
  ```
  
  Prevent duplicate charges.
  
  ---
- ### Order Creation
  
  ```
  POST /orders
  ```
  
  Prevent duplicate orders.
  
  ---
- ### Ticket Booking
  
  ```
  POST /tickets
  ```
  
  Prevent booking the same seat twice.
  
  ---
- ### Kafka Consumers
  
  A message may be delivered more than once.
  
  The consumer stores the processed message ID.
  
  If the same message arrives again, it is ignored.
  
  ---
- # Advantages
- Prevents duplicate operations
- Makes retries safe
- Improves fault tolerance
- Essential for payment systems
- Works well with message queues
  
  ---
- # Limitations
- Additional storage required
- Keys should expire after some time (TTL)
- Does not replace database transactions
- Client must reuse the same key for retries
  
  ---
- # Interview Questions
- ### Why can't we simply retry failed requests?
  
  Because the original request may have already succeeded, leading to duplicate side effects.
  
  ---
- ### Does every API need idempotency?
  
  No.
  
  Typically only operations that create side effects, such as payments, orders, refunds, and bookings.
  
  ---
- ### Is GET idempotent?
  
  Yes.
  
  Calling GET multiple times does not change server state.
  
  ---
- ### Is POST idempotent?
  
  Not by default.
  
  It can be made idempotent using techniques such as idempotency keys.
  
  ---
- ### Is PUT idempotent?
  
  Yes.
  
  Replacing a resource with the same representation multiple times results in the same final state.
  
  ---
- ### Is PATCH idempotent?
  
  It depends.
  
  Setting a value (e.g. `status = "PAID"`) is idempotent.
  
  Incrementing a value (e.g. `balance += 100`) is not.