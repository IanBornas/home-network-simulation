#  SOHO Home Network Simulation (Cisco Packet Tracer)

A simulated Small Office / Home Office (SOHO) network built in **Cisco Packet Tracer**[cite: 6]. This project demonstrates dual-subnet IPv4 addressing, Layer 2 switching, Layer 3 inter-subnet routing, automated DHCP IP allocation, and end-to-end ICMP connectivity verification.

![Network Topology](screenshots/01-topology-overview.PNG)

---

##  Architecture & Hardware Used
* **Router:** Cisco 1941 ISR (`PLDTWIFI`) — Handles WAN/LAN routing and DHCP server leases[cite: 1, 6].
* **Switch:** Cisco Catalyst 2960-24TT (`SW1`) — Performs Layer 2 frame forwarding across VLAN 1[cite: 2, 6].
* **Modem / ISP:** DSL Modem connected to ISP Cloud (`192.168.10.0/24` WAN segment)[cite: 1, 6].
* **Endpoints:** 3 client machines (`Ian's PC`, `Mom's Laptop`, `Mark's PC`) receiving dynamic IPs via DHCP[cite: 6].

---

##  IP Addressing & Interface Table

| Device | Interface | IPv4 Address | Subnet Mask | Default Gateway | Role |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **PLDTWIFI (1941)** | `GigabitEthernet0/0` | `192.168.10.1` | `255.255.255.0` | N/A | WAN Uplink to DSL Modem[cite: 1, 6] |
| **PLDTWIFI (1941)** | `GigabitEthernet0/1` | `192.168.1.1` | `255.255.255.0` | N/A | LAN Default Gateway[cite: 1] |
| **SW1 (2960)** | `Gig0/1`, `Fa0/1–4` | `Unassigned` | N/A | N/A | Layer 2 Access Ports (VLAN 1)[cite: 2, 5] |
| **Ian's PC** | `FastEthernet0` | `192.168.1.2` *(DHCP)* | `255.255.255.0` | `192.168.1.1` | LAN Client[cite: 6] |
| **Mom's Laptop** | `FastEthernet0` | `192.168.1.3` *(DHCP)* | `255.255.255.0` | `192.168.1.1` | LAN Client[cite: 6] |
| **Mark's PC** | `FastEthernet0` | `192.168.1.4` *(DHCP)* | `255.255.255.0` | `192.168.1.1` | LAN Client[cite: 6] |

---

##  Key Configuration Highlights

Full device configurations are available in the [`/configs`](configs/) directory.

### Router Interface & DHCP Pool (`PLDTWIFI`)
```ios
interface GigabitEthernet0/0
 description WAN_TO_MODEM
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/1
 description LAN_DEFAULT_GATEWAY
 ip address 192.168.1.1 255.255.255.0
 no shutdown

ip dhcp excluded-address 192.168.1.1
ip dhcp pool HOME_LAN_POOL
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8
