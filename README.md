# **Enterprise Network Design**

This document outlines the architecture and configuration details for a highly available, secure, and scalable **Enterprise Network**. The design includes multiple network zones, key components, services, and best practices to ensure optimal network performance, security, and redundancy.

---

## **A) Network Strategy**

1. **Zones Configuration**:
   - **DMZ Zone**: This zone will house publicly accessible servers such as FTP, Web, Email, and Application servers, providing external access while maintaining security.
   - **Internal Zone**: The internal network will contain critical services such as Active Directory (for user authentication and management), DHCP, DNS, and RADIUS, ensuring seamless internal operations.
   - **External Zone**: This zone will house routers connected to two Internet Service Providers (ISPs), providing redundancy and high availability for external connectivity.

2. **Server Placement**:
   - The **DMZ** zone will host the FTP, Web, Email, and Application servers to provide essential services to external users.
   - The **Internal Zone** will contain essential services for managing internal resources, including Active Directory, DHCP, DNS, and RADIUS services.
   - The **External Zone** will facilitate redundant ISP connections through routers.

---

## **B) Network Components**

1. **ISP Connection**: Two ISPs will provide internet connectivity for redundancy.
2. **Network Security**: Cisco ASA Firewalls will be deployed to ensure robust perimeter security, controlling incoming and outgoing traffic.
3. **Network Routing**: Routing and switching will be handled by Cisco Firewalls and multi-layer switches, ensuring efficient data flow across the network.
4. **Server Virtualization**: Two physical servers will be utilized for virtualization, using a hypervisor to create virtual machines that run various network services.
5. **Wireless Infrastructure**: Cisco Wireless LAN Controllers (WLC) will manage lightweight access points (LAPs) for wireless connectivity across the network.
6. **VoIP**: Voice over IP services will be deployed to ensure seamless communication within the enterprise.
7. **External Users**: External users will securely access the network via the cloud.

---

## **C) Network Addressing Requirements**

1. **Management Network**: IP range `192.168.100.0/24` – Dedicated for network management devices.
2. **WLAN**: IP range `10.20.0.0/16` – Used for wireless LAN infrastructure.
3. **LAN**: IP range `172.16.0.0/16` – Internal LAN addressing for general devices.
4. **VoIP**: IP range `172.30.0.0/16` – Separate IP range for Voice over IP services.
5. **DMZ**: IP range `10.11.11.0/27` – Addressing for servers in the DMZ zone.
6. **Public IPs**: `Vodafone 105.100.50.0/30`, `Orange 197.200.100.0/30` – Public IPs for ISP connections.

---

## **D) Technical Details**

1. **Hierarchical Network Design**: To ensure network redundancy and minimize single points of failure, the network will use a hierarchical design with Core, Distribution, and Access layers.
2. **Wireless LAN Controller (WLC)**: Each department will have its own **Wireless Access Point (WAP)**, managed centrally by the Cisco WLC.
3. **VLAN IDs**: 
   - **VLAN 10**: Management network
   - **VLAN 20**: LAN network
   - **VLAN 50**: WLAN network
   - **VLAN 70**: VoIP network
   - **VLAN 199**: Blackhole VLAN (for unused or unassigned ports, enhancing security)
4. **EtherChannel (LACP)**: LACP (Link Aggregation Control Protocol) will be implemented to enhance link aggregation efficiency, increase bandwidth, and dynamically resolve physical link failures between devices.
5. **STP PortFast and BPDU Guard**: To prevent broadcast storms and reduce network downtime, **STP PortFast** and **BPDU Guard** will be configured on edge ports to immediately forward traffic and protect against loops during ARP requests.
6. **Subnetting**: The network will use proper subnetting techniques to allocate appropriate IP ranges for each group and service.
7. **Inter-VLAN Communication**: Trunk and access ports will be configured to facilitate inter-VLAN communication, ensuring that VLANs can communicate as needed while maintaining security and segmentation.
8. **Core Switches**: Multi-layer switches will be configured to enable both **routing** and **switching**, ensuring efficient data transmission across VLANs.
9. **DHCP**: A DHCP server will be deployed within the internal network to dynamically allocate IP addresses to devices within the enterprise.
10. **HSRP (Hot Standby Router Protocol)**: HSRP will be configured to ensure **router redundancy** for network reliability, ensuring that if one router fails, another router will seamlessly take over the role of the default gateway.
11. **Static IPs for Servers**: Critical servers will be assigned static IP addresses to ensure consistent access and reliability.
12. **OSPF (Open Shortest Path First)**: OSPF will be used for internal routing between routers, providing dynamic routing capabilities and efficient path selection.
13. **ACLs and SSH**: Standard **Access Control Lists (ACLs)** will be implemented for controlling traffic flow, and **SSH** will be used for secure remote administration.

---

## **E) Configuration Commands**

Below are the key configuration commands used on routers and switches for this enterprise network.

### **1. VLAN Configuration (Switch)**

```bash
# Create VLANs on a Cisco switch
vlan 10
 name Management
 exit

vlan 20
 name LAN
 exit

vlan 50
 name WLAN
 exit

vlan 70
 name VoIP
 exit

vlan 199
 name Blackhole
 exit
```

### **2. Inter-VLAN Routing (Layer 3 Switch)**

```bash
# Enable routing on the multi-layer switch
ip routing

# Assign IP addresses to VLAN interfaces (SVIs)
interface vlan 10
 ip address 192.168.100.1 255.255.255.0
 no shutdown
 exit

interface vlan 20
 ip address 172.16.0.1 255.255.255.0
 no shutdown
 exit

interface vlan 50
 ip address 10.20.0.1 255.255.0.0
 no shutdown
 exit

interface vlan 70
 ip address 172.30.0.1 255.255.0.0
 no shutdown
 exit
```

### **3. Trunk Port Configuration (Switch)**

```bash
# Configure trunk port on the switch
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,50,70
 exit
```

### **4. Access Port Configuration (Switch)**

```bash
# Configure access port for VLAN 20 (LAN)
interface GigabitEthernet0/2
 switchport mode access
 switchport access vlan 20
 exit
```

### **5. EtherChannel Configuration (Switch)**

```bash
# Configure EtherChannel with LACP (Link Aggregation Control Protocol)
interface range GigabitEthernet0/1 - 2
 channel-group 1 mode active
 exit
```

### **6. HSRP Configuration (Router)**

```bash
# Configure HSRP on a Cisco router
interface GigabitEthernet0/0
 ip address 172.16.0.2 255.255.255.0
 standby 1 ip 172.16.0.1
 standby 1 priority 110
 standby 1 preempt
 exit
```

### **7. OSPF Configuration (Router)**

```bash
# Configure OSPF routing on a Cisco router
router ospf 1
 network 172.16.0.0 0.0.255.255 area 0
 network 10.20.0.0 0.0.255.255 area 0
 exit
```

### **8. DHCP Configuration (Server)**

```bash
# Configure DHCP on a Cisco router or server
ip dhcp pool LAN-Pool
 network 172.16.0.0 255.255.255.0
 default-router 172.16.0.1
 lease 7
 exit
```

### **9. ACL Configuration (Router/Switch)**

```bash
# Configure ACL to permit only specific traffic
access-list 100 permit ip 172.16.0.0 0.0.255.255 any
access-list 100 deny ip any any

# Apply ACL to interface
interface GigabitEthernet0/0
 ip access-group 100 in
 exit
```

