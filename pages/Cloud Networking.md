- Ports, Interfaces
  collapsed:: true
	- ## 1. Core Mental Model
	  
	  ```
	  Device (VM / Router / Load Balancer)
	        ↓
	  Uses Ports (Interfaces)
	        ↓
	  Ports own Fixed IPs
	        ↓
	  Each Fixed IP belongs to a Subnet
	        ↓
	  Traffic enters/exits through Ports
	  ```
	  
	  ---
	- # 2. What is a Port? 
	  
	  A port (network interface) is the endpoint through which a device communicates with a network.
	  
	  Think of it as a **socket (hole)** on a device.
	  
	  ```
	  Computer
	  
	  +---------------------+
	  |                     |
	  |   [ Ethernet Port ] |  ← Port / Interface
	  |                     |
	  +---------------------+
	  ```
	  
	  A **port is NOT a cable.**
	  
	  The cable connects two ports.
	  
	  ```
	  Computer                    Router
	  
	  [ Port ] ===== Cable ===== [ Port ]
	  ```
	- In OpenStack:
	  
	  ```
	  Port ≈ Virtual Network Interface (NIC)
	  ```
	  
	  A port is a virtual connection point through which a device sends and receives network traffic.
	  
	  A port can belong to:
	- VM
	- Router
	- Load Balancer
	- DHCP service
	- Firewall
	- etc.
	- Security groups are attached on the port.
	- ```
	  VM
	   │
	   ▼
	  Port
	  ├── Fixed IP
	  ├── MAC
	  ├── Security Groups
	  └── Network
	  ```
	- Example:
	  
	  ```
	  {
	  "id": "port-123",
	  "device_owner": "network:router_interface",
	  "device_id": "router-1",
	  "fixed_ips": [
	    {
	      "subnet_id": "subnet-1",
	      "ip_address": "10.0.0.1"
	    }
	  ]
	  }
	  ```
	  
	  ---
	- # 3. What is an Interface?
	  
	  An interface is:
	  
	  ```
	  A network connection point on a device.
	  ```
	  
	  Linux examples:
	  
	  ```
	  eth0
	  eth1
	  wlan0
	  ```
	  
	  In OpenStack:
	  
	  ```
	  Port ≈ Interface
	  ```
	  
	  OpenStack represents virtual interfaces using Neutron Ports.
	- Interfaces send/receive packets.
	- ---
	- # 4. Why can a Port have Multiple Fixed IPs?
		- ### Key Idea
		  
		  A **Neutron Port** (Virtual Network Interface) owns **Fixed IPs**.
		  
		  Normally, a port has **one Fixed IP**.
		  
		  However, a port **can** have multiple Fixed IPs because a network interface can legally have multiple IP addresses.
		  
		  Example:
		  
		  ```
		  "fixed_ips": [
		  {
		    "subnet_id": "subnet-1",
		    "ip_address": "10.0.0.5"
		  },
		  {
		    "subnet_id": "subnet-2",
		    "ip_address": "192.168.1.10"
		  }
		  ]
		  ```
		  
		  This means:
		  
		  ```
		  One Port (Virtual Interface)
		  
		  ├── Fixed IP : 10.0.0.5
		  └── Fixed IP : 192.168.1.10
		  ```
		  
		  The port owns both IP addresses.
		  
		  ---
		- ### ⚠️ Important
		  
		  Although OpenStack supports multiple Fixed IPs on a port, **router interfaces are usually created with one port per connected subnet**.
		- OpenStack intentionally creates one router interface per subnet because it makes networking resources (DHCP, routing, security, tenant isolation, lifecycle management, etc.) much simpler and less error-prone to manage.
		- So the common architecture is:
		  
		  ```
		  Router
		  
		  ├── Port A
		  │     └── 10.0.0.1 (Subnet A)
		  │
		  ├── Port B
		  │     └── 192.168.1.1 (Subnet B)
		  │
		  └── Port C
		      └── 172.16.0.1 (Subnet C)
		  ```
		  
		  instead of
		  
		  ```
		  Router
		  
		  └── One Port
		      ├── 10.0.0.1
		      ├── 192.168.1.1
		      └── 172.16.0.1
		  ```
		  
		  This is mainly a **design choice** that keeps routing, DHCP, security groups, and tenant isolation simple.
	- ---
- fixed Ip, floating Ip, Reserved Ip
  collapsed:: true
	- # 1. What is a Fixed IP?
	  
	  A fixed IP is:
	  
	  ```
	  Private/internal IP assigned to a port/interface.
	  ```
	  
	  Example:
	  
	  ```
	  10.0.0.5
	  192.168.1.10
	  ```
	  
	  Characteristics:
	- Internal/private network IP
	- Assigned directly to port
	- Usually not internet reachable
	- Belongs to a subnet
	  
	  ---
	- # 2. Who Handles Traffic Logic?
	  
	  The networking logic is handled by the component itself:
	  
	  | Component | Responsibility |
	  | ---- | ---- | ---- |
	  | Router | Routes packets between subnets |
	  | Load Balancer | Distributes traffic |
	  | VM | Sends/receives application traffic |
	  | Firewall | Filters traffic |
	  
	  The port/interface only acts as:
	  
	  ```
	  Door/connection point for network traffic
	  ```
	  
	  ---
	- # 3. Router + Port Relationship
	  
	  Routers connect subnets using ports/interfaces.
	  
	  Example:
	  
	  ```
	  Subnet A: 10.0.0.0/24
	  Subnet B: 192.168.1.0/24
	  ```
	  
	  Router:
	  
	  ```
	  Port A:
	  10.0.0.1
	  
	  Port B:
	  192.168.1.1
	  ```
	  
	  Now router can route traffic:
	  
	  ```
	  10.0.0.x <--> 192.168.1.x
	  ```
	  
	  Important:
	  
	  ```
	  Router performs routing.
	  Ports only provide connectivity/interfaces.
	  A router owns multiple ports/interfaces.
	  One router-interface port exists per subnet.
	  ```
	- Router Internally looks like
		- Router R1
		     ├── Port1 -> 10.0.0.1 -> subnet A
		     ├── Port2 -> 192.168.1.1 -> subnet B
		     └── Port3 -> 172.16.0.1 -> subnet C
	- ---
	- # 4. Floating IP vs Fixed IP
	- ## Fixed IP
	  
	  ```
	  Internal/private IP on port
	  ```
	  
	  Example:
	  
	  ```
	  10.0.0.5
	  ```
	  
	  ---
	- ## Floating IP
	  
	  ```
	  Public IP mapped to a fixed IP using NAT
	  ```
	  
	  Example:
	  
	  ```
	  Floating IP: 203.0.113.25
	  Fixed IP:    10.0.0.5
	  ```
	  
	  Mapping:
	  
	  ```
	  203.0.113.25 -> 10.0.0.5
	  ```
	  
	  ---
	- # 5. Why Floating IP Links to Fixed IP
	  
	  Because OpenStack needs to know:
	  
	  ```
	  Which internal interface should receive internet traffic?
	  ```
	  
	  A VM may have:
	- multiple ports
	- multiple interfaces
	- multiple fixed IPs
	  
	  So floating IP maps to ONE specific fixed IP.
	  
	  Example:
	  
	  ```
	  203.0.113.25 -> 10.0.0.5
	  ```
	  
	  ---
	- # 6. Packet Flow
	  
	  ```
	  Internet
	   ↓
	  Floating/Public IP
	  203.0.113.25
	   ↓
	  OpenStack Router (NAT)
	   ↓
	  Fixed IP
	  10.0.0.5
	   ↓
	  Port/Interface
	   ↓
	  VM
	  ```
	  
	  ---
	- # 7. device_owner in Ports
	  
	  `device_owner` tells which component owns the port.
	  
	  Examples:
	  
	  | device_owner | Meaning |
	  | ---- | ---- | ---- |
	  | network:router_interface | Router interface |
	  | network:ha_router_replicated_interface | HA router |
	  | compute:nova | VM port |
	  | network:dhcp | DHCP service |
	  
	  ---
	- # 8. Router Interface Discovery Logic
	  
	  To find router connected to subnet:
	- Fetch ports
	- Find ports whose:
		- `fixed_ips[].subnet_id == targetSubnet`
		- `device_owner == router interface`
	- Extract:
	  
	  ```
	  device_id = router ID
	  ```
	  
	  ---
	- # 9. Why `.1` is Often Preferred
	  
	  Many environments use:
	  
	  ```
	  x.x.x.1
	  ```
	  
	  as gateway IP.
	  
	  Example:
	  
	  ```
	  10.0.0.1
	  ```
	  
	  So router interface selection often prefers `.1`.
	  
	  But best practice:
	  
	  ```
	  Use subnet.gateway_ip instead of checking endsWith('.1')
	  ```
	  
	  because `.1` is convention, not guarantee.
	- # 10. Reserved Ip
		- A reserved IP is most commonly an internal/private IP reserved for infrastructure purposes.
		  
		  Example from your subnet:
		  
		  ```
		  192.168.0.1 -> router gateway
		  192.168.0.2 -> distributed routing service
		  ```
		  
		  These are:
		  
		  ```
		  internal reserved IPs
		  ```
		  
		  used by OpenStack networking components.
		  
		  ---
		  
		  But technically:
		  
		  ```
		  "reserved" describes PURPOSE
		  not whether IP is private/public
		  ```
		  
		  Meaning:
		- a private IP can be reserved
		- a public IP can also be reserved
- Public Ip VS Floating Ip
  collapsed:: true
	- # 1️⃣ Public IP
	  
	  A public IP means:
	  
	  ```
	  Any IP address reachable from the internet
	  ```
	  
	  Example:
	  
	  ```
	  8.8.8.8
	  142.250.x.x
	  203.0.113.25
	  ```
	  
	  Characteristics:
	- internet routable
	- globally unique
	- reachable from outside world
	  
	  That’s all.
	  
	  ---
	- # 2️⃣ Floating IP
	  
	  A floating IP is:
	  
	  ```
	  A cloud-managed public IP dynamically mapped to a private/fixed IP
	  ```
	  
	  Example:
	  
	  ```
	  Floating IP: 203.0.113.25
	  Fixed IP:    10.0.0.5
	  ```
	  
	  Mapping:
	  
	  ```
	  203.0.113.25
	        ↓ NAT
	  10.0.0.5
	  ```
	  
	  ---
	- # Key difference
	- ## Public IP
	  
	  Generic networking concept.
	  ---
	- ## Floating IP
	  
	  Cloud/OpenStack concept.
	  
	  It is:
	  
	  ```
	  a movable public IP managed through NAT mapping
	  ```
	  
	  ---
	- # Why called “floating”?
	  
	  Because it can move between instances/VMs.
	  
	  Example:
	  
	  Initially:
	  
	  ```
	  203.0.113.25 -> VM1
	  ```
	  
	  Later:
	  
	  ```
	  203.0.113.25 -> VM2
	  ```
	  
	  Same public IP “floats” between machines.
	  
	  ---
	- # In OpenStack
	  
	  Floating IP usually:
	- belongs to external/public network
	- mapped to fixed IP
	- handled by router NAT
	- not directly configured inside VM
	  
	  ---
	- # Important understanding
	  
	  Inside VM you usually see:
	  
	  ```
	  10.0.0.5
	  ```
	  
	  NOT:
	  
	  ```
	  203.0.113.25
	  ```
	  
	  because floating IP exists at networking/router layer.
	  
	  ---
	- # Packet flow
	  
	  ```
	  Internet
	   ↓
	  Floating/Public IP
	  203.0.113.25
	   ↓
	  OpenStack Router NAT
	   ↓
	  Fixed IP
	  10.0.0.5
	   ↓
	  VM
	  ```
	  
	  ---
	- # VERY IMPORTANT
	- ## Every floating IP is usually public
	  
	  TRUE.
	  
	  ---
	- ## But not every public IP is floating
	  
	  Also TRUE.
	  
	  Example:
	  
	  A physical server directly configured with internet IP:
	  
	  ```
	  203.x.x.x
	  ```
	  
	  This is:
	  
	  ```
	  public IP
	  ```
	  
	  but NOT floating IP.
	  
	  Because no cloud NAT mapping exists.
	  
	  ---
	- # Real-world analogy
	- ## Public IP
	  
	  ```
	  Permanent phone number
	  ```
	  
	  ---
	- ## Floating IP
	  
	  ```
	  Call forwarding number
	  ```
	  
	  Today forwarded to:
	  
	  ```
	  Person A
	  ```
	  
	  Tomorrow forwarded to:
	  
	  ```
	  Person B
	  ```
	  
	  Same number moves dynamically.
- Firewall VS Security Groups
  collapsed:: true
	- ## Firewall
	- A firewall is a centralized network security system that filters and controls traffic between networks/subnets.
	- It decides whether packets should be **allowed or blocked** based on:
		- source IP
		- destination IP
		- protocol (TCP/UDP)
		- port numbers
	- Firewalls usually protect:
		- entire networks
		- subnets
		- internet boundaries
	- Firewalls can also perform:
		- NAT
		- VPN
		- routing
		- IDS/IPS
		- deep packet inspection
		- network segmentation
	- ### Example
	  
	  ```
	  Internet
	   ↓
	  Firewall
	   ↓
	  Private VPC/Subnets
	  ```
	  
	  ---
	- ## Security Groups
	- Security Groups are cloud-managed mini firewalls.
	- They are attached to:
		- VM instances
		- network interfaces/ports
	- They mainly perform:
		- ingress/egress filtering
		- protocol filtering
		- TCP/UDP port filtering
		- source IP filtering
	- They usually protect:
		- a single VM
		- a single interface/port
	- ### Example
	  
	  ```
	  VM Interface
	   ↓
	  Security Group
	   ↓
	  Allow:
	  - SSH 22
	  - HTTPS 443
	  ```
	  
	  ---
	- # Key Difference
	  
	  | Firewall | Security Group |
	  | ---- | ---- | ---- |
	  | Centralized network security system | Lightweight cloud firewall |
	  | Protects networks/subnets | Protects single VM/interface |
	  | Supports NAT, VPN, routing, IDS/IPS | Mostly packet filtering |
	  | Works at broader network level | Works at instance/interface level |
	  | Can inspect and route traffic | Mainly allow/deny rules |
	  
	  ---
	- # Important Understanding
	  
	  ```
	  Security Group = simplified cloud firewall
	  Firewall = broader and more powerful network security/routing system
	  ```
- OpenStack ↔ AWS
  collapsed:: true
	- | **OpenStack** | **AWS Equivalent** | **Purpose / Usage** |
	  | ---- | ---- | ---- |
	  | **Nova** | **EC2** | Creates and manages Virtual Machines. |
	  | **Flavor** | **EC2 Instance Type** | Defines CPU, RAM and other compute resources for a VM. |
	  | **Image (Glance)** | **AMI (Amazon Machine Image)** | OS template used to launch a VM/EC2. |
	  | **Neutron Network** | **VPC Network** | Virtual network where instances communicate. |
	  | **Neutron Subnet** | **VPC Subnet** | IP range inside a network. |
	  | **Neutron Port** | **ENI (Elastic Network Interface)** | Virtual Network Interface attached to a VM. Owns MAC, IPs, Security Groups. |
	  | **Fixed IP** | **Private IP** | Internal IP used for communication inside the cloud network. |
	  | **Floating IP** | **Elastic IP / Public IP** | Publicly reachable IP mapped to a private IP. |
	  | **Router** | **VPC Router + Route Tables** | Routes traffic between subnets and external networks. |
	  | **External Network** | **Internet Gateway (IGW)** | Connects private cloud networks to the Internet. |
	  | **Security Group** | **Security Group** | Stateful virtual firewall attached to interfaces. |
	  | **Network ACL** | **Network ACL** | Stateless subnet-level firewall (AWS has this separately). |
	  | **Cinder Volume** | **EBS Volume** | Persistent block storage attached to VMs. |
	  | **Swift** | **S3** | Object storage for files, backups, images, logs, etc. |
	  | **Keystone** | **IAM** | Identity, authentication and authorization. |
	  | **Horizon** | **AWS Console** | Web UI to manage cloud resources. |
	  | **Heat** | **CloudFormation** | Infrastructure as Code (IaC). |
	  | **Telemetry (Ceilometer)** | **CloudWatch** | Metrics, monitoring and alarms. |
	  | **Octavia** | **Elastic Load Balancer (ALB/NLB)** | Distributes incoming traffic across instances. |
	  | **Designate** | **Route 53** | Managed DNS service. |