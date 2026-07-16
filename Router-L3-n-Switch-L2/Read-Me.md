# CCNA Lab: Inter-VLAN Routing & Core Connectivity

## Overview
This lab represents "Part 2" of a comprehensive CCNA routing and switching topology. The primary objective is to establish foundational Layer 2 and Layer 3 connectivity before implementing dynamic routing protocols (OSPF). 

This configuration utilizes the **"Inter-VLAN Routing, Option 2 (Router-on-a-Stick)"** method.

## Topology Structure
* **ISP:** ISR4331 Router (Simulating the external internet gateway)
* **R1:** ISR4331 Router (Edge router connecting the local network to the ISP, inter-VLAN routing via subinterfaces, and DHCP)
* **SW1:** 2960-24TT Layer 2 Switch (Serving as a central trunking distribution switch)
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
* **Loopbacks:** Isolated subnets (e.g., `192.168.1.1` for R1) to prevent SVI overlaps.

## Features Configured
1. **DHCP:** R1 acts as the DHCP server for VLANs 10, 20, and 30.
2. **SLAAC:** IPv6 Unicast Routing is enabled on R1 to allow for automatic IPv6 host addressing via subinterfaces.
3. **SSH Access:** Version 2 configured on all devices via VLAN 99 SVIs.
4. **Routing & Gateways:** R1 utilizes a static default route to the ISP. SW1, SW2, SW3, and SW4 utilize the `ip default-gateway` command pointing to R1's VLAN 99 subinterface IP for management traffic..

## Logical Configuration Checklist
For every device deployed in this lab, the following configuration flow is verified:
1. Identify the IPv6 link-local address.
2. Identify interfaces to other devices (e.g., gig0/1/0, gig0/0/0, fa0/1, or fa0/11).
3. Determine if the device is a Router or a Switch.
    * **If Router:** Is the link strictly L3-capable (ISR4331 built-in gig0/0/0) or L2-capable (ISR4331 NIM-ES2-4 switch module)? Apply routing or subinterface commands accordingly.
    * **If Switch:** Is it an L3 Switch (3650) requiring `ip routing` and SVIs, or an L2 Switch (2960) requiring trunks and an `ip default-gateway`?
4. Assign IPv4 and IPv6 addresses on each link or VLAN.
5. Create Loopback addresses (Note: Layer 2 2960 switches do not support loopbacks).
6. Configure the DHCP pools on the acting routing device.
7. Establish basic static routing or default gateways to ensure end-to-end connectivity.


## ⚠️ Important Deployment Notes
### 1. Catalyst 2960 IPv6 Limitation
Out of the box, Cisco 2960 switches operate on an SDM (Switch Database Management) template that does not support IPv6. Before applying the `SW2.txt`, `SW3.txt`, or `SW4.txt` configurations, you must enable dual-stack support:
```text
SW(config)# sdm prefer dual-ipv4-and-ipv6 default
SW(config)# end
SW# write memory
SW# reload


