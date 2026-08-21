# Week 5 – Routing Tables, IP Forwarding and Dynamic Routing with OSPF

## Overview

The Week 5 practical focused on learning how routers forward packets between IP networks and how routing information is stored.

The practical was divided into two major tasks.
- View routing tables and enable IP forwarding
- Configure dynamic routing using OSPF and FRRouting (FRR)

During the activities, I created numerous subnets, allocated static IPv4 addresses, set default gateways, enabled packet forwarding on a Linux router, reviewed routing tables, tested connectivity with ping, and analysed dynamic routing behaviour using traceroute and OSPF.

## Task 1 – Viewing Routing Tables and IP Forwarding

### Network Topology

For the first task, I developed a network that includes:

Host1, Host2, Host3, Router1, and Switch1

Hosts 1 and 2 were linked to the Ethernet switch. The switch connected to Router1 via eth0, but Host3 connected directly to Router1 via eth1.

Figure 1: GNS3 topology consisting of three Linux hosts, one Ethernet switch, and one Linux router

This topology resulted in two independent IPv4 subnets linked by Router1.

### IPv4 Addressing Scheme

The initial subnet used:

10.1.1.0/24

The second subnet used:

10.1.2.0/24

The address configuration was:

| Device  | Interface | IPv4 Address | Purpose                        |
| ------- | --------- | ------------ | ------------------------------ |
| Router1 | eth0      | 10.1.1.1/24  | Gateway for subnet 10.1.1.0/24 |
| Host1   | eth0      | 10.1.1.2/24  | Host on first subnet           |
| Host2   | eth0      | 10.1.1.3/24  | Host on first subnet           |
| Router1 | eth1      | 10.1.2.1/24  | Gateway for subnet 10.1.2.0/24 |
| Host3   | eth0      | 10.1.2.2/24  | Host on second subnet          |

### Configuring Host1

Host1 was setup using a static IP address.

10.1.1.2

The Subnet Mask was:

255.255.255.0

The default gateway was set as follows:

10.1.1.1

The setup utilised was:

auto eth0
iface eth0 inet static 
address 10.1.1.2
netmask 255.255.255.0
gateway 10.1.1.1

up sysctl net.ipv4.ip_forward=0

Figure 2: Static IPv4 configuration for Host1

IP forwarding was turned off because Host1 was behaving as an end host rather than a router.

### Configuring Host2

Host 2 was configured with:

IP address: 10.1.1.3 
Subnet mask: 255.255.255.0
Gateway: 10.1.1.1

The configuration was:

auto eth0
iface eth0 inet static
address 10.1.1.3
netmask 255.255.255.0
gateway 10.1.1.1

up sysctl net.ipv4.ip_forward=0

Figure 3: Static IPv4 configuration for Host2

### Configuring Router1

Router 1 joined the two distinct IPv4 networks.

The first interface, eth0, was setup as follows.

auto eth0 
iface eth0 inet static 
address 10.1.1.1 
netmask 255.255.255.0

IP forwarding was enabled with:

up sysctl net.ipv4.ip_forward=1

Figure 4: Router1 eth0 interface configured for the 10.1.1.0/24 subnet

The second interface, eth1, was setup as follows.

auto eth1 
iface eth1 inet static 
address 10.1.2.1 
netmask 255.255.255.0

Figure 5: Router1 eth1 interface configured for the 10.1.2.0/24 subnet

Router1 therefore had one interface for each subnet and could route traffic between them.

### Configuring Host3

Host3 was installed on the second subnet and configured as follows:

IP address: 10.1.2.2
Subnet mask: 255.255.255.0
Gateway: 10.1.2.1

The setup utilised was:

auto eth0
iface eth0 inet static
address 10.1.2.2
netmask 255.255.255.0
gateway 10.1.2.1

up sysctl net.ipv4.ip_forward=0

Figure 6: Static IPv4 configuration for Host3

## Verifying IP Addresses and Forwarding

The configuration was verified using the following commands:

Sysctl net.ipv4.ip_forward

and:
ip a

For regular hosts, the output displayed:

net.ipv4.ip_forward=0

This indicates that the device does not forward packets between interfaces.

### Host1 Verification

Host 1 showed:

inet 10.1.1.2/24

and:

net.ipv4.ip_forward=0

Figure 7: Verification of Host1's IPv4 address and forwarding status

### Host2 Verification

Host2 demonstrated:

inet 10.1.1.3/24

forwarding was disabled.

Figure 8: Verification of Host2's IPv4 configuration

### Router1 Verification

Router 1 displayed two configured IPv4 interfaces:

eth0: 10.1.1.1/24
eth1: 10.1.2.1/24

The router also displayed:

net.ipv4.ip_forward=1

Figure 9: Router1 interfaces and enabled IPv4 forwarding

This configuration is critical because the router must be able to receive an IP packet on one interface and route it through another.

### Host3 Verification

Host3 demonstrated:

inet 10.1.2.2/24

and:

net.ipv4.ip_forward=0

Figure 10: Host3 IP address and forwarding configuration

## Testing Communication Between Different Subnets
