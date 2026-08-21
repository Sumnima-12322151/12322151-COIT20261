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

![Network](./image/Week_2/Four_Linux_hosts_and_one_Ethenet_switch_added_to_the_GNS3_project.png)

Figure 1: Four Linux hosts and one Ethernet switch added to the GNS3 project

The four hosts were then linked to the Ethernet switch via their respective eth0 ports. This resulted in a basic LAN in which all devices could interact via the switch.

Figure 2: Completed GNS3 LAN topology with all four hosts connected to Switch1

### IP Addressing Scheme

The network utilised the following IPv4 subnet:

10.1.1.0/24

The Subnet Mask was:

255.255.255.0

The hosts were given addresses from this network.

| Device | IP Address | Subnet |
| -------| ---------- | ------ |
| Host1	| 10.1.1.1	| /24 |
| Host2	| 10.1.1.2	| /24 |
| Host3	| 10.1.1.3	| /24 |
| Host4	| 10.1.1.4	| /24 |

#### Method 1 – Configuring an IP Address Using GNS3

The initial host's static IP addresses were set using the GNS3 Configure option.

For example, Host1 was set up with:

auto eth0 
iface eth0 inet static 
address 10.1.1.1 
netmask 255.255.255.0

Figure 3: Static IPv4 configuration for Host1 using the GNS3 network configuration window

After launching the host, the IP address was verified using:

ip a

The report indicated that the eth0 interface had successfully received:

10.1.1.1/24

Figure 4: Verification of Host1's IP address using the ip a command

#### Method 2 – Editing /etc/network/interfaces

Another approach for configuring IP addresses was to change the Linux network configuration file directly.

The file was opened with:

nano /etc/network/interfaces

The static configuration has been entered into the file.

Figure 5: Editing the /etc/network/interfaces file using the Nano text editor

For Host3, the IP address was set as follows:

auto eth0 
iface eth0 inet static 
address 10.1.1.3 
netmask 255.255.255.0

Figure 6: Configuring Host3 with the static IPv4 address 10.1.1.3

After modifying the settings, the interface was refreshed with instructions like:

ifdown eth0
ifup eth0

The settings was then verified with:

ip a

The result verified that Host3 was using:

10.1.1.3/24

Figure 7: Host3 console showing the configured 10.1.1.3/24 address

#### Method 3 – Using the ip address add Command

Host4's static IP address was assigned straight from the command line using:

ip address add 10.1.1.4/24 dev eth0

The address was then validated with:

ip a

The output displayed:

10.1.1.4/24

Figure 8: Host4 configured with 10.1.1.4/24 using the Linux ip command

This approach assigns the IP address immediately. However, unlike the /etc/network/interfaces technique, the configuration is not generally saved when the device is restarted.

## Task 2 – Testing Network Connectivity Using Ping

After establishing the IP addresses, I used the ping command to check the connection between the Linux systems.

### Basic Ping Test

From Host 1, I checked connection to Host 2 using:

ping 10.1.1.2

The host successfully received ICMP responses from 10.1.1.2.

Figure 9: Successful ping from Host1 to Host2

The results showed:

There were 6 packets transmitted and 6 received, with 0% packet loss.

The round-trip times were approximately:

Min/average/max = 0.121 / 0.193 / 0.266 ms.

This proved that Host2 could be reached from Host1 and that the LAN was running properly.

### Testing an Invalid IP Address

I then tried to ping an address that wasn't assigned to any device on the network:

ping 10.1.1.10

The results returned:

The destination host is not accessible.

Figure 10: Ping test to an unavailable IP address

Statistics showed:

0 packets were received, resulting in 100% packet loss.

This explained how ping may be used to check if a destination device is accessible.

### Testing Different Ping Options

Different ping settings were also examined to determine how they impact network testing.

#### Limiting the Number of Packets

The -c option was used to restrict the amount of ICMP echo requests.

ping -c 3 10.1.1.2

Figure 11: Ping test limited to three packets

The results showed:

There were 3 packets broadcast and 3 packets received, with 0% loss.

The -c 3 option automatically terminated the test after three packets were transmitted.

#### Changing the Ping Interval

The interval between packets was modified using the -i option.

ping -i 10.1.1.2

Figure 12: Ping test using a modified transmission interval

The -i 10 option forced the host to wait around ten seconds between ICMP echo queries.

#### Changing the Packet Size

The quantity of data transmitted in each ping packet was changed as follows:

ping -s 100 10.1.1.2

Figure 13: Ping test using a data size of 100 bytes

The result revealed packets with a total displayed size of about:

108 Bytes

This is due to the inclusion of extra ICMP-related information together with the requested payload.

#### Combining Ping Options

Finally, many choices were consolidated into a single command:

ping -s 100 -c 3 -i 2 10.1.1.2

Figure 14: Ping test using packet size, count, and interval options together

Within this command:
- -s 100 sets the data size to 100 bytes.
- The -c 3 option restricts the test to three packets.
- -i 2 specifies a two-second delay between packets.
- The target IP address is 10.1.1.2.

The results showed:

There were 3 packets broadcast and 3 packets received, with 0% loss.

This proved that Host1 and Host2 were completely available even with altered ping settings.

## Commands Used

The primary commands utilised in this practical were:

ip a

Displays the network interfaces and their IP addresses.

nano /etc/network/interfaces

opens the Linux network interface configuration file.

ifdown eth0
ifup eth0

Restarts the eth0 interface so that modifications to the configuration file may be applied.

ip address add 10.1.1.4/24 dev eth0

Directly assigns an IPv4 address to an interface.

ping 10.1.1.2

Checks basic connection to another host.

ping -c 3 10.1.1.2

Limits the ping test to three queries.

ping -i 10.1.1.2

Changes the time between ping requests.

ping -s 100 10.1.1.2

Changes the data size of the ICMP echo request.

ping -s 100 -c 3 -i 2 10.1.1.2

Combines packet size, count, and interval parameters into a single command.

## What I Learned

During this week's practical assignment, I learned:
- Create a simple LAN in GNS3
- Connect numerous Linux hosts using an Ethernet switch
- Understand IPv4 addresses and the /24 subnet notation
- Configure a static IPv4 address via the GNS3 interface
- Edit the /etc/network/interfaces file to configure an IP address
- Use the Linux ip command to configure an IP address temporarily
- Use ip a to check network interface settings
- Use ping to see if another network device is reachable
- Interpret packet transmission, reception, and loss
- Understand the round-trip time (RTT)
- Determine the difference between successful and failed pings
- Using command-line arguments, you may change the number of ping tests, the interval, and the size of each packet

## Conclusion

The Week 2 practical gave participants hands-on experience with IPv4 configuration and basic network troubleshooting in GNS3. Using the 10.1.1.0/24 network, four Linux computers and an Ethernet switch were successfully connected to form a LAN.

Static addresses were configured using a variety of methods, including the GNS3 configuration interface, the /etc/network/interfaces file, and the Linux ip address add command. Ping connectivity tests verified effective communication between appropriately configured hosts, however pinging an unused address resulted in Destination Host Unreachable and packet loss.

Overall, this exercise increased my knowledge of Linux network settings, IPv4 addressing, ICMP, packet loss, and round-trip time.
