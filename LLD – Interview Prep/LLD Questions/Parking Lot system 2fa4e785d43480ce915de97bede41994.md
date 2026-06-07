# Parking Lot system

## Requirements & Clarifications

- Support **Two-Wheeler** and **Four-Wheeler**
- Parking lot has **multiple floors**
- Each floor has **fixed parking spots per vehicle type**
- On **entry**:
    - Assign a suitable spot
    - Generate a ticket
- On **exit**:
    - Calculate parking fee
    - Release the spot
- **Hourly pricing**, varies by vehicle type
- Track **real-time availability**
- Assume **payment is always successful**
- Out of scope:
    - Monthly passes, discounts
    - Extra services (washing, cleaning)
    - Admin dashboards, reports

---

## Core Entities & Responsibilities

### Vehicle

- Represents the vehicle being parked
- Holds:
    - vehicleNumber
    - vehicleType (enum)

---

### ParkingSpot

- Represents a single physical spot
- Manages only its **own state**
- Responsibilities:
    - Check availability
    - Occupy itself
    - Release itself
- Does **not** decide allocation

**Key methods**

- `isAvailable()`
- `occupy(vehicle)`
- `release()`

---

### ParkingFloor

- Groups multiple parking spots
- Knows availability **per vehicle type**
- Helps search for a free spot on that floor

**Key methods**

- `hasAvailableSpot(vehicleType)`
- `findAvailableSpot(vehicleType)`

---

### ParkingLot

- **Orchestrator** of the system
- Handles entry and exit flows
- Delegates:
    - spot search → ParkingFloor
    - pricing → PricingStrategy

**Key methods**

- `parkVehicle(vehicle)`
- `unparkVehicle(ticket)`
- `hasAvailableSpot(vehicleType)`

---

### Ticket

- Represents one parking session
- Source of truth during exit
- Holds:
    - ticketId
    - vehicle
    - assignedParkingSpot
    - entryTime

---

### PricingStrategy

- Calculates parking charges
- Encapsulates pricing rules
- Allows pricing changes without modifying ParkingLot

**Key method**

- `calculateAmount(ticket)`

---

## Key Design Decisions

- **Composition over inheritance**
- Use `VehicleType` enum instead of separate vehicle subclasses
- ParkingLot is a **thin orchestrator**
- ParkingSpot is **passive** (state-only)
- Ticket avoids unnecessary lookups (no `findVehicle`)
- Pricing logic abstracted to support future changes
- No premature features (YAGNI)

---

## Trade-offs & Extension Scenarios

- Adding new vehicle types → update `VehicleType` and pricing strategy
- Adding new pricing rules → new `PricingStrategy` implementation
- Large parking lot → improve floor/spot search logic
- Multiple parking lots → extract ParkingLot as a higher-level manager
- Async entry/exit handling → decouple via events or queues (LLD-safe mention)

## CODE → [**https://github.com/aditya7483thakur/LLD-Problems/tree/master/Parking Lot**](https://github.com/aditya7483thakur/LLD-Problems/tree/master/Parking%20Lot)

## Patterns used implicitly

### 1️⃣ **Strategy Pattern** (Explicit, intentional)

**Where?**

`PricingStrategy` + `HourlyPricingStrategy`

### Why this is a Strategy Pattern

- Pricing logic can **vary**
- ParkingLot should not know *how* pricing works
- Behavior is selected via **composition**

```tsx
interfacePricingStrategy {calculateAmount(ticket:Ticket,exitTime:Date):number;
}
```

ParkingLot depends on the **abstraction**, not the implementation.

### Interview explanation (say this)

> “Pricing is a behavior that can change, so I isolated it using the Strategy pattern. This keeps ParkingLot closed for modification but open for extension.”
> 

✔ OCP

✔ DIP

✔ Clean

---

### 2️⃣ **Facade Pattern (Lightweight)**

**Where?**

`ParkingLot`

ParkingLot acts as a **simple interface** to a complex subsystem:

- floors
- spots
- tickets
- pricing

Clients only interact with:

```tsx
parkVehicle()unparkVehicle()
```

They don’t care about:

- how spots are searched
- how pricing is calculated
- how floors are structured

### Interview explanation

> “ParkingLot acts like a facade that hides internal complexity and exposes a simple API for entry and exit.”
> 

⚠️ Say **“facade-like”**, not heavy Facade pattern.

---

### 3️⃣ **Composition over Inheritance** (Design principle, not pattern)

**Where?**

- ParkingLot → has ParkingFloors
- ParkingFloor → has ParkingSpots
- ParkingLot → has PricingStrategy

We avoided:

- deep inheritance trees
- `TwoWheelerSpot extends ParkingSpot`
- `HourlyParkingLot extends ParkingLot`

### Interview explanation

> “I preferred composition over inheritance so behavior and structure can evolve independently.”
> 

Interviewers **love** this line.