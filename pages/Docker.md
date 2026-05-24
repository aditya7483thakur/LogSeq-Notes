- Virutalization vs Containerization
  collapsed:: true
	-
	-
	-
	- | Feature | Virtualization | Containerization |
	  | ---- | ---- | ---- |
	  | Definition | Creates multiple virtual machines with separate operating systems | Creates isolated applications sharing the same host OS kernel |
	  | Main Unit | Virtual Machine (VM) | Container |
	  | Isolation Level | Hardware-level isolation | Process-level isolation |
	  | OS Included? | Yes, each VM has full guest OS | No full OS, only app + dependencies |
	  | Kernel | Each VM has its own kernel | All containers share host kernel |
	  | Size | Heavy (usually GBs) | Lightweight (usually MBs) |
	  | Startup Time | Slow (seconds/minutes) | Fast (milliseconds/seconds) |
	  | Resource Usage | High | Low |
	  | Performance | Lower due to virtualization overhead | Near-native performance |
	  | Portability | Moderate | Very high |
	  | Density | Fewer VMs per machine | More containers per machine |
	  | Boot Process | Full OS boot required | No OS boot required |
	  | Hardware Emulation | Required | Not required |
	  | Dependency Isolation | Via separate OS | Via container runtime isolation |
	  | Security Isolation | Stronger | Comparatively lighter |
	  | OS Flexibility | Different OS possible on same host | Same kernel family required |
	  | Typical Technology | Hypervisor | Container Engine |
	  | Common Tools | VMware, Hyper-V, VirtualBox, KVM | Docker, containerd, Podman |
	  | Infrastructure Requirement | More CPU/RAM/storage | Less CPU/RAM/storage |
	  | Best Use Cases | Multiple OS environments, legacy systems | Microservices, CI/CD, cloud-native apps |
	  | Deployment Speed | Slower | Faster |
	  | Scalability | Slower scaling | Rapid scaling |
	  | Maintenance | More complex | Easier |
	  | Image Contents | Full OS + app | App + dependencies only |
	  | Kernel Dependency | Independent kernel per VM | Shared host kernel |
	  | Failure Impact | VM failures isolated strongly | Kernel issues may affect containers |
	  | Example | Running Ubuntu VM inside Windows | Running Node.js container using Docker |
	- <!--EndFragment-->
	-
	-
	-
	-
	-
	-
- Docker Architecture
  collapsed:: true
	- # 1. What is Docker Architecture?
	  
	  Docker architecture defines: 
	  
	  How Docker components interact to build, manage, and run containers.
	  Docker follows a client-server architecture
	  where different components communicate with each other.
	- # 2. Main Components of Docker Architecture
	  
	  Docker mainly consists of:
	  | Component | Purpose |
	  | ---- | ---- | ---- |
	  | Docker Client | User interface to interact with Docker |
	  | Docker Daemon | Core engine that manages containers |
	  | Docker Engine | Complete Docker runtime |
	  | Docker Images | Blueprint/template for containers |
	  | Docker Containers | Running instances of images |
	  | Docker Registry | Stores Docker images |
	  ---
	- # 3. High-Level Architecture
	  
	  ```
	  Docker Client
	      ↓
	  Docker Daemon (Docker Engine)
	      ↓
	  ┌───────────────┐
	  │ Images        │
	  │ Containers    │
	  │ Networks      │
	  │ Volumes       │
	  └───────────────┘
	      ↓
	  Docker Registry (Docker Hub)
	  ```
	  
	  ---
	- # 4. Docker Client
	  
	  The Docker Client is the interface through which users interact with Docker.
	  
	  Example commands:
	  
	  ```
	  docker build
	  docker pull
	  docker run
	  docker ps
	  ```
	  
	  The client:
	- accepts commands
	- sends requests to Docker daemon
	  
	  ---
	- # 5. Docker Daemon (`dockerd`)
	  
	  Docker Daemon is the core background service.
	  
	  It:
	- builds images
	- runs containers
	- manages networks
	- manages storage
	- handles container lifecycle
	  
	  Daemon listens for Docker API requests.
	  
	  ---
	- # Important
	  
	  Docker daemon is the actual component doing the work.
	  
	  The client only sends instructions.
	  
	  ---
	- # 6. Docker Engine
	  
	  Docker Engine is the complete container runtime environment.
	  
	  It includes:
	- Docker daemon
	- REST API
	- Docker CLI
	  
	  ---
	- # Architecture Internally
	  
	  ```
	  Docker CLI
	     ↓
	  REST API
	     ↓
	  Docker Daemon
	  ```
	  
	  ---
	- # 7. Docker Images
	  
	  Docker Image is:
	  A read-only template used to create containers.
	  
	  It contains:
	- application code
	- libraries
	- dependencies
	- runtime
	- configurations
	  
	  ---
	- # Example
	  
	  Ubuntu image:
	- bash
	- apt
	- libraries
	- filesystem
	  
	  Node image:
	- Node.js runtime
	- npm
	- Linux filesystem
	  
	  ---
	- # Important Property
	  
	  Images are:
	- immutable (cannot be modified directly)
	  
	  ---
	- # 8. Docker Containers
	  
	  A container is:
	  A running instance of a Docker image.
	  
	  When image runs:
	- container gets created
	  
	  ---
	- # Example
	  
	  ```
	  docker run ubuntu
	  ```
	  
	  Flow:
	  
	  ```
	  Docker Image
	      ↓
	  Running Instance
	      ↓
	  Container
	  ```
	  
	  ---
	- # 9. Docker Registry
	  
	  Docker Registry stores Docker images.
	  
	  Most popular registry:
	- Docker Hub
- Docker Image and Dockerfile
  collapsed:: true
	- # 1. What is a Docker Image?
	  
	  A Docker Image is:
	  
	  > 
	  
	  A read-only template used to create Docker containers.
	  
	  It contains everything needed to run an application:
	- source code
	- runtime
	- libraries
	- dependencies
	- environment setup
	- configuration
	  
	  ---
	- # Important Understanding
	  
	  A Docker Image is like:
	- a blueprint
	- a snapshot
	- a packaged application environment
	  
	  Containers are created from images.
	  
	  ---
	- # Flow
	  
	  ```
	  Dockerfile
	    ↓
	  Docker Image
	    ↓
	  Docker Container
	  ```
	  
	  ---
	- # 2. What Does a Docker Image Contain?
	  
	  A Docker image can contain:
	  
	  | Component | Included? |
	  | ---- | ---- | ---- |
	  | Application source code | Yes |
	  | Runtime (Node/Python/Java) | Yes |
	  | Libraries | Yes |
	  | Dependencies | Yes |
	  | Environment variables | Yes |
	  | Configurations | Yes |
	  | Full OS kernel | No |
	  
	  ---
	- # Important Point
	- ## YES — Source code is usually included inside image.
	  
	  For example:
	- React app files
	- Node.js backend code
	- Python scripts
	  
	  can all exist inside image.
	  
	  ---
	- # Example Node.js Image
	  
	  ```
	  Docker Image
	  │
	  ├── Node.js Runtime
	  ├── npm Packages
	  ├── Application Source Code
	  ├── Environment Config
	  └── Startup Command
	  ```
	  
	  ---
	- # 3. Why Include Source Code in Image?
	  
	  Because image should be:
	- portable
	- reproducible
	- self-contained
	  
	  So image carries everything required to run app anywhere.
	  
	  ---
	- # Example
	  
	  Suppose you build a backend image.
	  
	  That image may contain:
	- Express server code
	- package.json
	- node_modules
	- Node.js runtime
	  
	  Then anyone can run it without manual setup.
	  
	  ---
	- # 4. What is a Dockerfile?
	  
	  A Dockerfile is:
	  
	  > 
	  
	  A text file containing instructions to build a Docker image.
	  
	  It defines:
	- what base image to use
	- what files to copy
	- what commands to run
	- how application should start
	  
	  ---
	- # Dockerfile → Recipe
	  
	  Image → Final cooked product
	  
	  ---
	- # 5. Example Dockerfile
	- ## Node.js Backend Example
	  
	  ```
	  FROM node:20
	  
	  WORKDIR /app
	  
	  COPY package*.json ./
	  
	  RUN npm install
	  
	  COPY . .
	  
	  EXPOSE 5000
	  
	  CMD ["npm", "start"]
	  ```
	  
	  ---
	- # 6. Step-by-Step Explanation
	  
	  ---
	- ## `FROM node:20`
	  
	  ```
	  FROM node:20
	  ```
	  
	  Base image.
	  
	  Docker downloads:
	- Node.js runtime
	- Linux filesystem
	  
	  ---
	- ## `WORKDIR /app`
	  
	  ```
	  WORKDIR /app
	  ```
	  
	  Sets working directory inside container.
	  
	  ---
	- ## `COPY package*.json ./`
	  
	  ```
	  COPY package*.json ./
	  ```
	  
	  Copies package files into image.
	  
	  ---
	- ## `RUN npm install`
	  
	  ```
	  RUN npm install
	  ```
	  
	  Installs dependencies inside image.
	  
	  ---
	- ## `COPY . .`
	  
	  ```
	  COPY . .
	  ```
	  
	  Copies entire source code into image.
	  
	  ---
	- # VERY IMPORTANT
	  
	  This is where your:
	- backend code
	- frontend code
	- application files
	  
	  enter the image.
	  
	  So YES:
	- source code becomes part of image.
	  
	  ---
	- ## `EXPOSE 5000`
	  
	  ```
	  EXPOSE 5000
	  ```
	  
	  Documents container port.
	  
	  ---
	- ## `CMD ["npm", "start"]`
	  
	  ```
	  CMD ["npm", "start"]
	  ```
	  
	  Default startup command when container runs.
	  
	  ---
	- # 7. Image Build Process
	  
	  Suppose:
	  
	  ```
	  docker build -t myapp .
	  ```
	  
	  Docker:
	- reads Dockerfile
	- executes instructions step-by-step
	- creates image layers
	- builds final image
	  
	  ---
	- # Build Flow
	  
	  ```
	  Dockerfile
	    ↓
	  Read Instructions
	    ↓
	  Execute Step-by-Step
	    ↓
	  Create Image Layers
	    ↓
	  Final Docker Image
	  ```
	  
	  ---
	- # 8. Image Layers
	  
	  Docker images are layered.
	  
	  Each instruction creates a new layer.
	  
	  Example:
	  
	  ```
	  FROM node:20
	  COPY package.json .
	  RUN npm install
	  COPY . .
	  ```
	  
	  Layers:
	  
	  ```
	  Layer 1 → node:20
	  Layer 2 → package.json
	  Layer 3 → npm install
	  Layer 4 → source code
	  ```
	  
	  ---
	- # Why Layers Matter
	  
	  Benefits:
	- caching
	- faster builds
	- layer reuse
	- reduced storage
	  
	  ---
	- # 9. What Happens When Container Starts?
	  
	  When you run:
	  
	  ```
	  docker run myapp
	  ```
	  
	  Docker:
	- creates writable layer over image
	- starts application process
	  
	  ---
	- # Important
	  
	  Image itself is:
	- immutable (read-only)
	  
	  Container adds:
	- writable runtime layer
	  
	  ---
	- # 10. Difference Between Dockerfile, Image, and Container
	  
	  | Component | Meaning |
	  | ---- | ---- | ---- |
	  | Dockerfile | Instructions to build image |
	  | Docker Image | Packaged application template |
	  | Docker Container | Running instance of image |
- Dockerfile, CMD, ENTRYPOINT, and Docker Build
  collapsed:: true
	- # 1. What is a Dockerfile?
	  
	  A Dockerfile is:
	  
	  > 
	  
	  A text file containing instructions to build a Docker image.
	  
	  It defines:
	- base image
	- dependencies
	- source code copying
	- startup commands
	- environment configuration
	  
	  ---
	- # Flow
	  
	  ```
	  Dockerfile
	     ↓
	  docker build
	     ↓
	  Docker Image
	     ↓
	  docker run
	     ↓
	  Container
	  ```
	  
	  ---
	- # 2. How to Create a Dockerfile
	  
	  Create a file named:
	  
	  ```
	  Dockerfile
	  ```
	  
	  (no extension)
	  
	  ---
	- # 3. Basic Dockerfile Structure
	  
	  Example Node.js backend:
	  
	  ```
	  FROM node:20-alpine
	  
	  WORKDIR /app
	  
	  COPY package*.json ./
	  
	  RUN npm install
	  
	  COPY . .
	  
	  EXPOSE 5000
	  
	  CMD ["npm", "start"]
	  ```
	  
	  ---
	- # 4. Important Dockerfile Instructions
	  
	  | Instruction | Purpose |
	  | ---- | ---- | ---- |
	  | FROM | Base image |
	  | WORKDIR | Set working directory |
	  | COPY | Copy files into image |
	  | RUN | Execute command during build |
	  | CMD | Default command when container starts |
	  | ENTRYPOINT | Main executable for container |
	  | EXPOSE | Document container port |
	  | ENV | Set environment variables |
	  
	  ---
	- # 5. `FROM`
	  
	  Defines base image.
	  
	  Example:
	  
	  ```
	  FROM node:20-alpine
	  ```
	  
	  This image already contains:
	- Linux filesystem
	- Node.js runtime
	  
	  ---
	- # 6. `WORKDIR`
	  
	  Sets current directory inside container.
	  
	  ```
	  WORKDIR /app
	  ```
	  
	  Equivalent to:
	  
	  ```
	  cd /app
	  ```
	  
	  ---
	- # 7. `COPY`
	  
	  Copies files from local machine into image.
	  
	  ```
	  COPY . .
	  ```
	  
	  Meaning:
	- copy everything from current local directory
	- into current container directory
	  
	  ---
	- # 8. `RUN`
	  
	  Executes command during image build.
	  
	  ```
	  RUN npm install
	  ```
	  
	  Runs while creating image.
	  
	  Result becomes part of image layer.
	  
	  ---
	- # VERY IMPORTANT
	- ## `RUN` executes during IMAGE BUILD
	  
	  NOT during container startup.
	  
	  ---
	- # 9. `CMD`
	  
	  Defines default command when container starts.
	  
	  Example:
	  
	  ```
	  CMD ["npm", "start"]
	  ```
	  
	  When container runs:
	- this command executes automatically
	  
	  ---
	- # Important Property of CMD
	- ## CMD can be overridden.
	  
	  ---
	- # Example
	  
	  Dockerfile:
	  
	  ```
	  CMD ["npm", "start"]
	  ```
	  
	  Run normally:
	  
	  ```
	  docker run myapp
	  ```
	  
	  Actually executes:
	  
	  ```
	  npm start
	  ```
	  
	  ---
	- # Override CMD
	  
	  ```
	  docker run myapp node server.js
	  ```
	  
	  Now:
	- `npm start` ignored
	- `node server.js` runs instead
	  
	  ---
	- # Why CMD Exists?
	  
	  CMD provides:
	- default startup behavior
	- flexibility to override
	  
	  ---
	- # 10. ENTRYPOINT
	  
	  ENTRYPOINT defines:
	  
	  > 
	  
	  The main fixed executable of container.
	  
	  Example:
	  
	  ```
	  ENTRYPOINT ["node"]
	  ```
	  
	  Now container behaves like:
	- a Node.js executable
	  
	  ---
	- # Example
	  
	  Run:
	  
	  ```
	  docker run myapp app.js
	  ```
	  
	  Internally executes:
	  
	  ```
	  node app.js
	  ```
	  
	  ---
	- # Important Difference
	  
	  | CMD | ENTRYPOINT |
	  | ---- | ---- | ---- |
	  | Default command | Fixed executable |
	  | Easily overridden | Usually not overridden |
	  | Flexible | Enforces container behavior |
	  
	  ---
	- # 11. Why ENTRYPOINT is Special
	  
	  ENTRYPOINT makes container act like:
	- executable application
	  
	  Example:
	- nginx container always runs nginx
	- mysql container always runs mysql server
	  
	  ---
	- # Example
	  
	  ```
	  ENTRYPOINT ["python"]
	  ```
	  
	  Now container behaves like:
	- Python interpreter
	  
	  ---
	- # 12. CMD + ENTRYPOINT Together
	  
	  Very common pattern.
	  
	  Example:
	  
	  ```
	  ENTRYPOINT ["node"]
	  CMD ["server.js"]
	  ```
	  
	  ---
	- # Result
	  
	  Default execution:
	  
	  ```
	  node server.js
	  ```
	  
	  ---
	- # Override Only CMD
	  
	  ```
	  docker run myapp app2.js
	  ```
	  
	  Now executes:
	  
	  ```
	  node app2.js
	  ```
	  
	  ENTRYPOINT remains fixed.
	  
	  CMD changes.
	  
	  ---
	- # Very Important Understanding
	- ## ENTRYPOINT = fixed base command
	- ## CMD = default arguments
	  
	  ---
	- # 13. Docker Build
	  
	  Command:
	  
	  ```
	  docker build -t myapp .
	  ```
	  
	  ---
	- # What Happens During Build?
	  
	  Docker:
	- reads Dockerfile
	- executes instructions sequentially
	- creates layers
	- generates image
	  
	  ---
	- # Build Flow
	  
	  ```
	  Dockerfile
	     ↓
	  Docker Build Process
	     ↓
	  Image Layers
	     ↓
	  Final Docker Image
	  ```
	  
	  ---
	- # Meaning of Command
	  
	  ```
	  docker build -t myapp .
	  ```
	  
	  | Part | Meaning |
	  | ---- | ---- | ---- |
	  | docker build | Build image |
	  | -t myapp | Tag/name image |
	  | . | Current directory as build context |
	  
	  ---
	- # 14. What is Build Context?
	  
	  The `.` means:
	  
	  ```
	  docker build .
	  ```
	  
	  Docker can access:
	- all files in current directory
	  
	  during build.
	  
	  ---
	- # 15. Docker Image Layers
	  
	  Every instruction creates layer.
	  
	  Example:
	  
	  ```
	  FROM node:20
	  COPY package.json .
	  RUN npm install
	  COPY . .
	  ```
	  
	  Layers:
	  
	  ```
	  Layer 1 → node base image
	  Layer 2 → package.json
	  Layer 3 → npm install
	  Layer 4 → source code
	  ```
	  
	  ---
	- # Why Layers Are Important
	  
	  Benefits:
	- caching
	- faster rebuilds
	- storage optimization
	- layer reuse
	  
	  ---
	- # 16. Important Interview Questions
	- ## Q: Difference between RUN and CMD?
	  
	  | RUN | CMD |
	  | ---- | ---- | ---- |
	  | Executes during image build | Executes during container startup |
	  | Creates image layer | Does not create build layer |
	  | Used for installation/setup | Used for startup command |
- Docker Networking
  id:: 6a00976b-d28c-42cb-9991-eea066b3309f
  collapsed:: true
	- # 1. What is Docker Networking?
	  
	  Docker networking allows:
	- containers to communicate with each other
	- containers to access the internet
	- external users/services to access containers
	  
	  Without networking:
	- containers would be isolated processes only.
	  
	  ---
	- # Why Docker Networking is Needed
	  
	  Real applications usually contain multiple services:
	  
	  Example:
	  
	  ```
	  Frontend
	   ↓
	  Backend API
	   ↓
	  Database
	  ```
	  
	  These services must communicate.
	  
	  Docker networking provides:
	- communication
	- isolation
	- DNS resolution
	- port exposure
	  
	  ---
	- # 2. Docker Network Types
	  
	  Main Docker network types:
	  
	  | Network Type | Purpose |
	  | ---- | ---- | ---- |
	  | Bridge | Default isolated network |
	  | User-defined Bridge | Custom bridge with DNS |
	  | Host | Shares host network directly |
	  | None | No networking |
	  
	  ---
	- # 3. Bridge Network (Default Network)
	  
	  Bridge is Docker’s default network.
	  
	  When you run:
	  
	  ```
	  docker run nginx
	  ```
	  
	  Docker automatically attaches container to:
	- bridge network
	  
	  ---
	- # Architecture
	  
	  ```
	  Host Machine
	      ↓
	  Docker Bridge Network
	      ↓
	  ┌──────────┐
	  │Container1│
	  └──────────┘
	  
	  ┌──────────┐
	  │Container2│
	  └──────────┘
	  ```
	  
	  ---
	- # What Happens in Bridge Network?
	  
	  Each container gets:
	- separate private IP
	- isolated network namespace
	  
	  Example:
	  
	  ```
	  Container A → 172.17.0.2
	  Container B → 172.17.0.3
	  ```
	  
	  ---
	- # Important
	  
	  Containers are isolated from:
	- host network
	- other external systems
	  
	  unless ports are exposed.
	  
	  ---
	- # 4. Port Mapping in Bridge Network
	  
	  Suppose app inside container runs on:
	- port 3000
	  
	  You expose it using:
	  
	  ```
	  docker run -p 3000:3000 myapp
	  ```
	  
	  ---
	- # Meaning
	  
	  ```
	  Host Port 3000
	        ↓
	  Container Port 3000
	  ```
	  
	  ---
	- # Real Traffic Flow
	  
	  ```
	  Browser
	   ↓
	  Host:3000
	   ↓
	  Docker NAT/Port Forwarding
	   ↓
	  Container:3000
	  ```
	  
	  ---
	- # VERY IMPORTANT
	  
	  Container STILL has:
	- separate IP
	- isolated network
	  
	  Port mapping only forwards traffic.
	  
	  ---
	- # Why Bridge Network is Useful
	  
	  Benefits:
	- isolation
	- security
	- multiple containers can use same internal ports
	  
	  ---
	- # Example
	  
	  Two containers both running internally on:
	- port 3000
	  
	  Possible because they have different private IPs.
	  
	  ```
	  Container A → 172.17.0.2:3000
	  Container B → 172.17.0.3:3000
	  ```
	  
	  Host exposes them differently:
	  
	  ```
	  docker run -p 3000:3000 app1
	  docker run -p 3001:3000 app2
	  ```
	  
	  ---
	- # 5. User-Defined Bridge Network (Custom Bridge)
	  
	  Custom bridge is manually created.
	  
	  Example:
	  
	  ```
	  docker network create mynetwork
	  ```
	  
	  Run container:
	  
	  ```
	  docker run --network mynetwork nginx
	  ```
	  
	  ---
	- # Why Custom Bridge is Better
	  
	  Provides:
	- automatic DNS resolution
	- container-name communication
	- better microservice communication
	- better isolation
	  
	  ---
	- # Example
	  
	  Backend:
	  
	  ```
	  docker run --name backend --network mynetwork mybackend
	  ```
	  
	  Frontend:
	  
	  ```
	  docker run --name frontend --network mynetwork myfrontend
	  ```
	  
	  Now frontend can call:
	  
	  ```
	  http://backend:5000
	  ```
	  
	  Docker automatically resolves:
	  
	  ```
	  backend → container IP
	  ```
	  
	  using internal DNS.
	  
	  ---
	- # VERY IMPORTANT
	- ## User-defined bridge provides built-in DNS.
	  
	  Default bridge does NOT provide good container-name communication.
	  
	  ---
	- # 6. Host Network
	  
	  Host networking removes network isolation.
	  
	  Run:
	  
	  ```
	  docker run --network host myapp
	  ```
	  
	  ---
	- # What Happens?
	  
	  Container directly shares:
	- host machine network stack
	  
	  Container does NOT get:
	- separate IP
	- isolated network namespace
	  
	  ---
	- # Architecture
	  
	  ```
	  Host Machine Network
	        ↕
	  Container uses same network directly
	  ```
	  
	  ---
	- # Important Difference from Port Mapping
	  
	  ---
	- ## Bridge + Port Mapping
	  
	  ```
	  Host Port
	    ↓
	  Docker Forwarding
	    ↓
	  Container Port
	  ```
	  
	  Container isolated.
	  
	  ---
	- ## Host Network
	  
	  ```
	  Host Port
	    ↓
	  Container directly uses it
	  ```
	  
	  No forwarding.
	  
	  No separate IP.
	  
	  ---
	- # Example
	  
	  Bridge mode:
	  
	  ```
	  docker run -p 3000:3000 myapp
	  ```
	  
	  Host mode:
	  
	  ```
	  docker run --network host myapp
	  ```
	  
	  No `-p` needed.
	  
	  ---
	- # Advantages of Host Network
	- faster networking
	- low latency
	- no NAT overhead
	  
	  ---
	- # Disadvantages
	- less secure
	- no isolation
	- port conflicts possible
	  
	  ---
	- # Example of Conflict
	  
	  If container uses:
	- port 3000
	  
	  and host already uses:
	- port 3000
	  
	  container cannot start.
	  
	  ---
	- # 7. None Network
	  
	  Completely disables networking.
	  
	  Example:
	  
	  ```
	  docker run --network none ubuntu
	  ```
	  
	  ---
	- # What Happens?
	  
	  Container gets:
	- no internet
	- no external communication
	- no inter-container communication
	  
	  ---
	- # Use Cases
	  
	  Useful for:
	- maximum isolation
	- offline batch jobs
	- security-sensitive workloads
	  
	  ---
	- # 8. Compare Docker Network Types
	  
	  | Feature | Bridge | User-Defined Bridge | Host | None |
	  | ---- | ---- | ---- |
	  | Default Network | Yes | No | No | No |
	  | Separate Container IP | Yes | Yes | No | No |
	  | Network Isolation | Yes | Yes | No | Complete |
	  | Internet Access | Yes | Yes | Yes | No |
	  | Container Name DNS | Limited | Yes | N/A | No |
	  | Port Mapping Needed | Yes | Yes | No | No |
	  | Performance | Good | Good | Best | N/A |
	  | Security | Good | Better | Lower | Highest |
	  
	  ---
	- # 9. Important Docker Networking Commands
	  
	  ---
	- ## List Networks
	  
	  ```
	  docker network ls
	  ```
	  
	  ---
	- ## Inspect Network
	  
	  ```
	  docker network inspect bridge
	  ```
	  
	  ---
	- ## Create Custom Network
	  
	  ```
	  docker network create mynetwork
	  ```
	  
	  ---
	- ## Connect Container to Network
	  
	  ```
	  docker network connect mynetwork mycontainer
	  ```
	  
	  ---
	- # 10. Real-World Example
	  
	  Suppose application contains:
	- frontend
	- backend
	- database
	  
	  Best practice:
	  
	  ```
	  frontend
	   ↓
	  backend
	   ↓
	  database
	  ```
	  
	  All connected using:
	- custom bridge network
	  
	  Services communicate using:
	- container names
	  
	  Example:
	- frontend → backend
	- backend → database
- Docker Volumes and Storage
  collapsed:: true
	- # 1. Why Docker Storage is Important
	  
	  By default, containers are:
	- temporary
	- ephemeral
	  
	  This means:
	  When container is deleted, its data is also lost.
	  
	  ---
	- # Example Problem
	  
	  Suppose MySQL container stores:
	- users
	- orders
	- database records
	  
	  If container gets deleted:
	  
	  ```
	  docker rm mysql-container
	  ```
	  
	  all database data may disappear.
	  
	  This is a huge problem.
	  
	  ---
	- # Docker Storage solves this problem.
	  
	  ---
	- # 2. What is Docker Volume?
	  
	  A Docker Volume is:
	  A persistent storage mechanism managed by Docker.
	  
	  Volumes allow data to:
	- survive container deletion
	- be shared between containers
	- exist independently of containers
	  
	  ---
	- # Core Idea
	  
	  Container lifecycle:
	- temporary
	  
	  Volume lifecycle:
	- independent
	  
	  ---
	- # Architecture
	  
	  ```
	  Container
	    ↓
	  Docker Volume
	    ↓
	  Host Storage
	  ```
	  
	  ---
	- # 3. Without Volume vs With Volume
	  
	  ---
	- # Without Volume
	  
	  ```
	  Container Filesystem
	       ↓
	  Container Deleted
	       ↓
	  Data Lost
	  ```
	  
	  ---
	- # With Volume
	  
	  ```
	  Container
	    ↓
	  Volume
	    ↓
	  Host Storage
	  
	  Container Deleted
	       ↓
	  Volume Still Exists
	  ```
	  
	  ---
	- # 4. Types of Docker Storage
	  
	  Main Docker storage types:
	  
	  | Storage Type | Purpose |
	  | ---- | ---- | ---- |
	  | Volumes | Docker-managed persistent storage |
	  | Bind Mounts | Direct host folder mapping |
	  | tmpfs Mounts | In-memory temporary storage |
	  
	  ---
	- # 5. Docker Volumes
	  
	  Most recommended storage mechanism.
	  
	  Docker manages:
	- location
	- permissions
	- lifecycle
	  
	  ---
	- # Create Volume
	  
	  ```
	  docker volume create myvolume
	  ```
	  
	  ---
	- # Use Volume
	  
	  ```
	  docker run -v myvolume:/app/data myapp
	  ```
	  
	  ---
	- # Meaning
	  
	  ```
	  Docker Volume → myvolume
	  Container Path → /app/data
	  ```
	  
	  Anything written inside:
	  
	  ```
	  /app/data
	  ```
	  
	  gets stored persistently.
	  
	  ---
	- # Real Example
	  
	  Suppose app writes:
	  
	  ```
	  /app/data/users.json
	  ```
	  
	  Data actually goes into:
	- Docker-managed storage
	  
	  Even if container dies:
	- data survives.
	  
	  ---
	- # 6. Why Volumes Are Preferred
	  
	  Benefits:
	- persistent data
	- safer
	- portable
	- better performance
	- easy backups
	- shareable between containers
	  
	  ---
	- # 7. Bind Mounts
	  
	  Bind mounts directly map:
	- host machine folder
	- into container
	  
	  ---
	- # Example
	  
	  ```
	  docker run -v /home/aditya/project:/app myapp
	  ```
	  
	  Meaning:
	  
	  ```
	  Host Folder
	  /home/aditya/project
	        ↓
	  Mapped Into Container
	  /app
	  ```
	  
	  ---
	- # Important
	  
	  Changes reflect both ways.
	  
	  If you edit local files:
	- container sees changes instantly.
	  
	  ---
	- # Very Common Use
	  
	  Development environments.
	  
	  Example:
	- live code editing
	- hot reload
	  
	  ---
	- # Example
	  
	  You edit:
	  
	  ```
	  index.js
	  ```
	  
	  on local machine.
	  
	  Container immediately sees updated file.
	  
	  No rebuild needed.
	  
	  ---
	- # 8. Volume vs Bind Mount
	  
	  | Feature | Volume | Bind Mount |
	  | ---- | ---- | ---- |
	  | Managed By | Docker | User |
	  | Storage Location | Docker internal | Any host path |
	  | Safer | Yes | Less |
	  | Portability | Better | Lower |
	  | Development Use | Less common | Very common |
	  | Production Use | Recommended | Less recommended |
	  
	  ---
	- # 9. tmpfs Mounts
	  
	  tmpfs stores data:
	- only in memory (RAM)
	  
	  No disk storage.
	  
	  ---
	- # Example
	  
	  ```
	  docker run --tmpfs /app/temp myapp
	  ```
	  
	  ---
	- # Features
	- very fast
	- temporary
	- disappears after container stops
	  
	  ---
	- # Use Cases
	  
	  Useful for:
	- sensitive temporary data
	- cache
	- session storage
	  
	  ---
	- # 10. Container Writable Layer
	  
	  Every container automatically gets:
	- writable layer
	  
	  ---
	- # Example
	  
	  Suppose container writes:
	  
	  ```
	  /app/log.txt
	  ```
	  
	  without volume.
	  
	  File exists only:
	- inside container writable layer
	  
	  ---
	- # Problem
	  
	  If container deleted:
	- writable layer deleted
	- data lost
	  
	  ---
	- # VERY IMPORTANT
	  
	  Container writable layer is NOT persistent storage.
	  
	  ---
	- # 11. Real Database Example
	  
	  Suppose MySQL container:
	  
	  ```
	  docker run -d \
	  -v mysql-data:/var/lib/mysql \
	  mysql
	  ```
	  
	  ---
	- # Meaning
	  
	  ```
	  Volume → mysql-data
	  Container DB Path → /var/lib/mysql
	  ```
	  
	  Now:
	- database survives container recreation.
	  
	  ---
	- # 12. Sharing Volumes Between Containers
	  
	  Multiple containers can use same volume.
	  
	  Example:
	  
	  ```
	  docker run -v shared-data:/data app1
	  docker run -v shared-data:/data app2
	  ```
	  
	  Both containers access same files.
	  
	  ---
	- # Architecture
	  
	  ```
	  Container1
	      ↓
	  Shared Volume
	      ↑
	  Container2
	  ```
	  
	  ---
	- # 13. Useful Docker Volume Commands
	  
	  ---
	- ## List Volumes
	  
	  ```
	  docker volume ls
	  ```
	  
	  ---
	- ## Inspect Volume
	  
	  ```
	  docker volume inspect myvolume
	  ```
	  
	  ---
	- ## Remove Volume
	  
	  ```
	  docker volume rm myvolume
	  ```
	  
	  ---
	- # 14. Anonymous Volumes
	  
	  If no volume name provided:
	  
	  ```
	  docker run -v /app/data myapp
	  ```
	  
	  Docker creates:
	- anonymous volume automatically
- Docker Compose
  collapsed:: true
	- # 1. What is Docker Compose?
	  
	  Docker Compose is a tool used to:
	  
	  > 
	  
	  Define and run multiple Docker containers together using a single YAML file.
	  
	  ---
	- # Problem Without Docker Compose
	  
	  Suppose application has:
	- frontend
	- backend
	- MongoDB
	  
	  Without Compose, you manually run:
	  
	  ```
	  docker network create mynetwork
	  
	  docker run -d --name mongodb --network mynetwork mongo
	  
	  docker run -d --name backend --network mynetwork backend-image
	  
	  docker run -d --name frontend --network mynetwork frontend-image
	  ```
	  
	  Managing many containers manually becomes difficult.
	  
	  ---
	- # Docker Compose Solves This
	  
	  You define everything in:
	  
	  ```
	  docker-compose.yml
	  ```
	  
	  Then run entire application using:
	  
	  ```
	  docker compose up
	  ```
	  
	  ---
	- # 2. Main Benefits of Docker Compose
	  
	  | Benefit | Explanation |
	  | ---- | ---- | ---- |
	  | Multi-container management | Run all services together |
	  | Easy networking | Automatic service communication |
	  | Centralized configuration | Everything in one YAML file |
	  | Easy startup/shutdown | One command |
	  | Better development workflow | Simplifies local environments |
	  
	  ---
	- # 3. Docker Compose Architecture
	  
	  ```
	  docker-compose.yml
	        ↓
	  Docker Compose
	        ↓
	  ┌──────────┐
	  │Frontend  │
	  └──────────┘
	  
	  ┌──────────┐
	  │Backend   │
	  └──────────┘
	  
	  ┌──────────┐
	  │MongoDB   │
	  └──────────┘
	  ```
	  
	  ---
	- # 4. Docker Compose File
	  
	  Compose uses:
	  
	  ```
	  docker-compose.yml
	  ```
	  
	  written in:
	- YAML format
	  
	  ---
	- # 5. Real Example (MERN Application)
	  
	  Suppose application has:
	- frontend
	- backend
	- MongoDB
	  
	  ---
	- # Example Compose File
	  
	  ```
	  version: "3.9"
	  
	  services:
	  
	  frontend:
	    build: ./frontend
	    ports:
	      - "3000:3000"
	    depends_on:
	      - backend
	  
	  backend:
	    build: ./backend
	    ports:
	      - "5000:5000"
	    environment:
	      MONGO_URI: mongodb://mongo:27017/mydb
	    depends_on:
	      - mongo
	  
	  mongo:
	    image: mongo
	    ports:
	      - "27017:27017"
	    volumes:
	      - mongo-data:/data/db
	  
	  volumes:
	  mongo-data:
	  ```
	  
	  ---
	- # 6. Step-by-Step Explanation
	  
	  ---
	- # `services`
	  
	  ```
	  services:
	  ```
	  
	  Defines all containers/services.
	  
	  ---
	- # Frontend Service
	  
	  ```
	  frontend:
	  build: ./frontend
	  ```
	  
	  Meaning:
	- build Docker image from:
	  
	  ```
	  ./frontend
	  ```
	  
	  folder.
	  
	  ---
	- # Port Mapping
	  
	  ```
	  ports:
	  - "3000:3000"
	  ```
	  
	  Meaning:
	  
	  ```
	  Host Port 3000
	        ↓
	  Container Port 3000
	  ```
	  
	  ---
	- # `depends_on`
	  
	  ```
	  depends_on:
	  - backend
	  ```
	  
	  Meaning:
	- frontend starts after backend.
	  
	  ---
	- # Backend Service
	  
	  ```
	  backend:
	  ```
	  
	  Another container/service.
	  
	  ---
	- # Environment Variables
	  
	  ```
	  environment:
	  MONGO_URI: mongodb://mongo:27017/mydb
	  ```
	  
	  Very important.
	  
	  ---
	- # Why `mongo` Works Here?
	  
	  Docker Compose automatically creates:
	- custom bridge network
	- internal DNS
	  
	  So backend can communicate using:
	  
	  ```
	  mongo
	  ```
	  
	  instead of IP address.
	  
	  ---
	- # Mongo Service
	  
	  ```
	  mongo:
	  image: mongo
	  ```
	  
	  Uses official MongoDB image directly from Docker Hub.
	  
	  ---
	- # Volume
	  
	  ```
	  volumes:
	  - mongo-data:/data/db
	  ```
	  
	  Meaning:
	  
	  ```
	  Docker Volume → mongo-data
	  Container Path → /data/db
	  ```
	  
	  MongoDB data becomes persistent.
	  
	  ---
	- # Global Volume Declaration
	  
	  ```
	  volumes:
	  mongo-data:
	  ```
	  
	  Creates named Docker volume.
	  
	  ---
	- # 7. How Docker Compose Networking Works
	  
	  Docker Compose automatically creates:
	- custom bridge network
	  
	  All services join same network.
	  
	  ---
	- # Result
	  
	  Containers communicate using:
	- service names
	  
	  Example:
	  
	  ```
	  frontend → backend
	  backend → mongo
	  ```
	  
	  No manual IP handling needed.
	  
	  ---
	- # 8. Important Docker Compose Commands
	  
	  ---
	- # Start Application
	  
	  ```
	  docker compose up
	  ```
	  
	  ---
	- # Run in Background
	  
	  ```
	  docker compose up -d
	  ```
	  
	  ---
	- # Stop Application
	  
	  ```
	  docker compose down
	  ```
	  
	  ---
	- # View Running Containers
	  
	  ```
	  docker compose ps
	  ```
	  
	  ---
	- # Rebuild Images
	  
	  ```
	  docker compose up --build
	  ```
	  
	  ---
	- # View Logs
	  
	  ```
	  docker compose logs
	  ```
	  
	  ---
	- # 9. What Happens Internally?
	  
	  When you run:
	  
	  ```
	  docker compose up
	  ```
	  
	  Docker Compose:
	- reads YAML file
	- creates network
	- builds images
	- creates containers
	- attaches volumes
	- starts services
	  
	  automatically.
	  
	  ---
	- # Internal Flow
	  
	  ```
	  docker-compose.yml
	        ↓
	  Create Network
	        ↓
	  Build Images
	        ↓
	  Create Containers
	        ↓
	  Attach Volumes
	        ↓
	  Start Services
	  ```
	  
	  ---
	- # 10. Why Docker Compose is Useful
	  
	  Without Compose:
	- many long commands
	- manual networking
	- manual startup ordering
	  
	  With Compose:
	- entire infrastructure in one file
	  
	  ---
	- # Real-World Usage
	  
	  Very common for:
	- local development
	- microservices
	- testing environments
	- CI/CD pipelines
- Docker Compose vs Kubernetes
  collapsed:: true
	- | Feature | Docker Compose | Kubernetes |
	  | ---- | ---- | ---- |
	  | Purpose | Multi-container management | Production container orchestration |
	  | Scale | Single machine | Multi-node cluster |
	  | Complexity | Simple | Complex |
	  | Setup | Easy | Difficult |
	  | Best Use Case | Development/testing | Production systems |
	  | Scaling | Limited | Massive |
	  | Auto-Healing | No | Yes |
	  | Load Balancing | Minimal | Advanced |
	  | Networking | Simple | Advanced cluster networking |
	  | Storage | Docker volumes | PV/PVC |
	  | High Availability | Limited | Strong |
	  | Rolling Updates | Limited | Built-in |
	  | Service Discovery | Basic | Advanced |
	  | Cloud Integration | Minimal | Extensive |
	  | Self-Healing | No | Yes |
	  | Infrastructure Management | Small scale | Enterprise scale |