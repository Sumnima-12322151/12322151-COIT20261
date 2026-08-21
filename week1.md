# Week 1 – Introduction to GNS3 Basics

## Overview

This week's practical assignment was designed to familiarise students with GNS3's fundamental capabilities as well as teach them how to establish and run a tiny network project. The task entailed creating a GNS3 project, adding a Linux host, setting its network interface, booting the device, viewing its console, and verifying its allocated IP address.

## Tasks Completed

During this practical session, I accomplished the following tasks:
- Created a new project using the GNS3 Web Interface.
- The network architecture has been updated with the addition of a Linux host (Host1).
- Included an annotation with the unit title, name, and student ID.
- Successfully started the Host1 node.
- Accessed the Linux console on Host 1.
- To see network interface details, I used the Linux command ip a.
- Verified the IP address given to the eth0 interface.
- Took screenshots of the GNS3 topology and the Host1 console to demonstrate that the task was complete.

## GNS3 Network Topology

The screenshot below depicts the GNS3 project, which includes Host1. The host was successfully added to the topology and started, as seen by the green status signal.

![Network](./image/GNS3_topology_containing_a_Linux_Host_named_Host1.png)

Figure 1: GNS3 topology containing a Linux Host named Host1

## Checking the IP Address

After running Host1, I opened its console and typed the following Linux command.

ip a

This program displays the list of accessible network interfaces together with their IP settings.

![Network](./image/Host1_console_displaying_its_network_interface_and_IP_address.png)

Figure 2: Host1 console displaying its network interface and IP address

Based on the console output, the eth0 interface was operational and had the following IPv4 address:

192.168.0.42/24

The /24 prefix denotes the subnet mask.

255.255.255.0

The console also displayed the loopback interface lo, which had the normal loopback address:

127.0.0.1/8

## What I Learned

During this activity, I learned:
- Create and manage a simple project in GNS3
- Add a Linux host to your GNS3 topology
- Starting and stopping network devices in GNS3
- Enter the console of a Linux host
- Use the standard Linux networking commands
- Identify network interfaces like eth0 and lo
- Check the IPv4 address associated with a network interface
- Understand fundamental CIDR notation, such as /24
- Take screenshots and use GitHub to document your networking activity

## Conclusion

The Week 1 exercise was an introduction to using GNS3 for network simulation. I successfully constructed a basic topology with one Linux server, entered its console, and confirmed that the eth0 interface was using the IP address 192.168.0.42/24. This practical practice helps to develop the fundamental GNS3 and Linux networking abilities necessary for the next networking exercises.
