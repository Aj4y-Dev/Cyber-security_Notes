### How many "questions" (tasks) are there?

There are essentially **10 major tasks** integrated into this one scenario. Here is the checklist you should follow within your single file:

1. **Cleanup:** Remove the default DHCP config on the ASA.
2. **VLAN/Interface Setup:** Configure VLAN 1 (Inside) and VLAN 2 (Outside) with their respective IP addresses and security levels (100 and 0).
3. **Physical Layer:** Assign the ASA physical ports (E0/0 and E0/1) to the correct VLANs.
4. **Addressing:** Configure the IP addresses for the ISP Router and Router 1 interfaces.
5. **Internal DHCP:** Set up the DHCP pool on the ASA for the internal PCs (Range: .5 to .10).
6. **Routing Part A:** Configure a static default route on the ASA pointing to the ISP.
7. **Routing Part B:** Configure OSPF Area 0 on the ISP Router and Router 1 so they can share routes.
8. **NAT:** Create the NAT rule on the ASA so internal traffic can "hide" behind the outside IP.
9. **Security (ACL):** Create and apply the "INTERNET" extended ACL to the outside interface.
10. **Identity/Management:** Create MOTD banners with your full name on **every** switch and router.

## Step 1: ASA 5505 Firewall Configuration

The ASA is the "brain" of this lab. It handles the security zones and NAT.

```
enable
# Press Enter, no password

conf t

# 1. Remove default DHCP
no dhcpd address 192.168.1.5-192.168.1.36 inside

# 2. Configure VLAN1 (Inside)
interface Vlan1
 nameif inside
 security-level 100
 ip address 172.16.XX.1 255.255.255.0

# 3. Configure VLAN2 (Outside)
interface Vlan2
 nameif outside
 security-level 0
 ip address 10.XX.1.1 255.255.255.0

# 4. Assign physical ports
interface Ethernet0/0
 switchport access vlan 2   # Connects to ISP router
interface Ethernet0/1
 switchport access vlan 1   # Connects to internal switch

# 5. Configure DHCP for internal PCs
dhcpd address 172.16.XX.5-172.16.XX.10 inside
dhcpd dns 8.8.XX.8
dhcpd enable inside

# 6. Default route (gateway to ISP router)
route outside 0.0.0.0 0.0.0.0 10.XX.1.2

# 7. NAT for internal network
object network INTERNAL_NET
 subnet 172.16.XX.0 255.255.255.0
 nat (inside,outside) dynamic interface

# 8. ACL for Internet access
access-list INTERNET extended permit tcp any any
access-list INTERNET extended permit icmp any any
access-group INTERNET in interface outside

# 9. Save configuration
banner motd # [Your Full Name] - ASA Firewall #
wr mem
```

✅ **Notes:**

- `XX` = last two digits of your SLC ID.
- DHCP DNS must point to a valid server (8.8.8.8).
- NAT ensures internal PCs can reach external networks.
## Step 2: ISP Router Configuration

Acts as the middle router between ASA and Router1.

```
enable
conf t

# 1. Interfaces
interface g0/0
 ip address 10.XX.1.2 255.255.255.0   # Connects to ASA outside
 no shutdown

interface g0/1
 ip address 15.1.XX.1 255.255.255.252  # Connects to Router1
 no shutdown

# 2. OSPF configuration
router ospf 1
 network 10.XX.1.0 0.0.0.255 area 0
 network 15.1.XX.0 0.0.0.3 area 0

# 3. Save configuration
banner motd # [Your Full Name] - ISP Router #
do wr
```

## Step 3: Router 1 Configuration

This router connects to the server side (8.8.XX.0/24).

```
enable
conf t

# 1. Interfaces
interface g0/0
 ip address 15.1.XX.2 255.255.255.252  # Connects to ISP Router
 no shutdown

interface g0/1
 ip address 8.8.XX.1 255.255.255.0     # Connects to server subnet
 no shutdown

# 2. OSPF configuration
router ospf 1
 network 15.1.XX.0 0.0.0.3 area 0
 network 8.8.XX.0 0.0.0.255 area 0

# 3. Save configuration
banner motd # [Your Full Name] - Router 1 #
do wr
```

## Step 4: Switch Configurations

Even though switches are L2 devices, create a MOTD banner for grading.

```
enable
conf t
banner motd # [Your Full Name] - Switch #
do wr
```

- Apply this to **both internal and server switches**.
- No VLAN routing is required on L2 switches.
## **Step 5: End Devices Setup**

### **PCs A, B, C (Inside VLAN1)**

1. Go to each PC → **Desktop → IP Configuration → DHCP**
2. Verify IP assignment: `172.16.XX.5-10`
3. Gateway should be: `172.16.XX.1`

### **Server (Outside VLAN2)**

1. Go to Server → **Desktop → IP Configuration**
2. Set manually:
    - IP: `8.8.XX.8`
    - Subnet Mask: `255.255.255.0`
    - Default Gateway: `8.8.XX.1`

### **Verification**

```
# From PC A:
ping 8.8.XX.8
# Should succeed after first ARP attempt
```

✅ PCs A, B, C can ping Server.

❌ The reverse (server to PCs) should **fail** because ACL only allows traffic in one direction.

### **Important Notes**

1. ACL `INTERNET` allows **TCP/ICMP inbound to outside** (Internet) but blocks unsolicited inbound from server to PCs.
2. NAT is dynamic, only translates internal IPs when going out.
3. OSPF ensures both routers share routes automatically.
4. Always `wr mem` after config to save.

---

This sequence **avoids errors like missing VLAN assignment, wrong DHCP ranges, or incorrect NAT**.