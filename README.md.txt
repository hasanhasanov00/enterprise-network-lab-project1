# Enterprise Network Security Lab

A small enterprise network security lab built with Cisco Packet Tracer.

## Project Overview

This project simulates an enterprise network with multiple departments, VLAN segmentation, inter-VLAN routing, centralized DHCP, DNS, web services, secure SSH management, ACL-based traffic filtering, NAT/PAT, and WAN connectivity.

## Technologies

- Cisco Packet Tracer
- VLANs
- 802.1Q Trunking
- Inter-VLAN Routing
- DHCP
- DNS
- HTTP
- ACL
- SSH
- NAT/PAT
- Static Routing
- WAN Connectivity.

## Network Topology

The following diagram shows the overall enterprise network topology:

![Enterprise Network Topology](topology/enterprise.network.png)


## VLAN and IP Addressing

The network is divided into several VLANs. Each department has its own network, while the servers are placed in a separate VLAN.

| VLAN | Name | Network | Gateway |
|------|------|---------|---------|
| 10 | HR | 192.168.10.0/24 | 192.168.10.1 |
| 20 | IT | 192.168.20.0/24 | 192.168.20.1 |
| 30 | SALES | 192.168.30.0/24 | 192.168.30.1 |
| 40 | SERVERS | 192.168.40.0/24 | 192.168.40.1 |

### Servers

| Server | IP | Service |
|--------|----|---------|
| DHCP | 192.168.40.30 | DHCP |
| DNS | 192.168.40.10 | DNS |
| Web | 192.168.40.20 | HTTP |

### WAN Connection

The company router is connected to the ISP router through a /30 network.

| Device | Interface | IP |
|--------|-----------|----|
| COMPANY-RTR | G0/0 | 203.0.113.2/30 |
| ISP-RTR | G0/1 | 203.0.113.1/30 |
| ISP-RTR | Loopback0 | 8.8.8.8/32 |


## Security

Some basic security controls were added to the network.

### VLAN Segmentation

Different departments are separated using VLANs:

- VLAN 10 - HR
- VLAN 20 - IT
- VLAN 30 - SALES
- VLAN 40 - SERVERS

This keeps the departments in separate broadcast domains.

### ACL

An extended ACL was configured on the router.

HR and SALES are blocked from accessing the web server, while other traffic is allowed.

### SSH

SSH was enabled on the company router for remote management.

Telnet was not used.

### NAT/PAT

NAT overload is configured on the company router so internal networks can access the WAN using the router's public IP address.

### DHCP

DHCP is used to automatically assign IP addresses to clients in the HR, IT and SALES VLANs.

### DNS and HTTP

A separate server VLAN is used for internal services.

- DNS: `192.168.40.10`
- Web Server: `192.168.40.20`
- DHCP: `192.168.40.30`


## Testing

The main network functions were tested after configuration.

### Connectivity

- PCs can communicate with their default gateway.
- Inter-VLAN communication was tested.
- The DHCP server successfully assigns IP addresses to clients.
- DNS resolves `www.company.local` to the web server.
- The web server can be accessed from allowed networks.
- HR and SALES access to the web server is blocked by the ACL.
- IT can access the web server.
- SSH access to the company router was tested.
- NAT/PAT was tested by sending traffic from an internal PC to the simulated WAN.
- The simulated external address `8.8.8.8` was reachable from the internal network.

### Useful Verification Commands

```text
show ip interface brief
show ip route
show vlan brief
show interfaces trunk
show access-lists
show ip nat translations
show ip nat statistics
show arp