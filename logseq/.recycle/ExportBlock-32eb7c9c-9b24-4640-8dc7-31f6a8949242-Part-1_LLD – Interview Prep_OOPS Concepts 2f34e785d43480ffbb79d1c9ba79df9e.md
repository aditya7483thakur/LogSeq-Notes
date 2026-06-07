# OOPS Concepts

## Encapsulation

### Meaning (say this)

> “Encapsulation ensures that an object’s internal state can only be modified through well-defined methods.”
> 

### ❌ Bad Design (No Encapsulation)

```tsx
class BankAccount {
balance:number =0;
}

const acc =newBankAccount();
acc.balance = -500;// ❌ invalid state

```

**Problem**

- Anyone can modify data
- No validation
- Object state is unsafe

---

### ✅ Good Design (Encapsulation)

```tsx
class BankAccount {
privatebalance:number =0;

deposit(amount:number):void {
if (amount <=0)return;
this.balance += amount;
  }

withdraw(amount:number):boolean {
if (amount >this.balance)returnfalse;
this.balance -= amount;
returntrue;
  }

getBalance():number {
returnthis.balance;
  }
}

```

**Key Takeaways**

- `balance` is private
- All rules live inside the class
- Object controls its own state

## Abstraction

### ❌ Bad Design (No Abstraction)

```tsx
class PaymentService {
processPayment(method:"CARD" |"UPI",amount:number):void {
if (method ==="CARD") {
console.log("Paying by Card:", amount);
    }elseif (method ==="UPI") {
console.log("Paying by UPI:", amount);
    }
  }
}

```

**Why BAD**

- Tight coupling
- Violates Open/Closed Principle
- Class grows endlessly

---

### ✅ Good Design (Abstraction)

### Step 1️⃣ Interface

```tsx
interface PaymentMethod {
pay(amount:number):void;
}

```

### Step 2️⃣ Implementations

```tsx
class CardPayment implements PaymentMethod {
pay(amount:number):void {
console.log("Paying by Card:", amount);
  }
}

class UpiPayment implements PaymentMethod {
pay(amount:number):void {
console.log("Paying by UPI:", amount);
  }
}

```

### Step 3️⃣ High-level Service

```tsx
class PaymentService {
constructor(privatepaymentMethod:PaymentMethod) {}

processPayment(amount:number):void {
this.paymentMethod.pay(amount);
  }
}

```

### Step 4️⃣ Usage (🔥 AHA PART)

```tsx
const cardService =new PaymentService(new CardPayment());
cardService.processPayment(100);

const upiService =new PaymentService(new UpiPayment());
upiService.processPayment(200);

```

**Interview Line**

> “Passing implementations from outside avoids tight coupling and allows easy extension.”
> 

## Encapsulation vs Abstraction

| **Encapsulation** | **Abstraction** |
| --- | --- |
| Hides **data** | Hides **implementation** |
| Protects object state | Simplifies interaction |
| Uses `private` fields | Uses `interface` / contracts |
| Prevents misuse | Enables extensibility |
| Focus: **inside object** | Focus: **between objects** |

## Inheritance vs Composition

### ❌ Wrong Inheritance

```tsx
class Engine {
  start(): void {
    console.log("Engine started");
  }
}

class Car extends Engine { // ❌ Car is NOT an Engine
  drive(): void {
    this.start();
    console.log("Car is driving");
  }
}

```

**Why wrong**

- Wrong IS-A relationship
- Tight coupling
- No flexibility

---

### ✅ Composition (Preferred)

```tsx
interface Engine {
  start(): void;
}

class PetrolEngine implements Engine {
  start(): void {
    console.log("Petrol engine started");
  }
}

class ElectricEngine implements Engine {
  start(): void {
    console.log("Electric engine started");
  }
}

class Car {
  constructor(private engine: Engine) {}

  drive(): void {
    this.engine.start();
    console.log("Car is driving");
  }
}

```

**Usage**

```tsx
const petrolCar = new Car(new PetrolEngine());
petrolCar.drive();

const electricCar = new Car(new ElectricEngine());
electricCar.drive();

```

---

## 4️⃣ Diamond Problem (WITH CODE)

### C++ (Problem EXISTS)

```cpp
class A {
public:
voidshow() { cout <<"A"; }
};

class B :public A {};
class C :public A {};
class D :public B,public C {};// ❌ ambiguity

```

```cpp
D obj;
obj.show();// ❌ which A?

```

---

### Java (Classes → NO problem)

```java
class D extends B, C { }// ❌ NOT allowed

```

👉 Java avoids diamond problem by design.

---

### Java Interfaces (Controlled Case)

```java
interface A {
defaultvoidshow() {
    System.out.println("A");
  }
}

interface B {
default void show() {
    System.out.println("B");
  }
}

class C implements A, B {
@Override
publicvoidshow() {
    A.super.show();// or B.super.show()
  }
}

```

## Polymorphism

### Runtime Polymorphism

```tsx
interface PaymentMethod {
  pay(amount: number): void;
}

class CardPayment implements PaymentMethod {
  pay(amount: number): void {
    console.log("Card payment:", amount);
  }
}

class UpiPayment implements PaymentMethod {
  pay(amount: number): void {
    console.log("UPI payment:", amount);
  }
}

class PaymentService {
  constructor(private paymentMethod: PaymentMethod) {}

  process(amount: number): void {
    this.paymentMethod.pay(amount); // polymorphism
  }
}

```

**Where polymorphism lives**

```tsx
paymentMethod.pay(amount);

```

---

## Method Overloading (Compile-time)

```tsx
class Calculator {
  add(a: number, b: number): number;
  add(a: number, b: number, c: number): number;

  add(a: number, b: number, c?: number): number {
    return c !== undefined ? a + b + c : a + b;
  }
}

```

## Association vs Aggregation vs Composition

### 1️⃣ **Association**

### 📌 Meaning

- One object **uses** another object
- **No ownership**
- **No lifecycle dependency**

### 🔑 Key Characteristics

- Relationship type: **USES**
- Objects are **independent**
- Usually seen via **method parameters**
- No storing of the object as a field

### 🧠 Interview Explanation (say this)

> “Association means one class uses another class temporarily, without owning it.”
> 

### ✅ Real-world example

- Project uses Employee
- Teacher teaches Student
- Controller uses Service

### 🧾 Lifecycle Rule

- If one object is destroyed, the other **still exists**

---

### 2️⃣ **Aggregation**

### 📌 Meaning

- One object **has** another object
- **Weak ownership**
- Lifecycle is **NOT dependent**

### 🔑 Key Characteristics

- Relationship type: **HAS-A**
- Child object is **created outside**
- Parent only **references** it
- Child can exist without parent

### 🧠 Interview Explanation (say this)

> “Aggregation is a HAS-A relationship with weak ownership, where the child object can exist independently.”
> 

### ✅ Real-world example

- Company has Employees
- Team has Players
- Library has Books

### 🧾 Lifecycle Rule

- If parent is destroyed → child **still survives**

---

### 3️⃣ **Composition**

### 📌 Meaning

- One object **has and owns** another object
- **Strong ownership**
- Lifecycle is **tightly coupled**

### 🔑 Key Characteristics

- Relationship type: **HAS-A**
- Child object is **created inside** parent
- Child **cannot exist** without parent
- Strong encapsulation

### 🧠 Interview Explanation (say this)

> “Composition is a HAS-A relationship with strong ownership, where the child’s lifecycle depends on the parent.”
> 

### ✅ Real-world example

- Car has Engine
- House has Rooms
- Order has OrderItems

### 🧾 Lifecycle Rule

- If parent is destroyed → child **must be destroyed**

---

## 🔥 One-Look Comparison Table (MEMORIZE)

| Aspect | Association | Aggregation | Composition |
| --- | --- | --- | --- |
| Relationship | USES | HAS-A | HAS-A |
| Ownership | ❌ None | ⚠️ Weak | ✅ Strong |
| Lifecycle dependency | ❌ No | ❌ No | ✅ Yes |
| Object creation | External | External | Internal |
| LLD usage | Rare | Common | Very common |

### Association

```tsx
class Employee {
  constructor(public name: string) {}
}

class Project {
  start(employee: Employee): void {
    console.log(`${employee.name} working on project`);
  }
}

// 👉 Usage
const emp = new Employee("Aditya");
const project = new Project();

project.start(emp);
```

**What this proves**

- `Project` does NOT store `Employee`
- `Employee` is only **used**
- Both objects exist independently → **Association**

---

Aggregation

```tsx
class Employee {
  constructor(public name: string) {}
}

class Company {
  constructor(private employees: Employee[]) {}

  listEmployees(): void {
    this.employees.forEach(e => console.log(e.name));
  }
}

// 👉 Usage
const e1 = new Employee("Aditya");
const e2 = new Employee("Rahul");

const company = new Company([e1, e2]);
company.listEmployees();
```

**What this proves**

- `Employee` objects created **outside**
- `Company` only holds references
- Employees survive even if company is destroyed → **Aggregation**

---

### Composition

```tsx
class Engine {
  start(): void {
    console.log("Engine started");
  }
}

class Car {
  private engine: Engine;

  constructor() {
    this.engine = new Engine(); // created inside
  }

  drive(): void {
    this.engine.start();
    console.log("Car driving");
  }
}

// 👉 Usage
const car = new Car();
car.drive();
```

**What this proves**

- `Engine` is created **inside** `Car`
- Cannot exist independently
- Lifecycle tied to `Car` → **Composition**

**One-liner to memorize:**

> Encapsulation protects how data is accessed, abstraction controls how components interact.
>