# OSPF Multi-Router Topology Lab

## Overview

This lab demonstrates the configuration and verification of Open Shortest Path First (OSPF) routing across a multi-router network. The objective was to establish full OSPF neighbor adjacencies, advertise connected LAN networks, and achieve end-to-end connectivity between hosts on different subnets.

---

## Objectives

- Configure OSPF on all routers
- Establish FULL OSPF neighbor relationships
- Advertise connected LAN networks into OSPF
- Verify routing table updates
- Test end-to-end connectivity using ICMP (ping)
- Troubleshoot routing and addressing issues

---

## Network Topology

![Network Topology](images/ospf-topology.png)

---

## IP Addressing

| Device | Interface | IP Address | Subnet Mask |
|---------|-----------|------------|-------------|
| R1 | G0/0 | 192.168.2.1 | /29 |
| R2 | WAN | Configured | /30 |
| R3 | WAN | Configured | /30 |
| R4 | G0/0 | 192.168.1.1 | /29 |
| PC1 | NIC | 192.168.2.2 | /29 |
| PC2 | NIC | 192.168.1.2 | /29 |

> Note: WAN interfaces use /30 point-to-point addressing.

---

## Technologies Used

- Cisco IOS
- OSPF (Process ID 1)
- IPv4
- ICMP

---

## Configuration Highlights

### OSPF

Configured OSPF on all routers using Process ID 1.

Example:

```bash
router ospf 1
network 192.168.2.0 0.0.0.7 area 0
```

---

## Verification Commands

```bash
show ip ospf neighbor

show ip route

show ip protocols

show ip ospf interface brief

ping
```

---

## Routing Table Verification

Expected output:

```
O 192.168.1.0/29
O 192.168.2.0/29
```

This confirms that both LAN networks are being learned dynamically through OSPF.

---

## Troubleshooting

During testing, OSPF neighbors successfully reached the **FULL** state, but end-to-end communication between LANs initially failed.

### Root Cause

The PCs were configured with default gateways belonging to a **different subnet** than their own LAN.

Example of incorrect configuration:

```
PC
IP Address: 192.168.1.2/29
Default Gateway: 10.10.10.1
```

Because the default gateway was outside the local subnet, the PC could not resolve the gateway using ARP, preventing packets from reaching the router.

### Solution

Configured the router interface and default gateway to belong to the same subnet as the PCs.

Correct configuration:

```
PC Address:
192.168.1.2/29

Gateway:
192.168.1.1/29
```

Once corrected, end-to-end connectivity was restored.

---

## Skills Demonstrated

- OSPF Configuration
- OSPF Neighbor Verification
- Dynamic Routing
- IPv4 Addressing
- Wildcard Masks
- Route Advertisement
- Routing Table Analysis
- Network Troubleshooting
- Layer 2 vs Layer 3 Troubleshooting
- ARP Fundamentals

---

## Lessons Learned

One important lesson from this lab is that establishing a FULL OSPF adjacency does **not** guarantee end-to-end connectivity. While OSPF successfully exchanged routing information, incorrect default gateway configuration on the end devices prevented traffic from reaching the routers. This reinforced the importance of validating IP addressing and gateway configuration before troubleshooting routing protocols.

---

## Author

**Ayomide Oyekunle**

Aspiring Network Engineer | CCNA 

GitHub Portfolio documenting networking labs and troubleshooting exercises.
