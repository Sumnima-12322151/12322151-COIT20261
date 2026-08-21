# Week 4 – HTTP Clients, Multi-Subnet Routing and Packet Capture

## Overview

The Week 4 practical concentrated on HTTP client-server communication over different routed networks. A GNS3 architecture was established, with three subnets linked by two Linux routers.

The practical introduces:
- Multi-subnet IPv4 communication
- Router interface setup
- Routing tables and Default gateways
- Connectivity testing using ping
- A Firefox-based graphical HTTP client and VNC connection to a graphical GNS3 host
- Use GNS3 to collect packets and Wireshark for examination
- HTTP transmission using TCP port 80
- Command-line HTTP clients, such as wget and curl

## Task 1 – HTTP Client with GUI

### Network Topology

The GNS3 topology consisted of:
- Host1 – Firefox HTTP client
- Switch1
- Router1
- Switch2
- Router2
- Switch3
- Server1 – HTTP server

Figure 1: GNS3 topology used for the Week 4 HTTP client activity

The logical communication pathway was:

Host1 → Switch1 → Router1 → Switch2 → Router2 → Switch3 → Server1

The topology was built such that the HTTP client and server were on separate networks. As a result, traffic between them had to go via both Routers 1 and 2.

### IPv4 Network Structure

The router setup depicted in the screenshots utilised three IPv4 networks:

| Subnet   | IPv4 Network    | Purpose                             |
| -------- | --------------- | ----------------------------------- |
| Subnet A | 192.168.10.0/24 | Client-side network                 |
| Subnet B | 192.168.20.0/24 | Network between Router1 and Router2 |
| Subnet C | 192.168.30.0/24 | Server-side network                 |

The major addresses recorded in the configuration were:

| Device  | Interface | IPv4 Address     |
| ------- | --------- | ---------------- |
| Host1   | eth0      | 192.168.10.10/24 |
| Router1 | eth0      | 192.168.10.1/24  |
| Router1 | eth1      | 192.168.20.1/24  |
| Router2 | eth0      | 192.168.20.2/24  |
| Router2 | eth1      | 192.168.30.1/24  |
| Host2   | eth0      | 192.168.30.10/24 |

## Router1 Configuration

The command:

ip addr

was used to verify the interfaces set on Router1.

Figure 2: IPv4 configuration of Router1

Router1 has two active IPv4 interfaces:

eth0: 192.168.10.1/24
eth1: 192.168.20.1/24

This enabled Router1 to link the client-side network to the intermediate network.

### Router1 Routing Table

The following command was executed:

ip route

Figure 3: Routing table of Router1

The result displayed two directly linked networks:

192.168.10.0/24 dev eth0 scope link src 192.168.10.1
192.168.20.0/24 dev eth1 scope link src 192.168.20.1

This meant that Router1 could immediately connect to Subnet A and Subnet B via their respective interfaces.

## Router2 Configuration

The routing table of Router 2 was examined using:

ip route

Figure 4: Routing table of Router2

The directly related networks were:

192.168.20.0/24 dev eth0 scope link src 192.168.20.2
192.168.30.0/24 dev eth1 scope link src 192.168.30.1

As a result, Router2 linked Subnet B to Subnet C on the server side.

### Router2 Interface Addresses

The command:

ip addr

was also utilised to validate the Router2 interfaces.

Figure 5: IPv4 interfaces configured on Router2

Important addresses were:

eth0: 192.168.20.2/24
eth1: 192.168.30.1/24

Routers 1 and 2 supplied the path for packets to transit between the HTTP client and the server.

## Host Configuration and Default Gateway

The end host on Subnet C was setup as follows:

192.168.30.10/24

Figure 6: IPv4 configuration of the host on Subnet C

The routing table was verified using:

ip route

Figure 7: Host routing table showing its default gateway

The output contained:

default via 192.168.30.1 dev eth0
192.168.30.0/24 dev eth0 scope link src 192.168.30.10

The address:

192.168.30.1

was Router2's interface on the same subnet, and hence served as the default gateway.

When a host wants to interact with a target outside of its local subnet, it uses the default gateway.

## Testing Connectivity Between Routers

Prior to trying HTTP connection, Router1 and Router2's connectivity was checked.

The command used by Router1 was:

ping -c 5 192.168.20.2

Figure 8: Successful connectivity test between Router1 and Router2

The results showed:

There were 5 packets broadcast and 5 packets received with 0% loss.

This demonstrated that the two routers could properly interact across Subnet B.

## Server Interface Verification

The server interface was tested with:

ip addr

Figure 9: Server1 network interface information

Checking the server address was critical since the HTTP client had to use the proper server IP address when requesting the webpage.

## Accessing the Firefox Client through VNC

Host1 ran a graphical Firefox environment and was accessible using the GNS3 VNC/noVNC interface.

Within Host1, a terminal was opened and the network setup was examined using:

ip addr

Figure 10: Firefox Host1 accessed through the graphical environment

The screenshot displayed:

192.168.10.10/24

on the eth0 interface.

This put Host1 on Subnet A.

Firefox represented the HTTP client, while Server1 represented the HTTP server.

