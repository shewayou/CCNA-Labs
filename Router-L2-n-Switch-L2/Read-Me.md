# CCNA Lab: Inter-VLAN Routing & Core Connectivity

## Overview
This lab represents "Part 3" of a comprehensive CCNA routing and switching topology. The primary objective is to establish foundational Layer 2 and Layer 3 connectivity before implementing dynamic routing protocols (OSPF). 

This configuration utilizes the **"Inter-VLAN Routing, Option 3 (Layer 3 Switch)"** method.

## Topology Structure
* **ISP:** ISR4331 Router (Simulating the external internet gateway)
* **R1:** ISR4331 Router with NIM-E2-4 L2 link to SW1 (Edge router connecting the local network to the ISP)
* **SW1:** 2960-24TT Layer 2 Switch (Core switch handling inter-VLAN routing and DHCP)
* **SW2, SW3, SW4:** 2960-24TT Layer 2 Switches (Access layer switches connecting end-user PCs)

## VLAN Allocation
* **VLAN 10:** Eng (Engineering)
* **VLAN 20:** Sales
* **VLAN 30:** Admin
* **VLAN 99:** Management (Used for SSH access to network devices)
* **VLAN 199:** Native (Used for 802.1Q trunking)

## IP Addressing Schema
* **IPv4:** `192.168.X.0/24` (Where X = VLAN ID)
* **IPv6:** `2001:db8:0:X::/64` (Where X = VLAN ID)
* **Loopbacks:** Isolated subnets (e.g., `192.168.0.21` for SW1 and `192.168.1.1` for R1) to prevent SVI overlaps.

## Features Configured
1. **DHCP:** SW1 acts as the DHCP server for VLANs 10, 20, and 30.
2. **SLAAC:** IPv6 Unicast Routing is enabled on SW1 and R1 to allow for automatic IPv6 host addressing.
3. **SSH Access:** Version 2 configured on all devices via VLAN 99 SVIs.
4. **Static Routing:** Minimal static routes applied on SW1 and R1 to allow for end-to-end ICMP verification prior to OSPF configuration.

## ⚠️ Important Deployment Notes
### 1. Catalyst 2960 IPv6 Limitation
Out of the box, Cisco 2960 switches operate on an SDM (Switch Database Management) template that does not support IPv6. Before applying the `SW2.txt`, `SW3.txt`, or `SW4.txt` configurations, you must enable dual-stack support:
```text
SW(config)# sdm prefer dual-ipv4-and-ipv6 default
SW(config)# end
SW# write memory
SW# reload



