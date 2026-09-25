#layer3
#ip

# Contents
1. binary to hexadecimal
2. Reason why IPv6
3. IPv6 Abbreviation Rules
4. Finding the IPV6 Prefix (Global Unicast Address)
5. EUI - Extended Unique Identifier
6. Global Unicast Address
7. Uniqe Local Address
8. Link Local Address
9. Multicast Adresses
10. Multicast Address Scopes
11. Anycast
12. Unspecified IPV6  Address
13. Loopback Address

## Table of Addresses
| Name               | Address    | Description |
| ------------------ | ---------- | ----------- |
| Link-Local Address | FE80:://10 |             |

## Troubleshooting
- 2001:0DB8::/32 is specifically reserved by IETF (RFC 3849) for documentations and examples. 
- Link-local addresses and 

## Context
- IPv5 - developed in the late 1970s, but never really introduced for public use (never called IPv5 but value of 5 in the version field of the ip header)
	- sp when the succesor of IPv4 was being developed, it was named IPv6 
- IPv6 doesn't use broadcast
- Unicast addresses are one-to-one
	- one source to one destination
- Broadcast addresses are one-to-all
	- One source to all destinations (within the subnet)
- Multicast addresses are one-to-many
	- One source to multiple destinations (that have joined the specific multicast group).
## Hexadecimal Review
- binary = 0b, decimal = 0d, hexadecimal = 0x
- hexadecimal is 4 binary bits for 1 hex since 1111 = 15
- We can readily turn binary into hexadecimals by grouping it into fours
	- then turning each groups of 4s into decimals
	- we then turn the decimals into hex then combine the two hex
	- ex: convert 0b11011011 (ans = 0xDB)
- We can turn hexadecimals into binary by doing the reverse of it
	- split the hex into two hex numbers
	- turn it into decimal
	- then turn it into binary

## What is IPv6
- 128 bit address
- every additional bit double the number of possible addresses so there are 340,282,366,920,938,463,463,374,607,431,768,211,456 addresses

## IPv6 Header (40 bytes)
![[Pasted image 20260925134643.png]]
- 1st Row (VTF)
	- Version - 4 bits - indicates version of IP that is used (0b0110)
	- Traffic class- 8 bits - used for QoS, indicate high priority traffic
	- Flow Label - 20 bits - used to identify traffic flows (communications between a specific source and destination)
- 2nd Row (PNH)
	- Payload Length - 16 bits - length of encapsulated layer 4 segment in bytes
	- Next Header - indicates the type of the next header (header of the encapsulated segment), for example TCP or UDP (same function as IPv4 protocol field)
	- Hop Limit - the value in this field i decremeneted by 1 by each router that forwards it. If it reaches 0, the packet is discarded. Same function as the IPv4 header's 'TTL' field
- 3rd Row - source address - 128 bits
- 4th row - destination address - 128 bits

### RFC 5952 'A recommendation for IPv6 Address Text Representation'
- leading 0's MUST be removed
- :: must be used to shorten the longest string of all 0 quartets
- If there are two equal-length choices for the ::, use :: to shorten the one on the left.
- Hexadecimal characters MUST be written using lower-case, NOT upper-case

### Why IPv6
- not enough IPv4 addresses available (4,294,967,296 - $2^{32}$ addresses available)
- VLSM, private addresses, and NAT have been used to conserve the use of IPv4 address space
- IPv4 address assignments are controlled by IANA (Internet Assigned Numbers Authority)
	- IANA distributes IPv4 address space to various RIRs (Regional Internet Registries), which then assign them to companies that need them
	- AFRINIC - africa and nearby indian ocean islands
	- APNIC - Asia Pacific Network Information Centre
	- ARIN - American Registry for Internet Numbers
	- LACNIC - Latin American and Carribean Internet Addresses REgistry
	- RIPE NCC - Réseaux IP Européens Network Coordination Centre

### ## Shortening (abbreviating) IPv6 Addresses
1. Remove leading zeros from each hextet
	- 2001:0DB8:000A:001B:20A1:0020:0080:34BD
	- 2001:DB8:A:1B:20A1:20:80:34BD
2. Replace one or more contiguous hextets consisting of zeroes with a double colon (Consecutive quartets of 0s can only be abbreviated once in an IPv6 address)
	- 2001:0DB8:0000:0000:0000:0000:0080:34BD
	- 2001:DB8:80:34BD

### Expanding Shortened IPv6 Addresses
1. Put leading 0s where needed (all quartets should have 4 hexadecimal characters)
	- FE80::2:0:0:FBE8
	- FE80::0002:0000:0000:FBE8
2. If a double colon is used, replace it with all-0 quartets. Make sure there are 8 quartets in total.
	- FE80:0000:0000:0000:0002:0000:0000:FBE8

### Finding the IPv6 Prefix (global unicast address)
- typically an enterprise requesting IPv6 addresses from their ISP will receive a /48 block.
- Typically, IPv6 subnets use a /64 prefix length.
- That means and enterprise has 16 bits to use to make subnets.
- The remaining 64 bits can be used for hosts.
- ex: 2001:0DB8:8B00:0001:0000:0000:0000:0001/64
	- 2001:0DB8:8B00: - 48-bit global routing prefix assigned by the ISP
	- 0001: - 16-bit subnet identifier used by the enterprise to make various subnets
	- 0000:0000:0000:0001 - 64 bit interface identifier - host portion of the address

## IPv6 address configuration

### Modified EUI-64 (Extended Unique Identifier)
- method of converting a MAC address (48 bits) into a 64-bit interface identifier
- The interface identifier can then become the host portion of a /64 IPv6 address.
- How to convert the MAC address:
	1. Divide the MAC address in half
		- 1234 5678 90AB -> 1234 56 | 78 90AB
	2. Insert FFFE in the middle
		1. 1234 56FF FE78 90AB
	3. Invert the 7th bit (open the 2nd hex into binary, 3rd bit from left)
		- 1234 56FF FE78 90AB -> 1034 56FF FE78 90AB

#### Why is the 7th bit inverted?
- MAC addresses are divided into 2 types
	- UAA (Universally Administered Address)
		- Uniquely assigned to the device by the manufacturer
	- LAA (Locally Administered Address)
		- Manually assigned by an admin (with the mac-address command on the interface) or protocol. Doesn't have to be globally unique
- U/L bit (Universal/Local but):
	- UAA has U/L bit = 0
	- LAA has U/L bit = 1
- For IPv6 addresses/EUI-64, the meaning of the U/L bit is reversed
	- LAA has U/L bit = 0
	- UAA has U/L bit = 1

### IPv6 Address Types

#### Global Unicast Address
- IPv6 addresses that are public addresses which can be used over the internet. Must register to use them.
- They are expected to be unique since they are public addresses
- originally defined as 2000::/3 (2000:: to 3FFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF).
- Now defined as all addresses which aren't reserved for other purposes
- the IPv6 network prefix is made up of 2 parts
	- the global routing prefix (usually 48-bits ) - assigned by the ISP
	- subnet identifier (usually 16 bits)- used by the enterprise to make subnets
- the second half of the address is the host portion of the address in IPv4
	- This is the interface identifier - the host portion of the address

#### Unique Local addresses
- private addresses which cannot be used over the internet
- Do not need to register them. Can be used freely within internal networks. Can't be routed over the internet.
- Uses the address block FC00:/7 (FC00:: to FDFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF)
- A later update required the 8th bit to be set to 1, so first two digits must be FD.
- The global ID should be unique so that addresses don't overlap when companies merge
- host parts of a unique local address
	- FD - indicates a unique local address
	- Global ID (usually 40-bit if /64) - should be randomly generated
	- subnet identifier (16 bit if /64) - used by enterprise to make various subnets

#### Link Local Addresses
- Means that these addresses are used for communication within a single link (subnet). Routers will not route packets with a link-local destination IPv6 address.
- automatically generated on IPv6-enabled interfaces
- Uses the address block FE80::/10 (FE80:: to FEBF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF)
- However, the standard states that 54 bits after FE80/10 should be all 0, so you won't see link local addresses beginning with FE9, FEA, or FEB. ONLY FE8
- the interface ID is generated using EUI-64 rules
- Common uses of link-local addreses:
	- routing protocol peerings (OSPFv3 uses link-local addresses for neighbour adjacencies)
	- next-hop addresses for static routes
	- Neighbour Discovery Protocol (NDP, IPv6's replacement for ARP) uses link-local address to function

#### Multicast Addresses
- IPv6 uses range FF00::/8 for multicast (FF00:: to FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFF:FFFF)
##### IPv6 Multicast Scopes
- how far the packet should be forwarded

| Multicast Address    | IPv6 address | Description                                                                                                            |
| -------------------- | ------------ | ---------------------------------------------------------------------------------------------------------------------- |
| interface/node-local | FF01::/16    | packet doesn't leave the local device. Can be used to send traffic to a service within the local device                |
| link-local           | FF02::/16    | the packet remains in the local subnet. Routers will not route the packet between subnets. Does not go beyond a router |
| site-local           | FF05::/16    | The packet can be forwarded by routers. Should be limited to a single physical location. (not forwarded over a WAN).   |
| organizational-local | FF08::/16    | Wider in scope than site-local (an entire company/organisation includes WAN connection)                                |
| global               | FF0E::/16    | No boundaries. Possible to be routed over the internet                                                                 |
##### IPv6 Link-Local multicast scopes
- These IPv6 prefixes are not routable

| Purpose                                       | IPv6 Address | IPv4 Address |
| --------------------------------------------- | ------------ | ------------ |
| All nodes/hosts<br>(functions like broadcast) | FF02::1      | 224.0.0.1    |
| All Routers                                   | FF02::2      | 224.0.0.2    |
| All OSPF routers                              | FF02::5      | 224.0.0.5    |
| ALL OSPF DR/BDRs                              | FF02::6      | 224.0.0.6    |
| ALL RIP Routers                               | FF02::9      | 224.0.0.9    |
| ALL EIGRP Routers                             | FF02::A      | 224.0.0.10   |
#### Anycast Addresses
- new feature of IPv6
- one-to-one-of-many - multiple possible destinations but only on is chosen out of the many candidates
- Multiple routers are configured with the same IPv6 address.
	- they use a routing protocol to advertise the address
	- when hosts send packets to that destination address, routers will forward it to the nearest router configured with that IP address (based on routing metric)
- There is no specific address range for anycast addresses. 
- Use a regular unicast address (global unicast, unique local) and specify it as an anycast address:


#### The unspecified IPv6 address ::
- address = :: (all 0s)
- Can be used when a device doesn't yet know its IPv6 address.
- IPv6 default routes are configured to ::/0
- IPv4 equivalent 0.0.0.0

#### The loopback address ::1
- address = ::1
- Used to test protocol stack on the local device.
- Messages sent to this address are processed within the local device, but not sent to other devices.
- IPv4 equivalent: 127.0.0..0/8

## Neighbour Discovery Protocol
- protocol used with IPv6
- One of its many functions is to replace ARP used with IPv6
	- the ARP-like function of NDP uses ICMPv6 and solicited-node multicast address to learn the MAC address of other hosts. (ARP in IPv4 uses broadcast)
- Another function of NDP is to allow hosts to automatically discover routers on the local network.
- It performs duplicate address detection (DAD) which checks if other devices on the local link are using the same IPv6 address. 


### ARP Replacement Functions

#### Message types used
- Neighbour Solicitation (NS) = ICMPv6 Type 135
- Neighbour Advertisment (NA) = ICMPv6 Type 136

#### Solicited-Node Multicast Address
- an IPv6 solicited-node multicast address is calculated from a unicast address
- ff02:0000:0000:0000:0000:0001:ff + last 6 hex digits of unicast address
	- example: 2001:0db8:0000:0001:0f2a:4fff:fe(a3:00b1)
	- gives us ff02::1:ffa3:b1

#### Neighbour Solicitation (NS) - ARP request equivalent
- the goal is to ask for the MAC address "Who has this IPv6 address"
- Source IP address - sender's address as usual (same as ARP)
- Destination IP address - solicited-node multicast address
- source MAC - sender's MAC address as usual (same as ARP)
- Destination MAC - Multicast MAC based on the solicited-node multicast address
	- IPv6mcast_ff+last 6 hex digits (real address is shown on the right)

#### Neighbour Advertisement (NA) - ARP reply
- Source IP: usual address
- Destination IP: usual address (cause it has learned the address)
- Source MAC: usual address
- Destination MAC: usual address (cause it has learned the address)

### Router Discovery on Local Network
- two messages are used to this process
- allow hosts to automatically discover routers on the local network

#### Router Solicitation (RS) - ICMPv6 type 133
- sent to multicast address FF02::2 (all routers)
- asks all routers on the local link to identify themselves
- Sent when an interface is enabled/host is connected to the network

#### Router Advertisment (RA) - ICMPv6 type 134
- Sent to the multicast address FF02::1 (all nodes)
- the router announces its presence, as well as other information about the link
- These messages are sent in response to RS messages
- They are also sent periodically, even if the router hasn't received an RS

### SLAAC Stateless Address Auto-configuration
- Hosts use RS/RA messages to learn the IPv6 prefix of the local link (i.e. 2001:db8::/64), and then automatically generate an IPv6 address.
- Using the ipv6 address autoconfig command, you don't need to enter the prefix. The device uses NDP to learn the prefix used on the local link
- The device will use EUI-64 to generate the interface ID, or it will be randomly generated (depending on the device maker)

### Duplicate Address Detection (DAD)
- allows hosts to check if other devices on the local link are using the same IPv6 address
- Any time an IPv6-enabled interface initialises (no shutdown commands), or an IPv6 address is configured on an interface (by any method: manual, SLAAC, etc.), it performs DAD.
- DAD uses two messages: NS and NA
- the host will send an NS to its own solicited-node multicast address (IPv6) address. If it doesn't get a reply, it knows the address is unique.
- If it gets a reply, it means another host on the network is already using the address.

| Number | Reason                                                                                                        | Commands                              |
| ------ | ------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| 1      | Configure the IPv6 address automatically<br>without  typing in the prefix as you would for the eui-64 command | R(config-if)# ipv6 address autoconfig |
## IPv6 Static Routing
- works the same as IPv4 routing
- IPv4 and IPv6 routing are two separate processes on the router, and the two routing tables are separate as well
- IPv4 routing is enabled by default, while IPv6 is disabled by default and needs to enabled with ipv6 unicast-routing.
- If IPv6 routing is disabled, the router will be able to send and receive IPv6 traffic, but will not route IPv6 traffic (=will not forward it between networks)
- default gateway is ::/0

### IPv6 Routing Table
- A connected network route is automatically added for each connected network
- A local host route is automatically added for each address configured on the router
- Routes for link-local addresses are not added to the routing table

### Types of Static routes
- Directly attached static route
	- only the exit interface is specified
	- in IPv6, you can't use directly attached static routes if the interface is an Ethernet interface
- Recursive static route:
	- only the next hop is specified
- Full specified static route
	- Both the exit interface and next hop are specified

| Number | Reason                                                 | Commands                                                                           |
| ------ | ------------------------------------------------------ | ---------------------------------------------------------------------------------- |
| 1      | Check the IPv6 routing table                           | R# show ipv6 route                                                                 |
| 2      | Create an IPv6 static route (you can configure the AD) | R(config)# ipv6 route [dst-ip/netmask] {next-hop\| exit-interface [next-hop]} [AD] |
### Link-Local Next-hops
- if you want to use a link-local address as a next-hop, you have to specify both the next hop address and the exit interface.


### IPv6 Configuration Commands
| Number | Reason                                                                                                                                       | Commands                                                       |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| 1      | Allow the router to perform IPv6 routing                                                                                                     | R(config)# ipv6 unicast-routing                                |
| 2      | Enable IPv6 on an interface                                                                                                                  | R(config-if)#ipv6 enable                                       |
| 3      | configure an IPv6 address on an interface. You can abbreviate the address                                                                    | R(config-if)# ipv6 address [address/netmask]                   |
| 4      | Make sure it isn't shutdown                                                                                                                  | R(config-if)# no shutdown                                      |
| 5      | display the configured IPv6 address as well as the link-local address                                                                        | R# show ipv6 interface brief                                   |
| 6      | Configure a static route                                                                                                                     | R(config)# ipv6 route \[destination-ip/netmask] \[next-hop-ip] |
| 7      | Shows the IPv6 group addresses an interface joined and check other things like anycast addresses. Shows the solicited-node multicast address | R#show ipv6 interface [interface]                              |

### IPv6 addresses (EUI-64) command

| Number | Reason                                                                                                              | Commands                                              |
| ------ | ------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| 1      | Configure a host address using the modified EUI-64 by supplying the global routing prefix and the subnet identifier | R(config0if)# ipv6 address [ipv6-addr/netmask] eui-64 |
|        |                                                                                                                     |                                                       |
### IPv6 Anycast Configurations

| Number | Reason                                                                          | Commands                                               |
| ------ | ------------------------------------------------------------------------------- | ------------------------------------------------------ |
| 1      | Configure an anycast address by specifying anycast on a regular unicast address | R(config-if)# ipv6 address [ipv6-addr/netmask] anycast |
|        |                                                                                 |                                                        |
### IPv6 Neighbor Table
| Number | Reason                                                                                                                                                | Commands              |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| 1      | Shows the ipv6 neighbour table (ipv6 doesnt use arp<br>so this is basically its equivalent). The link-layer address shows the neighbour's MAC address | R# show ipv6 neighbor |
|        |                                                                                                                                                       |                       |

## Review Section
1. Shorten the following IP addresses
![[Pasted image 20260924102432.png]]
2. Expand the shortened IPv6 addresses ![[Pasted image 20260924102831.png]]
3. Find the IPv6 prefix
	- 300D:00F2:0B34:2100:0000:0000:1200:0001/56
		- each hextet is 16 bits so the first three hextet is 48
		- and since each character is 4 bits (hex) the prefix is until 1 of :2100
	- 2001:0DB8:8B00:0001:FB89:017B:0020:0011/93
		- the hextet FB89 = (16 x 5) = 80
		- 017B should be where the /93 is, since 0 = 84, 1 = 88, 7 = 92, we need to expand B into binary
		- B = 1011 and since we're only borrowing the leftmost binary we get 2001:0DB8:8B00:0001:FB89:0178::/93 or
		- 2001:DB8:8B00:1:FB89:178::/93
	![[Pasted image 20260924104505.png]]
	4. Modified EUI-64 address (convert MAC addr to EUI-64 Interface identifier)![[Pasted image 20260924111234.png]]


## Side note 
- IPv5 - developed in the late 1970s, but never really introduced for public use (never called IPv5 but value of 5 in the version field of the ip header)
	- sp when the succesor of IPv4 was being developed, it was named IPv6 
- eui-64 packetlife.net