- [Fundamentals+of+Networking+for+Effective+Backends-v2.pptx](Computer%20Networking/FundamentalsofNetworkingforEffectiveBackends-v2.pptx)
-
- ### What is a network? LAN, WAN, MAN
  collapsed:: true
	- ### 1. What is a Network?
		- A **network** is a **collection of interconnected devices (hosts)** that can **communicate and share resources**.
		- Devices in a network can include **computers, servers, routers, switches, printers, etc.**
	- ### 2. Types of Networks
		- | Type | Full Form | Scope / Coverage | Example |
		- | --- | --- | --- | --- |
		- | **LAN** | Local Area Network | Small area, like a building or campus | Office Wi-Fi, Home network |
		- | **MAN** | Metropolitan Area Network | City-wide network | University campus network across a city |
		- | **WAN** | Wide Area Network | Large area, often country/worldwide | Internet, corporate networks connecting multiple cities |
	- ### 3. Key Differences
		- **LAN:** High speed, low cost, limited area
		- **MAN:** Medium speed, connects multiple LANs in a city
		- **WAN:** Lower speed compared to LAN, long distance, expensive, connects multiple MANs or LANs
	- ### 4. Analogy
		- LAN = your **apartment building**
		- MAN = **city roads connecting buildings**
		- WAN = **highways connecting cities across the country/world**
- collapsed:: true
- ### Clients and servers
	- ### 1. What is a Client?
		- A **client** is a **device or program** that **requests services or resources** from another device (the server).
		- It initiates communication.
			- **Examples:**
		- Your **laptop or phone** using a web browser to visit a website
		- Email client requesting emails from a mail server
		- Mobile app requesting data from a cloud service
	- ### 2. What is a Server?
		- A **server** is a **device or program** that **provides services or resources** to clients.
		- It **waits for requests** and responds.
			- **Examples:**
		- Web server hosting a website
		- Database server storing user data
		- File server in an office providing shared documents
	- ### 3. Client-Server Model
		- **Clients** request → **Servers** respond
		- Enables **centralized resource management**
		- Can work over **LAN or WAN**
			- **Example:**
			- 1. You open a browser (client) → request a webpage
			- 2. Web server receives the request → sends the page back
			- 3. Your browser displays the page
			- ### 4. Key Points
		- **Clients:** Initiators, can be many
		- **Servers:** Responders, usually fewer, may handle multiple clients simultaneously
		- Common model in **web, email, database, cloud services**
- collapsed:: true
- ### Peer-to-peer vs client-server
	- **1. Client-Server Model**
	- **Explanation:**
	- This is the **traditional model** of networking.
	- There is a **central server** that provides resources or services.
	- Clients (like your computer, phone, or browser) **request services from the server**.
	- **Key Points:**
	- **Server:** Centralized, stores data, serves multiple clients.
	- **Client:** Requests data/services, usually doesn’t share its own data.
	- **Examples:**
	- Websites (browser = client, web server = server)
	- Email (Outlook/Gmail = client, mail server = server)
	- Online banking apps
		- **Pros:**
	- Centralized control → easy to manage, secure.
	- Data backup is easier.
	- **Cons:**
	- Single point of failure → if server goes down, clients cannot access services.
	- Can become a bottleneck under heavy load.
	- **2. Peer-to-Peer (P2P) Model**
	- **Explanation:**
	- There is **no central server**.
	- Every device (peer) can **act as both client and server**.
	- Devices **share resources directly** with each other.
	- **Key Points:**
	- All nodes are equal (peers).
	- Resources (files, processing power) are distributed.
	- Examples:
	- Torrent networks
	- Some blockchain systems
	- Local file sharing apps (like older versions of Skype, BitTorrent)
		- **Pros:**
	- No single point of failure → more resilient.
	- Efficient for sharing large amounts of data.
	- **Cons:**
	- Harder to manage and secure.
	- Data availability depends on peers being online.
	- ### Comparison Table
	- | Feature | Client-Server | Peer-to-Peer (P2P) |
	- | --- | --- | --- |
	- | Central Server | Yes | No |
	- | Control | Centralized | Distributed |
	- | Failure Point | Single point of failure | No single failure point |
	- | Resource Sharing | Server provides, client consumes | Each peer shares & consumes |
	- | Examples | Websites, email, apps | Torrents, blockchain |
-
- ### OSI Model Notes
  collapsed:: true
	- ### Definition
		- OSI (Open Systems Interconnection) model is a **standardized framework** that explains **how data travels from one device to another**.
		- Divides network communication into **7 layers**, each with specific responsibilities.
	- ### 7 Layers of OSI (Top → Bottom)
		- | Layer | What it handles | What can be “exposed” / inspected |
		- | --- | --- | --- |
		- | 7. Application | User apps, data meaning | URLs, HTTP content, emails, chat messages |
		- | 6. Presentation | Data formatting, encryption | Encryption type, compressed data |
		- | 5. Session | Connection/session management | Session IDs, authentication tokens |
		- | 4. Transport | TCP/UDP ports, sequence numbers | Ports (22 for SSH, 80 for HTTP), protocols, connection state |
		- | 3. Network | IP addressing, routing | Source/destination IPs, routing info |
		- | 2. Data Link | Frames, MAC addresses | MAC addresses, VLAN tags |
		- | 1. Physical | Bits over medium | Signals, cables, wireless waves |
	- ### Transport Layer Detail
		- **TCP (connection-oriented)** uses a **3-way handshake**: SYN → SYN-ACK → ACK
		- Ensures **reliable, ordered delivery** of packets before actual data transfer.
		- UDP (connectionless) skips this handshake, less reliable but faster.
	- ### Why OSI is Needed
		- Different systems/vendors had **incompatible methods** for sending, formatting, and error-handling data → communication failures.
		- 1. **Standardization** → Different systems/vendors can communicate.
		- 2. **Abstraction** → Upper layers don’t care how data is transmitted.
		- 3. **Interoperability** → Works across Wi-Fi, fiber, copper, different OS and hardware.
		- 4. **Error Handling** → Standard methods for detecting & recovering from errors.
		- 5. **Troubleshooting & Maintenance** → Layered structure isolates issues easily.
		- 6. **Scalability & Flexibility** → Changes in one layer don’t affect others.
		- ### Data Flow Example (Sending “HELLO”)
		- 1. **Application:** Type message “HELLO”
		- 2. **Presentation:** Convert to bytes, encrypt/compress if needed
		- 3. **Session:** Establish connection/session
		- 4. **Transport (TCP):** 3-way handshake → packetize + sequence numbers + checksum
		- 5. **Network:** Add IP addresses, decide route
		- 6. **Data Link:** Frame packets, add MAC addresses, error detection
		- 7. **Physical:** Transmit bits over Wi-Fi, fiber, or copper
		- **Receiver reverses steps** to reconstruct the original message.
-
- ### TCP/IP 4-Layer Model
  collapsed:: true
	- **Definition:**
	- TCP/IP (Transmission Control Protocol / Internet Protocol) is the **practical networking model used in the Internet and real-world apps**.
	- OSI is **conceptual/theoretical**, TCP/IP is **practical**.
	- ### TCP/IP Layers (Real-World Focus)
	- | TCP/IP Layer | OSI Equivalent | Key Function | Examples / Notes |
	- | --- | --- | --- | --- |
	- | **Application** | OSI 5–7 (Application, Presentation, Session) | Provides network services directly to apps | HTTP, HTTPS, FTP, SMTP, DNS; what apps “see” |
	- | **Transport** | OSI 4 (Transport) | Ensures end-to-end communication, error handling | TCP (reliable), UDP (fast, connectionless), ports |
	- | **Internet** | OSI 3 (Network) | Logical addressing and routing | IP (IPv4/IPv6), ICMP, routers, subnets |
	- | **Network Access / Link** | OSI 1–2 (Physical + Data Link) | Node-to-node delivery, MAC addressing, physical transmission | Ethernet, Wi-Fi, ARP, cables, switches |
	- ### Data Flow Example (Real-World)
	- 1. Browser sends **HTTP request** → Application layer
	- 2. TCP splits data into packets → Transport layer
	- 3. Adds source/destination IP → Internet layer
	- 4. Converts packets to frames, sends bits over cable/Wi-Fi → Network Access layer
-
- ### RPC
  collapsed:: true
	- > “RPC stands for Remote Procedure Call. It allows a program to call a function on another computer as if it were local, abstracting away all the network details.”
	- >
	- ### Optional Follow-up
	- If the interviewer probes further, you can add:
	- Client sends a request, server executes the function, result is returned.
	- Looks like a **normal function call**, but it happens over the network.
	- Used in **distributed systems and client-server architectures**.
- collapsed:: true
- ### MAC Address (Media Access Control Address)
  collapsed:: true
	- ### 🧠 **1. MAC Address – The “Physical” Identity of a Device**
		- **MAC (Media Access Control) address** = unique **hardware identifier** burned into a device’s **Network Interface Card (NIC)**.
		- 📦 **Example:**
	- ```
	  A4:5E:60:8B:CC:12
	  ```
		- It’s 48 bits (6 bytes), usually written in hexadecimal (base 16).
	- ### 🧩 **Structure of a MAC Address**
		- The **first 3 bytes** → manufacturer ID (OUI - Organizationally Unique Identifier)
		- The **last 3 bytes** → unique serial number for the device
			- So, every network device (computer, phone, router, printer) has a **unique MAC address**.
	- ### ⚙️ **Where it operates:**
		- Works at **Layer 2 (Data Link Layer)** of the OSI model.
		- Used **only within the local network (LAN)** — it does *not* travel over the internet.
		- When a device sends data to another device in the same LAN, it must know the **MAC address** of the destination device.
	- ### 🔍 **2. How Devices Identify Each Other on a LAN**
		- Imagine your laptop (Device A) wants to send a packet to another computer (Device B) **in the same Wi-Fi or Ethernet network**.
	- ### Here’s what happens:
		- 1. Each device in the LAN has:
		- An **IP address** (logical address, Layer 3)
		- A **MAC address** (hardware address, Layer 2)
		- 2. When your system sends data to another IP in the same network, it **needs to know the target’s MAC address** to actually deliver the frame on the wire.
		- 3. This is where **ARP (Address Resolution Protocol)** comes in.
		- ### 🛰️ **3. ARP (Address Resolution Protocol)**
		- ARP is the bridge between **IP addresses** and **MAC addresses**.
		- ### ⚡ The job of ARP:
		- Translate IP → MAC (so data can be delivered inside a LAN).
	- ### 📨 **ARP Process Example**
		- Let’s say:
		- Device A: IP → 192.168.1.10, MAC → A4:5E:60:8B:CC:12
		- Device B: IP → 192.168.1.11, MAC → 9C:35:EB:4F:7A:02
		- Now, Device A wants to send data to 192.168.1.11.
	- ### Step 1 → ARP Request (Broadcast)
		- Device A doesn’t know B’s MAC yet.
		- So, it sends a broadcast message to everyone in the LAN:
		- > “Who has IP 192.168.1.11? Please tell 192.168.1.10.”
		- >
		- 🟡 This ARP request is sent to a **special broadcast MAC address**:
		- `FF:FF:FF:FF:FF:FF` (means *everyone on the LAN hears it*).
	- ### Step 2 → ARP Reply (Unicast)
		- Device B sees the message and replies directly to Device A:
		- > “I’m 192.168.1.11, and my MAC is 9C:35:EB:4F:7A:02.”
		- >
		- Now A knows how to reach B physically.
	- ### Step 3 → ARP Cache
		- Device A stores this mapping in its **ARP table**:
	- ```
	  192.168.1.11 → 9C:35:EB:4F:7A:02
	  ```
		- So next time, A doesn’t need to broadcast again—it already knows the MAC.
-
- ### Internet Protocol (IP) & Related Tools
  collapsed:: true
	- ### 🧠 1. What is an IP?
		- **IP (Internet Protocol)** is the **unique address and set of rules** that allow devices to identify and communicate with each other over a network.
		- 📍 Think of it like a **house address system** — every device needs one so that data (like letters) knows where to go.
	- ### 📦 Types of IP Addresses
		- | Type | Description | Example |
		- | --- | --- | --- |
		- | **IPv4** | 32-bit address written as 4 numbers (0–255) | `192.168.1.1` |
		- | **IPv6** | 128-bit, newer format using hexadecimal | `2405:204:3a9a::1` |
		- IPv4 allows about **4.3 billion** addresses — nearly exhausted.
		- IPv6 was introduced to support **~3.4 × 10³⁸** unique addresses.
	- ### 🌐 Private vs Public IP
		- | Type | Used For | Example Range | Visible on Internet? |
		- | --- | --- | --- | --- |
		- | **Private IP** | Inside local network (home/office) | `192.168.x.x`, `10.x.x.x`, `172.16–31.x.x` | ❌ No |
		- | **Public IP** | Assigned by ISP, visible online | `49.205.71.24` | ✅ Yes |
		- 👉 Example:
		- Your **laptop (192.168.1.5)** → **Router (49.205.71.24)** → **Internet**
	- ### ⚙️ How IP Works (Step-by-Step)
		- 1. Every device gets an **IP address**.
		- 2. Data is split into **packets**.
		- 3. Each packet carries:
		- **Source IP** (sender)
		- **Destination IP** (receiver)
		- 4. **Routers** read the destination IP and forward packets to the right path.
		- 🧭 IP is a **Network Layer (Layer 3)** protocol in the OSI model.
		- ### 🧩 Key Concepts
		- ### a) Subnet
		- A **subset of IPs** within a network.
		- Example: `192.168.1.0/24` → `192.168.1.1–192.168.1.254`
	- ### b) Gateway
		- The **router’s internal IP**, e.g. `192.168.1.1`, which connects your LAN to the internet.
	- ### c) DNS
		- Converts **domain names → IP addresses**, e.g. `google.com → 142.250.182.206`.
	- ### d) NAT (Network Address Translation)
		- Allows many **private IPs** to share **one public IP** — handled by your router.
	- ### 🧭 Backend / DevOps Context
		- Your deployed **backend server** has a **public IP**.
		- Your **database** often has a **private IP** within a **VPC (Virtual Private Cloud)**.
		- Both communicate over IP (using ports like 80 for HTTP, 443 for HTTPS).
			- **Think of it like:**
			- 🧠 The **postal address system of the internet** — it defines *where* to send data and ensures it reaches the correct destination.
	- ### 📦 2. IP Packet
		- An **IP Packet** is the **basic unit of data** transmitted using the Internet Protocol.
		- It consists of two parts:
		- 1. **Header** → contains metadata like source, destination, TTL, protocol, etc.
		- 2. **Payload** → the actual data (like part of a web page, file, or email).
		- ### 📑 Key Fields in the IPv4 Header
		- | Field | Description |
		- | --- | --- |
		- | **Version** | IPv4 or IPv6 |
		- | **Header Length** | Size of the header |
		- | **Total Length** | Header + Payload |
		- | **TTL (Time To Live)** | Number of hops allowed before discarding |
		- | **Protocol** | Next layer (TCP = 6, UDP = 17, ICMP = 1) |
		- | **Source IP** | Sender’s address |
		- | **Destination IP** | Receiver’s address |
		- ### ⚙️ How It Works
		- Data is divided into packets.
		- Each packet is wrapped with an IP header → **forms an IP Packet**.
		- Routers read headers to route packets across networks.
		- Destination reassembles packets in the correct order.
		- **Think of it like:**
		- ✉️ A **digital envelope** that carries your data with a sender and receiver address written on it.
	- ### 🌐 3. ICMP (Internet Control Message Protocol)
		- **ICMP** is a **support protocol** used by network devices to send **error messages** and **status reports**.
		- It doesn’t carry user data like HTTP or TCP — instead, it carries **control information**.
	- ### 📚 Common ICMP Message Types
		- | Type | Meaning |
		- | --- | --- |
		- | **Echo Request / Echo Reply** | Used by `ping` |
		- | **Destination Unreachable** | Router can’t reach target |
		- | **Time Exceeded** | Used by `traceroute` when TTL hits 0 |
		- | **Redirect** | Suggests a better route |
	- ### 🧭 Role in Networking
		- Routers and hosts use ICMP to:
		- Report unreachable networks
		- Detect routing loops
		- Help tools like Ping & Traceroute diagnose connectivity
		- **Think of it like:**
		- 📨 The **postal service’s feedback system** — it doesn’t carry letters, but tells you *why* your letter didn’t reach or *how* it traveled.
	- ### ⚡ 4. Ping Command
		- **Ping** is a **network diagnostic tool** that uses **ICMP Echo Request and Echo Reply** to test connectivity between two devices.
	- ### 🧠 What Happens When You Ping
		- 1. Your device sends an **ICMP Echo Request** to a target.
		- 2. The target replies with an **ICMP Echo Reply**.
		- 3. The OS measures:
		- **Round-trip time (RTT)** in ms
		- **TTL (Time To Live)**
		- **Packet loss**
	- ### 🧮 Example Output
	- ```
	  Pinging 10.50.53.164 with 32 bytes of data:
	  Reply from 10.50.53.164: bytes=32 time=84ms TTL=64
	  ```
		- | Field | Meaning |
		- | --- | --- |
		- | `bytes=32` | Packet size |
		- | `time=84ms` | Round-trip time |
		- | `TTL=64` | Packet passed 64 hops |
		- 📊 Lower time → faster connection.
		- ❌ No reply → host unreachable.
		- **Think of it like:**
		- 🏓 Saying “Hello?” and waiting for a “Hi!” back — to check if the other person is reachable and how fast they respond.
	- ### 🧭 5. Traceroute (or Traceback)
		- **Traceroute** (Windows: `tracert`) is a tool that shows the **path packets take** to reach a destination.
	- ### ⚙️ How It Works
		- 1. Sends packets with **increasing TTL (Time To Live)**.
		- 2. Each router decreases TTL by 1.
		- 3. When TTL = 0, router sends back **ICMP Time Exceeded**.
		- 4. The tool records each router (hop) that responded.
		- ### 🧩 Example Output
	- ```
	  traceroute google.com
	  1  192.168.1.1       (your router)
	  2  10.12.14.1        (ISP gateway)
	  3  142.250.182.206   (Google server)
	  ```
		- ### 💡 Difference Between Ping and Traceroute
		- | Tool | Purpose | Protocol Used |
		- | --- | --- | --- |
		- | **Ping** | Checks if destination is reachable | ICMP Echo |
		- | **Traceroute** | Maps the path packets take | ICMP Time Exceeded |
		- **Think of it like:**
		- 🗺️ Following your **letter’s journey** through each post office until it reaches its destination.
		- ### 🧩 Quick Summary Diagram
	- ```
	  IP (Internet Protocol)
	  │
	  ├── IP Packet → carries data
	  │
	  └── ICMP → helper protocol for IP
	  │
	  ├── Ping → checks if destination is reachable
	  └── Traceroute → traces the route packets travel
	  ```
		- ### 🧩 Step-by-step lifecycle
		- ### 1. You (the sender) want to communicate
		- Let’s say your computer (192.168.1.2) wants to **ping google.com**.
		- You type:
	- ```bash
	  ping google.com
	  ```
		- DNS first resolves `google.com` → **142.250.190.78** (Google’s IP).
			- So now your system knows **where** to send packets.
		- ### 2. Creating the IP packet
		- Your system prepares an **ICMP Echo Request** message — basically:
		- > “Hey 142.250.190.78, are you alive?”
		- >
		- That ICMP message is wrapped inside an **IP packet**, which contains:
		- | Field | Description |
		- | --- | --- |
		- | **Source IP** | 192.168.1.2 (your IP) |
		- | **Destination IP** | 142.250.190.78 (Google) |
		- | **Protocol** | ICMP |
		- | **TTL (Time To Live)** | usually starts at 64 or 128 |
		- | **Payload** | “Ping data” |
		- ### 3. The packet travels across routers
		- Each **router** along the path does this:
		- Reads the **destination IP**.
		- **Decrements TTL by 1**.
		- If TTL > 0 → forwards to the next router.
		- If TTL == 0 → packet is discarded, and the router sends an **ICMP “Time Exceeded”** message back to the sender.
		- That “TTL expired” mechanism is exactly what **traceroute** uses.
		- ### 4. ICMP in action
		- When the IP packet finally reaches **142.250.190.78 (Google)**:
		- Google’s router sees the ICMP Echo Request.
		- It replies with an **ICMP Echo Reply** back to you.
		- This reply is also wrapped in a new **IP packet**, but now:
			- **Source IP** = 142.250.190.78
			- **Destination IP** = 192.168.1.2
		- When your system receives the Echo Reply, it shows:
	- ```
	  Reply from 142.250.190.78: bytes=32 time=50ms TTL=115
	  ```
		- ### 5. What traceroute does differently
		- Traceroute sends multiple packets with **increasing TTL values**:
		- | TTL | What happens |
		- | --- | --- |
		- | 1 | Router #1 sends ICMP “time exceeded” → hop 1 found |
		- | 2 | Router #2 sends ICMP “time exceeded” → hop 2 found |
		- | ... | continues until destination reached |
		- | final | Destination sends “ICMP Echo Reply” → route complete |
		- That’s how you get a full **path of routers from you to the destination.**
-
- ### Address Resolution Protocol
	- ### 🧭 ARP (Address Resolution Protocol)
	- > Think of it like: asking around the neighborhood,
	- >
	- >
	- > “Who lives at this house number?” — to find the person’s actual name.
	- >
	- > In networking terms → ARP helps find the **MAC address** of a device when we only know its **IP address**.
	- >
	- ### 🌐 1. What is ARP?
	- **ARP (Address Resolution Protocol)** is a **Network Layer helper protocol** used to map:
	- ```
	  IP address  ➜  MAC address
	  ```
	- It operates between **Layer 2 (Data Link)** and **Layer 3 (Network)** in the OSI model —
	- helping IP (Layer 3) communicate over Ethernet or Wi-Fi (Layer 2).
	- ### ⚙️ 2. Why Do We Need ARP?
	- Let’s imagine your computer wants to send a packet to `192.168.1.1` (your router).
	- Your OS prepares an **IP packet**:
	- ```
	  Source IP: 192.168.1.5
	  Destination IP: 192.168.1.1
	  ```
	- ✅ Great — IP knows *where logically* the data should go.
	- ❌ But your **Network Interface Card (NIC)** can’t send data using IP addresses —
	- it needs a **MAC address** (hardware address).
	- This is because:
	- **Layer 2 (Ethernet/Wi-Fi)** uses **MAC addresses** to deliver data *inside a local network (LAN)*.
	- The **switches and routers** at this level don’t understand IP; they only know how to forward frames based on MAC.
	- So your system needs to ask:
	- > “Hey — who has IP 192.168.1.1? Tell me your MAC.”
	- >
	- And that’s where **ARP** comes in.
	- ### 📡 3. Step-by-Step: How ARP Works
	- ### 🧩 Scenario:
	- Your PC wants to send data to another device on the same LAN (say, your router).
	- ### 🪧 Step 1: Check ARP Cache
	- Your PC first looks into its **ARP cache** (a small table storing IP ↔ MAC mappings).
	- If found → it uses that MAC directly.
	- If not → go to Step 2.
	- ### 📢 Step 2: ARP Broadcast Request
	- Your PC broadcasts this message to all devices on the LAN:
	- > “Who has 192.168.1.1? Tell 192.168.1.5.”
	- >
	- 📦 This ARP Request frame looks like:
	- ```
	  Destination MAC: FF:FF:FF:FF:FF:FF   (broadcast to everyone)
	  Source MAC: AA:AA:AA:AA:AA:AA
	  Payload: "Who has IP 192.168.1.1?"
	  ```
	- ### 💬 Step 3: ARP Reply
	- Only the device with IP `192.168.1.1` replies back (unicast):
	- > “I am 192.168.1.1 — my MAC is BB:BB:BB:BB:BB:BB.”
	- >
	- 📦 ARP Reply frame:
	- ```
	  Destination MAC: AA:AA:AA:AA:AA:AA   (your computer)
	  Source MAC: BB:BB:BB:BB:BB:BB
	  Payload: "192.168.1.1 → BB:BB:BB:BB:BB:BB"
	  ```
	- ### 💾 Step 4: Store in ARP Cache
	- Your computer now saves:
	- ```
	  192.168.1.1 → BB:BB:BB:BB:BB:BB
	  ```
	- So next time, it doesn’t need to ask again — it already knows.
	- ### 🚀 Step 5: Send Data
	- Now your NIC can finally wrap your IP packet inside an Ethernet frame:
	- ```
	  Destination MAC: BB:BB:BB:BB:BB:BB
	  Source MAC: AA:AA:AA:AA:AA:AA
	  Payload: [IP packet]
	  ```
	- ✅ Frame sent successfully at Layer 2.
	- ✅ Packet travels at Layer 3 using IP.
	- ### 🧠 4. Why We Need MAC (and Why IP Alone Isn’t Enough)
	- | Concept | Works At | Used For | Known To |
	- | --- | --- | --- | --- |
	- | **IP Address** | Layer 3 (Network) | Logical addressing between networks | Routers |
	- | **MAC Address** | Layer 2 (Data Link) | Physical delivery within local network | Switches & NICs |
	- ### 💬 Explanation:
	- **IP** tells *which device logically* should receive the packet (like an address on a map).
	- But **inside your LAN**, devices don’t route by IP — your **switch** forwards frames using **MAC addresses** only.
	- Without a MAC:
	- Your NIC wouldn’t know which device to actually hand the data to.
	- Your packet would never even leave your computer properly.
	- 🧭 So:
	- > IP = “Who to send to logically”
	- >
	- >
	- > MAC = “Who to send to physically (on this network)”
	- >
	- > ARP = “How to find that physical address”
	- >
	- ### 🗂️ 5. ARP Cache Example (inside your system)
	- You can view it using:
	- ```bash
	  arp -a
	  ```
	- Example output:
	- ```
	  Interface: 192.168.1.5
	  Internet Address      Physical Address      Type
	  192.168.1.1           b8-27-eb-45-67-89     dynamic
	  192.168.1.10          00-0c-29-3e-2b-7a     dynamic
	  ```
	- ### 🛡️ 6. Bonus: ARP Spoofing (Security Note)
	- Attackers can **fake ARP replies** to trick your device into sending data to them instead of the real target (MITM attack).
	- That’s why modern switches and OSes use **ARP inspection** and **cache validation**.
	- ### 🧩 7. Summary Table
	- | Term | Meaning | Think of it like |
	- | --- | --- | --- |
	- | **ARP** | Finds MAC for a given IP | Asking “Who lives at this address?” |
	- | **MAC Address** | Hardware ID used in local network | Person’s name |
	- | **IP Address** | Logical address for devices on Internet | House address |
	- | **Reason we need MAC** | Ethernet/Wi-Fi can only send using MAC, not IP | The local delivery needs the exact door |
	- | **Reason we need ARP** | To translate IP → MAC before sending the frame | Finding the right door before handing the letter |
-
- ### UDP (User Datagram Protocol)
  collapsed:: true
	- ### 🌍 **Layer Information**
	- **OSI Layer:** Transport Layer (Layer 4)
	- **TCP/IP Layer:** Transport Layer
	- ### ⚙️ **Definition**
	- > UDP (User Datagram Protocol) is a connectionless, unreliable, and lightweight transport layer protocol used to send data quickly between applications over a network.
	- >
	- >
	- > It focuses on **speed and simplicity** rather than reliability.
	- >
	- ### 🧩 **Key Characteristics**
	- | **Feature** | **Description** |
	- | --- | --- |
	- | **Connectionless** | UDP doesn’t establish a connection before sending data — it just sends packets (datagrams) directly to the destination. |
	- | **Unreliable** | No acknowledgment, retransmission, or guarantee that data will reach the receiver. |
	- | **No Flow Control** | UDP doesn’t care if the receiver is slower; it keeps sending data even if the receiver’s buffer overflows — extra packets are simply dropped. |
	- | **No Congestion Control** | UDP doesn’t monitor network congestion or slow down when the network is overloaded — it always sends at the same rate. |
	- | **Message-Oriented** | Each packet (datagram) is independent — no concept of continuous data stream like TCP. |
	- | **Lightweight Protocol** | Very small header (8 bytes) → minimal processing overhead, ideal for high-speed communication. |
	- | **Supports Broadcast & Multicast** | Can send data to multiple receivers simultaneously — useful for streaming, discovery protocols, etc. |
	- | **Stateless** | The sender and receiver don’t maintain any session or connection state, making it scalable for large systems. |
	- ### 🧱 **UDP Header Size**
	- **Header size:** 8 bytes (very small compared to TCP).
	- ### 🔁 **Working Concept (Simplified Explanation)**
	- When an application wants to send data using UDP:
	- 1. The **application** gives data to UDP along with a **destination port** (identifying the target application on the receiving device).
	- 2. UDP wraps the data into a **datagram** and adds a small 8-byte header.
	- 3. It passes this datagram to the **IP layer**, which handles **delivery to the correct machine** based on the destination IP address.
	- 4. Once the datagram reaches the target machine, **UDP uses the port number** to deliver it to the correct application (e.g., DNS, video player, etc.).
	- 5. If packets are lost, duplicated, or arrive out of order — UDP itself doesn’t fix it; the **application** must handle it if necessary.
	- > 🧩 In short:
	- >
	- > - **IP** gets the packet to the **right machine**.
	- > - **UDP** gets it to the **right application** on that machine.
	- ### 🚀 **Advantages**
	- ✅ Very fast (no connection setup or acknowledgment overhead)
	- ✅ Lightweight header (8 bytes only)
	- ✅ Ideal for real-time or time-sensitive data
	- ✅ Supports broadcasting and multicasting
	- ✅ Simple and efficient implementation
	- ### ⚠️ **Disadvantages**
	- ❌ No delivery guarantee
	- ❌ No order guarantee
	- ❌ No error recovery or retransmission
	- ❌ No congestion control
	- ### 💼 **Common Use Cases**
	- UDP is preferred in applications where **speed and timeliness** are more important than perfect reliability, such as:
	- **Video streaming** (YouTube, Netflix real-time playback)
	- **Voice over IP (VoIP)** calls
	- **Online gaming** (real-time updates faster than waiting for lost packets)
	- **DNS (Domain Name System)** lookups
	- **DHCP (Dynamic Host Configuration Protocol)**
	- **SNMP (Simple Network Management Protocol)**
	- **NTP (Network Time Protocol)** for clock synchronization
	- **Live broadcasting / real-time data feeds**
	- ### 💻 **Common UDP-Based Protocols**
	- | Application | Protocol | Port |
	- | --- | --- | --- |
	- | DNS (Domain Name System) | UDP | 53 |
	- | DHCP (Dynamic Host Configuration Protocol) | UDP | 67/68 |
	- | SNMP (Simple Network Management Protocol) | UDP | 161/162 |
	- | TFTP (Trivial File Transfer Protocol) | UDP | 69 |
	- | NTP (Network Time Protocol) | UDP | 123 |
	- | RTP/RTCP (Real-time Transport Protocol) | UDP | varies |
	- | QUIC (used by HTTP/3, Google) | UDP | varies |
-
- ### TCP (Transmission Control Protocol)
  collapsed:: true
	- What is TCP?
		- TCP (Transmission Control Protocol) is a **transport layer protocol** providing:
		- Reliability
		- Ordered delivery
		- Error control
		- Congestion control
		- Flow control
		- Full-duplex connection
		- Stream-based communication
		- ### Key properties
		- | Property | Meaning |
		- | --- | --- |
		- | Reliable | Retransmits lost packets |
		- | Ordered | Sequence numbers ensure correct order |
		- | Connection-oriented | Requires handshake |
		- | Stream-based | Sends continuous stream of bytes |
		- | Congestion controlled | Adjusts speed based on network load |
	- Ports, Sockets & the 5-Tuple
		- ### 3.1 Ports
		- Ports identify "which application" inside a machine should receive the packet.
		- Examples:
		- 80 → HTTP
		- 443 → HTTPS
		- 22 → SSH
		- 5432 → PostgreSQL
		- ### 3.2 Sockets
		- A socket = IP + Port
		- It is the software endpoint for communication.
		- ### 3.3 Connection Socket
		- When a server accepts a connection:
		- Listening socket (port 80)
		- Per-client socket (unique 5-tuple)
		- ### 3.4 5-Tuple
		- Every TCP connection is uniquely identified by:
	- ```
	  (src IP, dst IP, src port, dst port, protocol)
	  ```
		- This allows:
		- Multiple users connecting to same server
		- One client connecting to multiple servers
		- Server knowing exactly which client to send data to
	- MSS & MTU
		- ### 4.1 MTU (Maximum Transmission Unit)
		- MTU = Maximum size of an IP packet on the network.
		- Default Ethernet MTU = **1500 bytes**.
		- Includes:
		- IP header (20 bytes)
		- TCP header (20 bytes)
		- ### 4.2 MSS (Maximum Segment Size)
		- MSS = Maximum amount of TCP payload inside a packet.
	- ```
	  MSS = MTU - IP header - TCP header
	  = 1500 - 20 - 20
	  = 1460 bytes
	  ```
		- ### 4.3 Why MSS Matters
		- Prevents fragmentation
		- Improves throughput
		- Negotiated during TCP handshake
	- TCP Handshake (3-Way Handshake)
		- TCP uses a **3-way handshake** to establish a reliable, ordered, byte-stream connection.
		- This handshake has two purposes:
		- 1. **Synchronize Initial Sequence Numbers (ISNs)** for both sides
		- 2. **Establish connection parameters** (MSS, Window Scale, SACK, timestamps, etc.)
		- Short Explanation
			- When a TCP connection starts, **both sides need to agree on:**
			- Initial sequence numbers
			- Ability to communicate
			- Synchronizing data flow
			- So they exchange **SYN and ACK flags together**, which requires **3 steps**:
			- ### Steps
			- 1️⃣ **Client → Server : SYN**
			- “I want to start a connection, here is my sequence number.”
			- 2️⃣ **Server → Client : SYN + ACK**
			- “Okay, I got your request. Here is my sequence number.”
			- 3️⃣ **Client → Server : ACK**
			- “I got your SYN, connection ready!”
			- ✔ After these 3 messages, both sides have synchronized.
		- Long Explanation
			- ### 🧩 What is ISN?
			- **ISN = Initial Sequence Number**, a 32-bit value randomly chosen by each side.
			- Why needed?
			- To prevent old packets (from previous connections) from being mistaken as new.
			- To protect against sequence-guessing attacks (TCP spoofing/hijacking).
			- To start sequence number space cleanly.
			- TCP uses sequence numbers per **byte**, and the first byte has sequence = **ISN**.
			- ### 🔵 Step 1 — SYN (Client → Server)
			- Client selects a random ISN **x** and sends:
	- ```
	  Flags: SYN
	  Seq = x
	  Ack = —
	  ```
		- Also includes TCP options such as:
		- MSS
		- Window Scale
		- SACK-permitted
		- Timestamps
		- **Meaning:**
		- “I want to start a connection, and my first byte will start at sequence **x**.”
		- State → **SYN_SENT**
		- ### 🔷 Step 2 — SYN + ACK (Server → Client)
		- Server picks its own random ISN **y** and replies:
	- ```
	  Flags: SYN, ACK
	  Seq = y
	  Ack = x + 1
	  ```
		- Why `Ack = x + 1`?
		- → Because the SYN consumes **one sequence number**.
		- State → **SYN_RCVD**
		- **Meaning:**
		- “I accept the connection.
		- I acknowledge your SYN at sequence x, expect your next byte at x+1,
		- And my own ISN is y.”
		- ### 🔵 Step 3 — ACK (Client → Server)
		- Client sends the final ACK:
	- ```
	  Flags: ACK
	  Seq = x + 1
	  Ack = y + 1
	  ```
		- State → **ESTABLISHED** (on both sides)
		- **Meaning:**
		- “I acknowledge your SYN at sequence y; expect my first data byte at x+1.”
		- ### 📌 Full Diagram
	- ```
	  Client                                         Server
	  ---------------------------------------------------------------
	  SYN, Seq = x  ------------------------------------->
	  <---------------------  SYN+ACK, Seq = y, Ack = x+1
	  ACK, Seq = x+1, Ack = y+1  ----------------------->
	  ```
		- ### 📝 Key Points for Interviews
		- ### ✔ Why 3 steps?
		- Because both sides need to send a SYN and receive an ACK; a 2-step handshake cannot confirm both directions.
		- ### ✔ Why does SYN consume 1 sequence number?
		- To keep the sequence number space consistent and avoid ambiguity.
		- ### ✔ Why random ISN?
		- Security and preventing old packets from mixing into new connections.
		- ### ✔ Final state after handshake?
		- Both sides know:
		- The remote ISN
		- The next expected sequence number
		- The negotiated options (MSS, window scaling, SACK)
		- ### ✔ What happens if the final ACK is lost?
		- Server retransmits SYN+ACK.
		- Client resends ACK.
		- TCP remains safe and consistent.
	- TCP Termination(4-Way Connection Termination)
		- TCP closes a connection using **four steps**, because **each direction** of data flow must be closed **independently**.
		- A TCP connection is **full-duplex** (two independent, one-way streams).
		- So one side can stop sending while still receiving.
		- Short Explanation
			- Closing needs **4 steps** because **closing is one-directional per side**.
			- Each side must say:
			- “I have finished sending” (FIN)
			- “I acknowledge you finished” (ACK)
			- So it takes **4 messages** total.
			- ### Steps
			- 1️⃣ **Client → Server : FIN**
			- “I am done sending.”
			- 2️⃣ **Server → Client : ACK**
			- “Okay, I won’t expect more data from you.”
			- 3️⃣ **Server → Client : FIN**
			- “Now I am also done sending.”
			- 4️⃣ **Client → Server : ACK**
			- “Okay, closing now.”
			- ✔ Connection fully closed.
			- ### 🔥 **Why not just 3-way for termination too?**
			- Because **sending data is independent for each direction**.
			- A side may say **I’m done**, but the other side may still need to send more data.
			- So FIN must be acknowledged separately for each direction → **2 FINs + 2 ACKs = 4 steps**.
		- Long Explanation
			- ### 🔶 Step-by-step Breakdown
			- Let’s assume **Client wants to close first**.
			- ### 🔵 **Step 1 — Client → FIN**
			- Client sends:
	- ```
	  Flags: FIN
	  Seq = u          // last sequence number sent + 1
	  Ack = v
	  ```
		- Meaning:
		- “I am done sending data, but I can still receive.”
		- Client state → **FIN_WAIT_1**
		- ### 🔷 **Step 2 — Server → ACK**
		- Server acknowledges the FIN:
	- ```
	  Flags: ACK
	  Seq = v
	  Ack = u + 1
	  ```
		- Meaning:
		- “I received your FIN, and I won't expect more data from you.”
		- Client state → **FIN_WAIT_2**
		- Server state → **CLOSE_WAIT**
		- Server may still send remaining data to client while in CLOSE_WAIT.
		- ### 🔵 **Step 3 — Server → FIN**
		- After sending remaining data, server sends:
	- ```
	  Flags: FIN
	  Seq = v2        // sequence number after all server data
	  Ack = u + 1
	  ```
		- Meaning:
		- “I am also done sending.”
		- Server state → **LAST_ACK**
		- ### 🔷 **Step 4 — Client → ACK**
		- Client acknowledges the server’s FIN:
	- ```
	  Flags: ACK
	  Seq = u + 1
	  Ack = v2 + 1
	  ```
		- Client state → **TIME_WAIT**
		- Server state → **CLOSED**
		- ### 📌 Why TIME_WAIT is important (very common interview point)
		- The client stays in **TIME_WAIT** for **2 × MSL (Maximum Segment Lifetime)** → typically **60 seconds**.
		- Why?
		- 1. **To ensure the final ACK is received by the server**
			- (If lost, server resends FIN; client must still be able to respond.)
		- 2. **To avoid old duplicate packets from previous connections interfering with new ones.**
		- ### 📌 Full ASCII Diagram
	- ```
	  Client                                         Server
	  ---------------------------------------------------------------
	  FIN, Seq = u  --------------------------------------->
	  <---------------------------  ACK, Ack = u+1
	  (waiting for server to finish sending data)
	  <---------------------------  FIN, Seq = v2
	  ACK, Ack = v2+1  ------------------------------------->
	  (Client enters TIME_WAIT)
	  ```
		- ### 📌 Frequently Asked Interview Questions
		- ### ✔ Why does termination require 4 packets but handshake requires 3?
		- Because **each direction** must close separately.
		- Handshake synchronizes both sides at once, but closing is independent.
		- ### ✔ Why does FIN consume a sequence number?
		- Same reason as SYN → logical consistency, marking end of data stream.
		- ### ✔ What is MSL?
		- Maximum time a TCP segment can live in the network (~30 sec).
		- TIME_WAIT = 2 × MSL (~60 sec).
		- ### ✔ Why doesn’t the server go into TIME_WAIT?
		- Whichever side sends the **final ACK** goes to TIME_WAIT.
		- Usually the **client**, because it closes firs
	- TCP Flow Control
		- Flow control ensures the **sender does not overwhelm the receiver**.
		- ### ✔ **Mechanism: Sliding Window**
		- The receiver advertises a **Receive Window (rwnd)**:
		- 👉 **rwnd = how many more bytes I can accept in my buffer**
		- 📌 **This rwnd value is included inside every TCP ACK packet**
		- TCP ACK packets contain a field in the TCP header called **Window Size**, which carries the rwnd value.
		- So the sender always knows how much data it is allowed to send.
		- ### ✔ **Sender rule**
		- The sender can transmit only if:
		- ### unACKed data ≤ rwnd
		- ### ✔ **How rwnd works dynamically**
		- If receiver’s buffer is filling → **rwnd decreases**
		- If buffer empties → **rwnd increases**
		- If **rwnd = 0** → sender must stop sending
		- This prevents buffer overflow.
		- ### ✔ **Zero Window + Zero Window Probe**
		- If receiver advertises:
	- ```
	  rwnd = 0
	  ```
		- Sender must pause transmission.
		- But sender periodically sends:
		- ### Zero Window Probe (ZWP) → a 1-byte packet
		- This checks whether the receiver buffer is free again.
		- If receiver replies with:
	- ```
	  rwnd > 0
	  ```
		- Sender resumes normal transmission.
		- ### ✔ **Why flow control is needed?**
		- Because the sender might be a **high-performance device** (e.g., a server), while the receiver might be **slow or resource-limited** (e.g., an IoT device).
		- Without flow control:
		- Buffers would overflow
		- Packets would be dropped
		- Throughput would decrease
		- Flow control ensures reliable and efficient communication.
	- TCP Congestion Control
		- TCP Congestion Control prevents **overloading the network** (routers, links).
		- It controls the *sender’s speed* using:
		- ### cwnd → Congestion Window
		- (window maintained by the sender)
		- Sender can send:
		- ### ✔ **data ≤ min(cwnd, rwnd)**
		- (cwnd = congestion control, rwnd = flow control)
		- ### ⭐ **Four Algorithms of TCP Congestion Control**
		- ### 1️⃣ **Slow Start (Exponential Growth)**
		- Starts with:
	- ```
	  cwnd = 1 MSS
	  ```
		- For every RTT:
	- ```
	  cwnd = cwnd * 2   (exponential growth)
	  ```
		- Continues until cwnd reaches:
	- ```
	  ssthresh (slow start threshold)
	  ```
		- ✔ Purpose: Quickly find available bandwidth.
		- ✔ Fast increase but controlled.
		- ### 2️⃣ **Congestion Avoidance (Linear Growth)**
		- Once:
	- ```
	  cwnd >= ssthresh
	  ```
		- Growth becomes **linear**:
	- ```
	  cwnd = cwnd + 1 MSS per RTT
	  ```
		- ✔ Slower, safer growth.
		- ✔ Avoids pushing too much traffic too fast.
		- ### 3️⃣ **Fast Retransmit (Packet Lost but Network Working)**
		- Triggered when sender receives:
	- ```
	  3 duplicate ACKs
	  ```
		- Meaning:
		- > “One packet is missing, but the connection is alive.”
		- >
		- ### 🧩 **Why 3 Duplicate ACKs = Packet Lost?**
		- Imagine the sender sends multiple packets:
	- ```
	  Packet 1
	  Packet 2
	  Packet 3   <-- gets lost in the network
	  Packet 4
	  Packet 5
	  ```
		- Receiver receives:
		- Packet 1 ✔
		- Packet 2 ✔
		- Packet 4 ❗ (out of order)
		- Packet 5 ❗ (out of order)
		- Receiver is EXPECTING packet **3**, but it keeps receiving later packets.
		- So what does the receiver do?
		- ### It keeps sending ACKs for **the last correctly received packet**.
		- Which is PACKET 2.
		- So the receiver sends:
	- ```
	  ACK 3
	  ACK 3
	  ACK 3
	  ACK 3
	  ```
		- Because receiver is saying:
		- > “I got everything UNTIL packet 2.
		- >
		- >
		- > I’m STILL waiting for packet 3.”
		- >
		- These are **duplicate ACKs**.
		- Sender performs **Fast Retransmit**:
	- ```
	  Retransmit the missing packet immediately
	  → without waiting for timeout (RTO)
	  ```
		- ✔ Faster than waiting for timeout
		- ✔ Indicates *mild* congestion
		- ### 4️⃣ **Fast Recovery (After Fast Retransmit)**
		- After retransmitting the lost packet:
	- ```
	  ssthresh = cwnd / 2
	  cwnd = ssthresh
	  ```
		- Then cwnd grows **linearly**.
		- ✔ We skip Slow Start
		- ✔ Because network is only *mildly* congested
		- ✔ Connection is still stable (since later packets arrived)
		- ### ❌ **Timeout (RTO) — Heaviest Punishment**
		- If packet loss is detected by **timeout**:
		- This means:
		- > “Serious congestion. Packets are not even reaching the receiver.”
		- >
		- Actions:
	- ```
	  ssthresh = cwnd / 2
	  cwnd = 1 MSS
	  → Restart Slow Start
	  ```
		- ✔ Timeout = worst signal
		- ✔ So TCP slows down heavily
		- ### 📌 **Summary Table (Super Clean)**
		- | Phase | Trigger | Growth | Meaning |
		- | --- | --- | --- | --- |
		- | **Slow Start** | Start or timeout | Exponential | Probe bandwidth quickly |
		- | **Congestion Avoidance** | cwnd ≥ ssthresh | Linear | Stable growth |
		- | **Fast Retransmit** | 3 duplicate ACKs | — | Quickly retransmit loss |
		- | **Fast Recovery** | After fast retransmit | Linear | Mild congestion recovery |
		- | **Timeout** | No ACK for RTO time | Reset to 1 MSS | Heavy congestion |
		- ### 🎯 **One-Line Interview Answer**
		- **“TCP congestion control uses a congestion window. It starts with Slow Start (exponential), then Congestion Avoidance (linear). Loss via 3 duplicate ACKs triggers Fast Retransmit and Fast Recovery (cut cwnd in half, linear growth). Loss via timeout resets cwnd to 1 MSS and restarts Slow Start.”**
	- Retransmissions & Loss Recovery
		- TCP is a **reliable** protocol → it must detect lost packets and resend them.
		- Retransmission happens in two situations:
		- ### 1️⃣ **Retransmission via RTO (Timeout-based Retransmission)**
		- If sender **does not receive an ACK before RTO (Retransmission Timeout)**:
		- ✔ It assumes the packet is lost
		- ✔ Retransmits the packet
		- ✔ Applies the **heaviest congestion control penalty**
		- **When RTO triggers?**
		- ACK never came
		- Connection is very slow or congested
		- **Penalty:**
	- ```
	  cwnd = 1 MSS        // restart slow start
	  ssthresh = cwnd/2
	  ```
		- **SDE takeaway:**
		- > Timeout = severe congestion → reset and start over slowly.
		- >
		- ### 2️⃣ **Retransmission via Fast Retransmit (3 Duplicate ACKs)**
		- If the sender receives **3 duplicate ACKs**, it means:
		- > “One packet is missing, but the network is still delivering later packets.”
		- >
		- Example:
		- Receiver repeatedly says “I still need packet X”.
		- ⚡ **Fast Retransmit action:**
		- Sender IMMEDIATELY resends the missing packet (no waiting for timeout).
		- **Penalty:**
	- ```
	  ssthresh = cwnd/2
	  cwnd = ssthresh        // reduce moderately
	  // skip slow start
	  ```
		- **SDE takeaway:**
		- > Duplicate ACKs = mild congestion → retransmit fast and reduce speed slightly.
		- >
		- ### 3️⃣ **Fast Recovery**
		- After fast retransmit:
		- TCP halves the congestion window (**cwnd**)
		- Avoids dropping to 1 MSS
		- Grows **linearly**, not exponentially
		- **Purpose:**
		- Recover quickly without killing throughput.
		- **SDE takeaway:**
		- > Skip slow start and recover smoothly after minor packet loss.
		- >
		- ### 4️⃣ **Selective Acknowledgement (SACK) Loss Recovery (Modern TCP)**
		- If SACK is enabled (recommended, default in Linux/modern stacks):
		- Receiver explicitly tells sender:
		- Which packets arrived
		- Which packets are missing
		- Sender can retransmit **only the missing segments**, not guess.
		- **Benefits:**
		- Faster recovery
		- Fewer retransmissions
		- Works well in high-loss networks
		- **SDE takeaway:**
		- > SACK makes loss recovery precise and efficient.
		- >
		- **“TCP detects loss using two mechanisms: RTO-based timeout and fast retransmit based on 3 duplicate ACKs. Timeout indicates heavy congestion and resets cwnd to 1 MSS. Duplicate ACKs indicate a single lost segment, so TCP performs fast retransmit and enters fast recovery, reducing cwnd moderately. With SACK, TCP can retransmit only missing segments, improving loss recovery efficiency.”**
	- Window Scaling
		- ### ✔ Why do we need window scaling?
		- TCP’s receive window (rwnd) field is **only 16 bits** → max value **65 KB**.
		- Back in the 1980s this was fine.
		- Today, on high-speed networks (1 Gbps, 10 Gbps), **65 KB max throughput is too small**.
		- So TCP introduces **Window Scaling**.
		- ### ⭐ What is window scaling?
		- **Window Scaling = multiplying rwnd by a scale factor to allow bigger windows.**
		- Used **only during TCP 3-way handshake**.
		- Both sides agree on a *scale factor*.
		- After that, TCP can advertise **much larger windows** (MBs instead of KBs).
		- This helps increase throughput on high-bandwidth networks.
		- ### 🎯 What does window scaling solve?
		- Without window scaling:
		- rwnd max = 65 KB
		- On fast networks, the sender becomes **under-utilized**
		- Throughput becomes low
		- With window scaling:
		- rwnd can reach up to **1 GB**
		- TCP can fully utilize fast links
		- ### 🔥 Simple SDE-friendly explanation
		- > Window Scaling removes the 65 KB limit by allowing TCP to advertise bigger receive windows, increasing throughput on high-latency or high-bandwidth networks.
		- >
-
- ### NAT (Network Address Translation)
  collapsed:: true
	- NAT is a technique used by routers to map **private IP addresses** inside a LAN to **one public IP address** on the internet.
	- It acts as a translator between:
	- **Local network (private IPs)**
	- **Public network (internet)**
	- This solves IPv4 address shortage and adds security + flexibility.
	- ### ⭐ **Why NAT Exists?**
	- ### 1️⃣ **Private devices cannot be routed on the internet**
	- Private IP ranges (192.168.x.x, 10.x.x.x, 172.16.x.x) are **not globally unique**, so they cannot be used on the internet.
	- ### 2️⃣ **Only one public IP is needed**
	- A company/home may have **1000 devices**, but they all share **ONE public IP** using NAT.
	- ### 3️⃣ **IPv4 address exhaustion**
	- NAT greatly slowed the need for IPv6.
	- ### 🧱 **Types of NAT**
	- ### 1. Static NAT (One-to-One)
	- One private IP ↔ one public IP.
	- Used when you want a local server accessible publicly (e.g., on-prem web server).
	- ```
	  10.0.0.10 ↔ 203.0.113.5
	  ```
	- ### 2. Dynamic NAT (Pool-based)
	- Router has a pool of multiple public IPs.
	- Private IP is mapped to any free public IP.
	- Rare in home networks, used in enterprises.
	- ### 3. PAT / NAT Overload (Most common)
	- Many private IPs share **ONE public IP**.
	- Differentiated using **port numbers**.
	- This is what home WiFi routers use.
	- ```
	  10.0.0.2 : 49123 → 203.0.113.5 : 49123
	  10.0.0.3 : 49124 → 203.0.113.5 : 49124
	  10.0.0.4 : 49125 → 203.0.113.5 : 49125
	  ```
	- Ports make sessions unique.
	- ### 🔥 **How NAT Works (PAT Example)**
	- When a device inside LAN sends traffic outside:
	- 1. **Router receives:**
	- ```
	  Source: 192.168.1.10 : 55001
	  ```
	- 2. Router *rewrites* source IP to public IP and assigns a unique port:
	- ```
	  203.0.113.5 : 40021
	  ```
	- 3. Router stores this in NAT table:
	- ```
	  203.0.113.5:40021 → 192.168.1.10:55001
	  ```
	- 4. Response from internet comes back to **203.0.113.5:40021**,
		- router uses NAT entry → forwards back to correct device.
	- ### 📦 **NAT Use Cases (Important for SDE/System Design)**
	- ### 1️⃣ Local Network → Public Internet (Main Use-Case)
	- All home/office devices share same public IP.
	- Router uses PAT to maintain thousands of connections using ports.
	- ### 2️⃣ Port Forwarding (Inbound Traffic to a Local Server)
	- Want to run a server inside LAN (e.g., game server, CCTV feed)?
	- You map:
	- ```
	  203.0.113.5:8080 → 192.168.1.50:80
	  ```
	- Used for:
	- Hosting private servers
	- Accessing devices remotely (NAS, CCTV)
	- SSHing into a machine behind NAT
	- ### 3️⃣ Load Balancer (NAT in AWS / GCP / on-prem)
	- Load balancers use NAT heavily:
	- Client → LB Public IP → LB NATs to backend private IP.
	- E.g.:
	- ```
	  203.0.113.5:443 → 10.0.0.5:443
	  → 10.0.0.6:443
	  → 10.0.0.7:443
	  ```
	- Every cloud LB uses NAT under the hood.
	- ### 4️⃣ Security / Firewall Use
	- NAT hides internal IPs, making topology invisible from the internet.
	- ### ⚙️ **NAT Table & Port Limits (Important Concept)**
	- A router typically uses **16-bit ports → max 65,536 ports**.
	- ### So:
	- With **one public IP**, router can create ~65k simultaneous NAT mappings.
	- Example NAT table entries:
	- ```
	  203.0.113.5:40001 → 192.168.1.10:50111
	  203.0.113.5:40002 → 192.168.1.20:50112
	  203.0.113.5:40003 → 192.168.1.30:50113
	  ...
	  (max ≈ 65k)
	  ```
	- ### ❗ Can the NAT table store only 65k records?
	- **Per public IP**, yes—because only 65k ports exist.
	- If more connections needed → routers use:
	- **Multiple public IPs**
	- **Port pools**
	- **Carrier-Grade NAT (CGN)** for ISPs
	- ### ✔️ **SDE-Level Summary**
	- NAT translates private IP ↔ public IP.
	- PAT allows many devices to share 1 public IP using ports.
	- NAT table stores mappings:
		- **publicIP:port → privateIP:port**
	- Max ~65k concurrent connections per public IP.
	- Used for internet access, port forwarding, load balancing, and security
-
- ### Extras
  collapsed:: true
	- ### 🧩 **1. What is the Loopback Interface (127.0.0.1)?**
	- Every computer has a special “virtual network card” called **loopback**.
	- It’s not a real physical network device.
	- It exists only so apps can talk to themselves as if over a network.
	- `127.0.0.1` (localhost) always points to this interface.
	- ### ⭐ Why does loopback exist?
	- For testing servers locally
	- For IPC (inter-process communication)
	- For apps talking to themselves safely
	- No routing required
	- Never leaves your computer
	- ### 🚫 Traffic to `127.0.0.1` NEVER goes out to internet or LAN.
	- Even if WiFi is turned off, localhost still works.
	- ### 🧩 **2. What is 0.0.0.0? Why does binding to it expose the app?**
	- `0.0.0.0` is NOT an IP you can connect to.
	- Its real meaning is:
	- > “Bind this server to ALL available IPs on this machine.”
	- >
	- Every device has multiple network interfaces:
	- Loopback → 127.0.0.1
	- WiFi → 192.168.1.x
	- Ethernet → 192.168.0.x or something else
	- Public IP → VM or cloud server
	- Docker bridge → 172.17.0.x
	- Kubernetes virtual network → 10.x.x.x
	- When your app binds to `0.0.0.0`, it listens on **all** of these.
	- So anyone who can reach ANY of your machine's network addresses can access the app.
	- ### 🧩 **3. Why localhost and 0.0.0.0 behave differently?**
	- ### Binding to localhost (127.0.0.1):
	- ```
	  App → Only accessible inside your machine
	  ```
	- ### Binding to 0.0.0.0:
	- ```
	  App → Accessible from:
	  - Your machine
	  - Other devices on LAN
	  - The internet (if firewall and routing allow)
	  ```
	- ### 🧩 **4. Does binding to 0.0.0.0 mean world can access you?**
	- NO.
	- Even if your app binds to 0.0.0.0, external access depends on:
	- ### ✔ Firewalls
	- ### ✔ Private vs Public IP
	- ### ✔ Port forwarding
	- ### ✔ NAT rules
	- ### ✔ Cloud provider security groups
	- ### ✔ VPN
	- Binding is only one piece of the puzzle.
	- ### 🧩 **5. How 0.0.0.0 works in Docker**
	- When you run a container:
	- ```
	  docker run -p 3000:3000 my-app
	  ```
	- What happens:
	- The container listens on **0.0.0.0:3000 inside container**
	- Docker maps it to host’s **0.0.0.0:3000**
	- So the app becomes accessible through host:
	- ```
	  localhost:3000
	  192.168.x.x:3000
	  public-ip:3000 (if public VM)
	  ```
	- If instead the app listens on **localhost** inside Docker, it becomes invisible outside the container.
	- ### 🧩 **6. How 0.0.0.0 works in Kubernetes (important)**
	- Your Pod might run an app on:
	- ```
	  app.listen(3000, "0.0.0.0")
	  ```
	- Meaning the app listens inside the pod network.
	- Then a **Service** exposes it (ClusterIP/NodePort).
	- Thus:
	- ### Pod listens on 0.0.0.0 → K8s network can access it
	- ### NodePort → exposes to Node
	- ### Ingress → exposes to world
	- ### 🧩 **7. Why apps don’t run on 80 or 443 (privileged ports)**
	- On Linux:
	- Ports <1024 are privileged
	- Only root can bind to port 80 (HTTP) or 443 (HTTPS)
	- Developers run apps as **non-root** for safety, so apps run on:
	- ```
	  3000
	  4000
	  5000
	  8080
	  ```
	- Then NGINX/Render/Ingress handles:
	- ```
	  :443 (HTTPS) → forwards to app internal port
	  ```
	- ### 🧩 **8. Deep Mental Model for All Networking**
	- Here’s the complete picture:
	- ### ✔ 127.0.0.1 = Only inside your machine
	- ### ✔ 0.0.0.0 = Listen everywhere
	- ### ✔ 192.168.x.x = Local network only (private)
	- ### ✔ 10.x.x.x = ISP or K8s private network
	- ### ✔ Public IP = reachable from anywhere (if firewall allows)
	- ### ✔ Port forwarding = router chooses where to send traffic
	- ### ✔ Ports <1024 = need root to bind
	- ### ✔ Ports >1024 = free to use
	- ### ⭐ **1. Your app does NOT run on port 443.
	- But SOMETHING ELSE does.**
	- On any machine that serves HTTPS, you will always have **a reverse proxy or web server** running on port 443.
	- ### The common ones:
	- **NGINX**
	- **Apache**
	- **Caddy**
	- **Envoy**
	- **Traefik**
	- **Render’s load balancer**
	- **AWS ALB / ELB**
	- **Kubernetes Ingress Controller**
	- These are “root-level” processes allowed to bind port **443**.
	- They run as system-level services, not user apps.
	- ### ⭐ **2. Port 443 is doing TLS/SSL termination**
	- Port 443 performs:
	- ### ✔ TLS handshake
	- ### ✔ Certificate verification
	- ### ✔ Encryption/decryption
	- ### ✔ Connection security
	- Your app does **not** want to deal with encryption — it slows things down and is complicated.
	- So 443 is used for **security**, not for your application logic.
	- ### ⭐ **3. After handling HTTPS, port 443 forwards traffic internally**
	- Once the proxy on port 443 finishes the secure handshake, it forwards the request to your app running on a normal port.
	- Example:
	- ```
	  Client → Port 443 (NGINX SSL) → App on 3000
	  ```
	- or in Kubernetes:
	- ```
	  Client → Ingress (443) → Service (80) → Pod (3000)
	  ```
	- or in Render:
	- ```
	  Client → Render Router (443) → Your internal port 10000
	  ```
	- ### ⭐ **4. So what's actually running on port 443?**
	- ### ✔ A web server or reverse proxy — NOT your application.
	- This system process:
	- runs as **root**
	- binds to privileged ports (80, 443)
	- handles HTTPS
	- routes requests to the correct internal app
	- Your user app stays safe on **unprivileged ports** like 3000, 4000, 5000.
	- ### ⭐ **5. Why apps don’t bind to 443 directly**
	- Because:
	- 1. It requires root → security risk
	- 2. Harder to rotate SSL certificates
	- 3. No load balancing
	- 4. No routing
	- 5. No rate limiting or caching
	- 6. Hard to manage at scale
	- Reverse proxies solve all these problems.
	- ### ⭐ Example That Explains Everything Perfectly
	- Let’s say your Node.js app runs on port 3000.
	- ```
	  app.listen(3000);
	  ```
	- You want users to visit:
	- ```
	  https://mywebsite.com
	  ```
	- Here’s what happens:
	- 1. Browser connects to port **443**
	- 2. NGINX (running as root) accepts the HTTPS request
	- 3. NGINX decrypts the traffic
	- 4. NGINX forwards decrypted HTTP traffic to your app:
	- ```
	  /etc/nginx/sites-enabled/default
	  proxy_pass http://localhost:3000;
	  ```
	- Your app never touches 443.
	- ### ⭐ So what is port 443 doing?
	- **Port 443 is the secure entry point of the internet.
	- It handles encryption and routing.
	- It is used by proxies, not user apps.**
	- Your apps only handle business logic on internal ports.
	- ### 🟦 **1. The normal (raw) way: You specify ports**
	- If you do **NOT** use Nginx or Caddy, and you have apps like:
	- | App | Port |
	- | --- | --- |
	- | React app 1 | 3000 |
	- | React app 2 | 3001 |
	- | Node API | 5000 |
	- | Admin panel | 4000 |
	- Then users must access them like:
	- ```
	  http://public-ip:3000
	  https://public-ip:3000
	  http://public-ip:3001
	  http://public-ip:5000
	  ```
	- This works, but:
	- It is ugly
	- Users must remember ports
	- HTTPS certificates for each port are a headache
	- You cannot use a domain without exposing ports
	- Browsers block some non-standard ports
	- ### 🟩 **2. The professional way: You use NGINX / Caddy as a reverse proxy**
	- This is how **Render**, **Vercel**, **Netlify**, **Heroku**, **AWS**, **DigitalOcean**, **Kubernetes** do it.
	- You create ONE entry point:
	- ```
	  Port 80  → HTTP
	  Port 443 → HTTPS
	  ```
	- And NGINX routes requests to internal services on ports 3000, 3001, 4000, etc.
	- ### ⭐ HOW THIS WORKS
	- ### ✔ NGINX listens on:
	- ```
	  :80
	  :443
	  ```
	- ### ✔ Your apps still listen on:
	- ```
	  :3000
	  :3001
	  :4000
	  :5000
	  ```
	- ### ✔ NGINX forwards based on domain or path
	- ### 🟩 **3. How NGINX chooses the correct service?**
	- There are TWO common ways:
	- ### ⭐ **Method 1: Multiple domains → Multiple apps**
	- Example:
	- ```
	  app1.example.com → port 3000
	  app2.example.com → port 3001
	  api.example.com  → port 5000
	  admin.example.com → port 4000
	  ```
	- NGINX config:
	- ```
	  server {
	  server_name app1.example.com;
	  location / {
	  proxy_pass http://localhost:3000;
	  }
	  }
	  server {
	  server_name app2.example.com;
	  location / {
	  proxy_pass http://localhost:3001;
	  }
	  }
	  server {
	  server_name api.example.com;
	  location / {
	  proxy_pass http://localhost:5000;
	  }
	  }
	  ```
	- ### ✔ Users use HTTPS without ports
	- ### ✔ All traffic enters on port 443
	- ### ✔ NGINX routes to correct backend app internally
	- ### ⭐ **Method 2: One domain → Multiple apps (using paths)**
	- Example:
	- ```
	  https://example.com/app1 → 3000
	  https://example.com/app2 → 3001
	  https://example.com/api  → 5000
	  ```
	- NGINX:
	- ```
	  location /app1/ {
	  proxy_pass http://localhost:3000/;
	  }
	  location /app2/ {
	  proxy_pass http://localhost:3001/;
	  }
	  location /api/ {
	  proxy_pass http://localhost:5000/;
	  }
	  ```
	- ### Again:
	- ✔ Only 443 is used externally
	- ✔ Internal ports remain hidden
	- ✔ URL becomes clean
	- ### 🟥 **4. So what is the use of port 80?**
	- Port **80** = HTTP
	- Port **443** = HTTPS
	- What usually happens:
	- Port **80** receives HTTP requests
	- NGINX automatically redirects them to **HTTPS (port 443)**
	- Example redirect:
	- ```
	  http://example.com → https://example.com
	  ```
	- This is the standard web behavior.
	- ### 🟦 **Diagram: Your exact understanding (YES this is correct)**
	- ```
	  Client (HTTPS:443)
	  ↓
	  Cloud LoadBalancer (listens on 443)
	  ↓
	  Forwards to Node (VM)
	  ↓
	  Ingress Controller (NGINX)
	  - listens on 80/443
	  - reverse proxy
	  ↓
	  Service (internal LB)
	  - load balances to pods
	  ↓
	  Pod (container)
	  - app runs on port 8080 or others
	  ```
-
- ### Domains knowledge
  collapsed:: true
	- ### 🟦 **1. What is a Domain?**
	- ![https://media.geeksforgeeks.org/wp-content/uploads/20200822065029/UntitledDiagram.png?utm_source=chatgpt.com](https://media.geeksforgeeks.org/wp-content/uploads/20200822065029/UntitledDiagram.png?utm_source=chatgpt.com)
	- ![https://performanceconnectivity.com/wp-content/uploads/DNS-Explained.jpg?utm_source=chatgpt.com](https://performanceconnectivity.com/wp-content/uploads/DNS-Explained.jpg?utm_source=chatgpt.com)
	- 4
	- ![https://assets.bytebytego.com/diagrams/0176-dns-look-up.png?utm_source=chatgpt.com](https://assets.bytebytego.com/diagrams/0176-dns-look-up.png?utm_source=chatgpt.com)
	- A **domain** is simply a **human-readable name** that points to an IP address.
	- Example:
	- ```
	  amazon.com → 205.251.242.103  (IP)
	  google.com → 142.250.195.46   (IP)
	  yourapp.dev → 34.102.136.180  (IP)
	  ```
	- ### Why domains exist?
	- Because humans cannot remember:
	- ```
	  205.251.242.103
	  ```
	- Domains are easier.
	- ### 🟩 **2. What is a Subdomain?**
	- A **subdomain** is any name *before* the main domain.
	- Examples:
	- ```
	  api.amazon.com
	  seller.amazon.com
	  m.facebook.com
	  cdn.cloudflare.com
	  auth.myapp.dev
	  ```
	- Here:
	- `amazon.com` = domain
	- `api.amazon.com` = subdomain
	- `m.facebook.com` = subdomain
	- `auth.myapp.dev` = subdomain
	- Subdomains allow you to separate services:
	- ```
	  api.myapp.com → API backend
	  app.myapp.com → frontend
	  admin.myapp.com → dashboard
	  ```
	- ### 🟧 **3. What are `.com`, `.co`, `.dev`, `.in`, `.org` etc?**
	- These are **TLDs (Top-Level Domains)**.
	- They are the **last part** of a domain.
	- Examples:
	- | TLD | Meaning |
	- | --- | --- |
	- | `.com` | Commercial (most common) |
	- | `.dev` | For developers (Google-owned) |
	- | `.org` | Organizations |
	- | `.net` | Networks (ISPs originally) |
	- | `.co` | Originally for Colombia, now used globally |
	- | `.in` | India |
	- | `.io` | British Indian Territory; popular in tech |
	- They help categorize domains.
	- ### 🟥 **4. Are domains related to ports?**
	- ### ❌ **NO. Domains have ZERO relation to ports.**
	- They are completely separate concepts.
	- ### ⭐ How domains and ports actually work together:
	- When you type:
	- ```
	  https://amazon.com
	  ```
	- Your browser performs:
	- 1. **DNS Lookup**
	- ```
	  amazon.com → 205.251.242.103
	  ```
	- 2. **Choose default port**
	- HTTPS → port **443**
	- 3. **Make request to:**
	- ```
	  205.251.242.103:443
	  ```
	- If you manually specify a port:
	- ```
	  https://example.com:4000
	  ```
	- Then browser uses port **4000**, not 443.
	- ### ✔️ Domain = name
	- ### ✔️ Port = door
	- They are separate.
	- ### 🟨 **5. Visual Explanation (Super Simple)**
	- ```
	  https://api.myapp.com:443/search
	  |        |      |
	  domain   port   path
	  ```
	- Breakdown:
	- **Domain** → where to go
	- **Port** → which door to knock on
	- **Path** → what resource to request
	- ### 🟩 **6. Why `.com` vs `.dev` vs `.co` matters?**
	- They are **managed by domain registries**.
	- You buy:
	- `yourname.com` from Verisign
	- `yourproject.dev` from Google
	- `startup.co` from GoDaddy, etc.
	- It only affects:
	- Availability
	- Price
	- Reputation
	- Does **not** affect:
	- Ports
	- Networking
	- Kubernetes
	- Backend routing
	- ### 🟦 **7. Full Flow Example (Everything connected)**
	- User types:
	- ```
	  https://api.myapp.dev/users
	  ```
	- Behind the scenes:
	- ### Step 1: DNS
	- ```
	  api.myapp.dev → 34.122.184.102
	  ```
	- ### Step 2: Browser chooses port
	- HTTPS → **443**
	- ### Step 3: Request goes to LoadBalancer
	- ```
	  34.122.184.102:443
	  ```
	- ### Step 4: LoadBalancer → Ingress → Service → Pod
	- ```
	  LB (443) → Ingress (443) → service-api → pod (8080)
	  ```
	- See?
	- Domain & TLDs are **not involved** in port selection.
	- ### 🟧 **8. FINAL SUPER-CLEAR SUMMARY**
	- ### ✔️ **Domain = human-friendly name for an IP**
	- ### ✔️ **Subdomain = prefix that separates services**
	- ### ✔️ **`.com`, `.dev`, `.co` = top-level domain categories**
	- ### ✔️ **None of these have any relation to port numbers**
	- ### ✔️ **Ports are chosen by services & protocols, not domains**
- collapsed:: true
- ### TLS / SSL certificate
  collapsed:: true
	- ### Establishment
		- 1. client send a req with client_random_number to server for getting public key and server_random_number and certificate
		- 2. after receiving it validates the certificate signature using public key ( issuer) and public key (server) if validation is sucessfull then next step.
		- 3. generates a random_secret and encrypts it using server public key and send it back to server
		- 4. server decrypts the random_secret using its private key now they both have the shared random_secret
		- 5. In TLS(≤1.2) session key is derived by ( random_secret , client_random_number ,server_random_number )
		- 6. This session key is then used to encrypt data and decrypt data
-
- ### Protocols (pending and important → HTTP, HTTPS, FTP,SMTP, etc.)
  collapsed:: true
-
- ### HTTP Status Codes
  collapsed:: true
	- 1. 1xx (Informational):
	- o Indicate that the request is being processed.
	- Example: 100 Continue.
	- 2. 2xx (Success):
	- o Indicate that the request was successfully processed.
	- Example: 200 OK.
	- 3. 3xx (Redirection):
	- o Indicate that the client must take additional steps to complete the request.
	- Example: 301 Moved Permanently.
	- 4. 4xx (Client Errors):
	- o Indicate an error on the client’s part.
	- Example: 404 Not Found.
	- 5. 5xx (Server Errors):
	- o Indicate an error on the server’s part.
	- Example: 500 Internal Server Error.
-
- ### SDE’s Networking Cheat Sheet
  collapsed:: true
	- ### 🧠 Networking Cheat Sheet for SDEs (Updated)
	- ### 1️⃣ **IP Address**
		- Unique identifier for a device on a network.
		- IPv4: 32 bits → 4 numbers (0–255).
		- Example: `192.168.1.10`
	- ### 2️⃣ **Network**
		- A group of devices that can communicate directly.
		- Defined by the **network part** of the IP (based on subnet mask).
		- Example: `192.168.1.0/24` → devices 192.168.1.1 → 192.168.1.254
	- ### 3️⃣ **Host**
		- Any **device inside a network**.
		- Example: a computer, phone, or server in `192.168.1.0/24`.
	- ### 4️⃣ **Network Address**
		- The **first IP of a subnet**; identifies the subnet itself.
		- Cannot be assigned to a host.
		- Example: `192.168.1.0/24`
	- ### 5️⃣ **Broadcast Address**
		- The **last IP of a subnet**; used to send messages to **all devices** in the subnet.
		- Example: `192.168.1.255/24`
	- ### 6️⃣ **Subnet**
		- A **logical division of a larger network**.
		- Purpose: organize devices and control communication.
		- Example: `192.168.1.0/24` is a subnet of `192.168.0.0/16`.
	- ### 7️⃣ **Subnet Mask**
		- 32-bit number separating **network part** and **host part** of an IP.
		- 1s = network bits, 0s = host bits
		- Example: `255.255.255.0` → /24
	- ### 8️⃣ **/24 (CIDR Notation)**
		- Short way to represent subnet mask.
		- `/24` = first 24 bits are network, 8 bits are host
		- Equivalent to `255.255.255.0`
	- ### 9️⃣ **Gateway**
		- Device (usually a router) connecting your **subnet to other subnets/networks**.
		- Needed **only when communicating outside your subnet**.
		- Example: `192.168.1.1` as default gateway for `192.168.1.0/24`
			- **Key Point:**
		- Devices on the **same subnet** → no gateway needed
		- Devices on **different subnets** (even in the same larger network) → gateway needed
	- ### 🔹 Quick Reference Table
		- | Term | What it does | Example |
		- | --- | --- | --- |
		- | IP Address | Identifies device | 192.168.1.10 |
		- | Network | Group of devices | 192.168.1.0/24 |
		- | Host | Device inside network | 192.168.1.5 |
		- | Network Address | Identifies subnet | 192.168.1.0 |
		- | Broadcast Address | Message all hosts | 192.168.1.255 |
		- | Subnet | Division of network | 192.168.1.0/24 |
		- | Subnet Mask | Separates network/host | 255.255.255.0 |
		- | /24 | CIDR shorthand for subnet mask | /24 = 255.255.255.0 |
		- | Gateway | Connects to other networks | 192.168.1.1 |