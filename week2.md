# Week 2 – Static IP Addressing and Network Connectivity Testing

## Overview

The Week 2 practical assignment involved setting static IPv4 addresses on Linux computers in GNS3 and verifying communication between hosts using the ping function. A basic Local Area Network (LAN) was established by connecting four Linux machines via an Ethernet switch.

The activity also covered several ways for allocating IP addresses in Linux and showed how to use ping to verify device reachability, packet loss, and round-trip time (RTT).

## Task 1 – Setting Static IP Addresses

### Network Topology

For this challenge, I developed a GNS3 network that includes:

- Host 1
- Host 2
- Host 3
- Host 4
- A single Ethernet switch

Initially, the devices were integrated into the GNS3 workspace.

Figure 1: Four Linux hosts and one Ethernet switch added to the GNS3 project.

The four hosts were then linked to the Ethernet switch via their respective eth0 ports. This resulted in a basic LAN in which all devices could interact via the switch.

Figure 2: Completed GNS3 LAN topology with all four hosts connected to Switch1.

### IP Addressing Scheme

The network utilised the following IPv4 subnet:

10.1.1.0/24

The Subnet Mask was:

255.255.255.0

The hosts were given addresses from this network.

| Device |	| IP Address |	| Subnet |
--------------------------------------
| Host1 |	| 10.1.1.1 |	| /24 |
--------------------------------
| Host2 |	| 10.1.1.2 |	| /24 |
---------------------------------
| Host3 |	| 10.1.1.3 |	| /24 |
----------------------------------
| Host4 |	| 10.1.1.4 |	| /24 |
--------------------------------
