- Ports, Interfaces, fixed Ip, floating Ip, Reserved Ip
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
	  
	  In OpenStack:
	  
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
	  
	  Example:
	  
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
	  
	  ---
	- # 4. What is a Fixed IP?
	  
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
	- # 5. Why can a Port have Multiple Fixed IPs?
	  
	  A single interface can legally own multiple IP addresses.
	  
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
	  
	  Meaning:
	  
	  ```
	  This interface owns:
	  - 10.0.0.5 in subnet-1
	  - 192.168.1.10 in subnet-2
	  ```
	  
	  NOT:
	  
	  ```
	  Port itself is the subnet
	  ```
	  
	  ---
	- # 6. Who Handles Traffic Logic?
	  
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
	- # 7. Router + Port Relationship
	  
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
	  ```
	  
	  ---
	- # 8. Floating IP vs Fixed IP
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
	- # 9. Why Floating IP Links to Fixed IP
	  
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
	- # 10. Packet Flow
	  
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
	- # 11. device_owner in Ports
	  
	  `device_owner` tells which component owns the port.
	  
	  Examples:
	  
	  | device_owner | Meaning |
	  | ---- | ---- | ---- |
	  | network:router_interface | Router interface |
	  | network:ha_router_replicated_interface | HA router |
	  | compute:nova | VM port |
	  | network:dhcp | DHCP service |
	  
	  ---
	- # 12. Router Interface Discovery Logic
	  
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
	- # 13. Why `.1` is Often Preferred
	  
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
	- # 14. Reserved Ip
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