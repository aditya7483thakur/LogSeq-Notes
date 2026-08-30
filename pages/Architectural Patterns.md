# 📦 Repository Pattern (with Example)
collapsed:: true
	- ## 🔹 Definition
	  
	  Repository Pattern introduces a **separate layer for database operations**, so business logic does not directly depend on the database. ([Alex Rusin Blog](https://blog.alexrusin.com/clean-architecture-in-node-js-implementing-the-repository-pattern-with-typescript-and-prisma/?utm_source=chatgpt.com))
	  
	  👉 It acts as a **middle layer between service and database**
	  
	  ---
	- ## 🔹 Architecture Flow
	  
	  Controller → Service → Repository → Database
	  
	  ---
	- ## 🔹 Responsibilities
	- ### 🎯 Controller (HTTP Layer)
	- Handles HTTP request/response
	- Calls service
	- Sends **status codes + JSON response**
	  
	  ✅ Example:
	  
	  ```tsx
	  res.status(201).json({ data: user });
	  ```
	  
	  ❌ Should NOT:
	- Call DB
	- Contain business logic
	  
	  ---
	- ### 🧠 Service (Business Logic Layer)
	- Core logic (validation, decisions)
	- Calls repository
	- Returns **plain data**
	- Throws errors
	  
	  ❌ Should NOT:
	- Return `{ status, body }`
	- Use `req` or `res`
	  
	  👉 Why?
	  
	  Because service should work in:
	- Cron jobs
	- Background workers
	- CLI scripts
	  
	  ---
	- ### 🗄️ Repository (Data Layer)
	- Handles DB queries (Prisma, Mongo, etc.)
	- Performs CRUD operations
	  
	  ❌ Should NOT:
	- Contain business logic
	- Handle validation
	  
	  ---
	- ## 🔹 Key Idea
	  
	  👉 Business logic should NOT depend on database
	  
	  👉 Repository hides DB implementation details ([Albert Hernández](https://alberthernandez.dev/blog/understanding-the-repository-pattern-in-node-js?utm_source=chatgpt.com))
	  
	  ---
	- # 🔥 Example (Based on YOUR Code)
	  
	  ---
	- ## 1. Repository
	  
	  ```tsx
	  // user.repository.ts
	  
	  export const userRepository = {
	  async createUser(data) {
	    return prisma.user.create({ data });
	  },
	  
	  async findUserByEmail(email: string) {
	    return prisma.user.findUnique({ where: { email } });
	  }
	  };
	  ```
	  
	  ---
	- ## 2. Service
	  
	  ```tsx
	  // user.service.ts
	  
	  export const createClerkUserService = async (data: any) => {
	  const { id, email_addresses, first_name, last_name, image_url } = data;
	  
	  const email = email_addresses?.[0]?.email_address;
	  
	  if (!id || !email) {
	    throw new Error("Invalid Clerk payload");
	  }
	  
	  const existingUser = await userRepository.findUserByEmail(email);
	  
	  if (existingUser) {
	    return existingUser;
	  }
	  
	  const newUser = await userRepository.createUser({
	    clerkUserId: id,
	    email,
	    name: `${first_name} ${last_name}`,
	    imageUrl: image_url,
	  });
	  
	  return newUser;
	  };
	  ```
	  
	  ---
	- ## 3. Controller
	  
	  ```tsx
	  // user.controller.ts
	  
	  export const createClerkUser = async (req, res) => {
	  try {
	    const user = await createClerkUserService(req.body?.data);
	  
	    return res.status(201).json({
	      message: "User created or already exists",
	      data: user,
	    });
	  
	  } catch (error) {
	    if (error.message === "Invalid Clerk payload") {
	      return res.status(400).json({ error: error.message });
	    }
	  
	    return res.status(500).json({ error: "Internal server error" });
	  }
	  };
	  ```
	  
	  ---
	- # 🔹 Benefits
	- Clean separation of concerns
	- Easy testing (mock repository)
	- Scalable architecture
	- DB can be changed easily
	  
	  ---
	- # 🔹 Common Mistakes ❌
	- Returning `{ status, body }` from service
	- Writing DB queries in controller
	- Putting logic inside repository
	  
	  ---
	- # 🔹 One-line Summary (Interview)
	  
	  Repository Pattern separates database logic into a dedicated layer, keeping services focused on business logic and controllers focused on HTTP handling.