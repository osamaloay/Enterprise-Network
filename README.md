# Enterprise Network Design

This repository provides a comprehensive guide to designing and implementing a secure, scalable, and highly available enterprise network infrastructure. It includes detailed configurations, subnetting schemes, and security measures to ensure optimal network performance and protection.

## Table of Contents

- [Project Overview](#project-overview)
- [Network Strategy](#network-strategy)
  - [Zones Configuration](#zones-configuration)
  - [Server Placement](#server-placement)
- [Network Components](#network-components)
- [Network Addressing Requirements](#network-addressing-requirements)
- [Technical Details](#technical-details)
- [Configuration Files](#configuration-files)
- [Subnetting Scheme](#subnetting-scheme)
- [Packet Tracer Simulation](#packet-tracer-simulation)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)

## Project Overview

The goal of this project is to design an enterprise network that supports critical services with a focus on security, redundancy, and scalability. The design encompasses various network zones, components, and best practices to meet the organization's requirements.

### Preview
![Project Screenshot](media/Screenshot%20From%202025-04-05%2006-25-51.png)


## Network Strategy

### Zones Configuration

The network is segmented into distinct zones to enhance security and manageability:

- **DMZ Zone**: Hosts publicly accessible servers such as FTP, Web, Email, and Application servers, providing external access while maintaining security.
- **Internal Zone**: Contains critical internal services including Active Directory, DHCP, DNS, and RADIUS, ensuring seamless internal operations.
- **External Zone**: Houses routers connected to two Internet Service Providers (ISPs), providing redundancy and high availability for external connectivity.

### Server Placement

- **DMZ Zone**: Hosts FTP, Web, Email, and Application servers to provide essential services to external users.
- **Internal Zone**: Contains essential services for managing internal resources, including Active Directory, DHCP, DNS, and RADIUS services.
- **External Zone**: Facilitates redundant ISP connections through routers.

## Network Components

The network comprises the following key components:

1. **ISP Connections**: Dual ISP links for redundancy.
2. **Network Security**: Cisco ASA Firewalls to enforce robust perimeter security.
3. **Routing and Switching**: Managed by Cisco Firewalls and multi-layer switches for efficient data flow.
4. **Server Virtualization**: Utilizes two physical servers running virtual machines for various network services.
5. **Wireless Infrastructure**: Cisco Wireless LAN Controllers (WLC) managing lightweight access points (LAPs) for wireless connectivity.
6. **VoIP**: Deployment of Voice over IP services for seamless communication.
7. **External Access**: Secure access for external users via the cloud.

## Network Addressing Requirements

The network employs the following IP addressing scheme:

- **Management Network**: `192.168.100.0/24` – Dedicated for network management devices.
- **WLAN**: `10.20.0.0/16` – Used for wireless LAN infrastructure.
- **LAN**: `172.16.0.0/16` – Internal LAN addressing for general devices.
- **VoIP**: `172.30.0.0/16` – Separate IP range for Voice over IP services.
- **DMZ**: `10.11.11.0/27` – Addressing for servers in the DMZ zone.
- **Public IPs**: `Vodafone 105.100.50.0/30`, `Orange 197.200.100.0/30` – Public IPs for ISP connections.

## Technical Details

Key technical aspects of the network design include:

- **Hierarchical Network Design**: Implements Core, Distribution, and Access layers to ensure redundancy and minimize single points of failure.
- **Wireless LAN Controller (WLC)**: Each department has its own Wireless Access Point (WAP), centrally managed by the Cisco WLC.
- **VLANs**:
  - VLAN 10: Management network
  - VLAN 20: LAN network
  - VLAN 50: WLAN network
  - VLAN 70: VoIP network
  - VLAN 199: Blackhole VLAN for unused or unassigned ports, enhancing security.
- **EtherChannel (LACP)**: Enhances link aggregation efficiency, increases bandwidth, and dynamically resolves physical link failures between devices.
- **STP PortFast and BPDU Guard**: Prevents broadcast storms and reduces network downtime by configuring STP PortFast and BPDU Guard on edge ports.
- **Subnetting**: Proper subnetting techniques allocate appropriate IP ranges for each group and service.
- **Inter-VLAN Communication**: Trunk and access ports facilitate inter-VLAN communication, ensuring VLANs can communicate as needed while maintaining security and segmentation.
- **Core Switches**: Multi-layer switches enable both routing and switching, ensuring efficient data transmission across VLANs.
- **DHCP**: A DHCP server dynamically allocates IP addresses to devices within the enterprise.
- **HSRP (Hot Standby Router Protocol)**: Ensures router redundancy for network reliability, allowing seamless failover.
- **Static IPs for Servers**: Critical servers are assigned static IP addresses to ensure consistent access and reliability.
- **OSPF (Open Shortest Path First)**: Used for internal routing between routers, providing dynamic routing capabilities and efficient path selection.
- **ACLs and SSH**: Implements Access Control Lists (ACLs) for traffic control and uses SSH for secure remote administration.

## Configuration Files

The repository includes configuration files for various network devices:

- **Routers**: Located in the `routers/` directory.
- **Switches**: Located in the `switches/` directory.
- **Firewalls**: Located in the `firewalls/` directory.

These files provide detailed command-line configurations for each device type.

## Subnetting Scheme

A detailed subnetting scheme is provided in the `subnets.txt` file, outlining the IP address allocations for different network segments.

## Packet Tracer Simulation

The `University.pkt` file is a Packet Tracer simulation of the network, allowing for practical exploration and testing of the network design.

## Usage

To explore the network configurations:

1. Clone the repository:
   ```bash
   git clone https://github.com/osamaloay/Enterprise-Network.git
