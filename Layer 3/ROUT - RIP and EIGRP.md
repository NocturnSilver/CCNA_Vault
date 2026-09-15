#layer3 

## Definitions
- Broadcast - messages are delivered to all devices on the local network
- Multicast messages are delivered only to devices that have joined that specific multicast group

## Troubleshooting
- the AS number, for EIGRP, must match between routers, or they will not form an adjacency and share routing information
- auto-summary, conversion to classful networks, might be enabled/disabled by default depending on router/IOS version.
- the network command will assume a classful address if you don't specify the mask
- EIGRP uses a wildcard mask instead of a regular subnet mask

### Table of addresses for Routing
| Protocol | Address                | Additional info                                                                                                                                                                                                                                                                         |
| -------- | ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RIPv1    | 255.255.255.255        |                                                                                                                                                                                                                                                                                         |
| RIPv2    | 224.0.0.9              |                                                                                                                                                                                                                                                                                         |
| RIPng    | FF02::9                | Address is used for periodic routing updates                                                                                                                                                                                                                                            |
| EIGRP    | 224.0.0.10<br>FF02::A  | - Multicast IPv4 address. Used to send hello packets and routing updates to all adjacent EIGRP routers on the same segment                                                                                                                                                              |
| OSPF     | 224.0.0.5<br>224.0.0.6 | - Used by all OSPF routers to send and receive Hello Packets for neighbor discovery and maintenance. DR and BDR use this address to send routing updates to all other routers<br>- Used by non-DR and non-BDR routers to send link state updates and ack specifically to the DR and BDR |
|          |                        |                                                                                                                                                                                                                                                                                         |

## What is RIP (Routing Information Protocol)
- Industry standard protocol
- distance vector IGP
- Uses hop count as its metric
- maximum hop count is 15
- has three versions
	- RIPv1 and RIPv2 used for IPv4
	- RIPng (RIP Next Generation), used for IPv6
- Uses two message types:
	- Request: To ask RIP-enabled neighbour routers to send their routing tables
	- Response: To send the local router's routing table to neighbouring routers
- By default, RIP-enabled routers will share their routing table every 30 seconds

### RIPv1 Details
- only advertsises classful addresses (class A, class B, and class C)
- does not support VLSM, CIDR
- doesn't include subnet mask information in advertisments (response messages)
	- 10.1.1.0/24 will become 10.0.0.0 (Class A address assumed to be /8)
	- 12.16.192.0/18 will become 172.16.0.0 (Class B address, so assumed to be /16)
	- 192.168.1.4/30 will become 192.168.1.0 (Class C address, so assumed to be /24)
- messages are broadcast to 255.255.255.255

### RIPv2 Details
- supports VLSM, CIDR
- includes subnet mask information in advertisements
- messages are multicast to 224.0.0.9

### RIP Config Commands
- Use version 2 since version 1 doesn't support CIDR and VLSM

| Number | Reason                                                                                                                                                                                                                                                                                                                                                                              | Command                                                                                        |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| 1      | Enter RIP configuration mode                                                                                                                                                                                                                                                                                                                                                        | R(config)#router rip                                                                           |
| 2      | Set the version number                                                                                                                                                                                                                                                                                                                                                              | R(config-router)# version 2                                                                    |
| 3      | Disable auto-conversion of address to classful networks                                                                                                                                                                                                                                                                                                                             | R(config-router)# no auto-summary                                                              |
| 4      | Tells router to look for interfaces with an IP address within specified range. Then activate RIP on the interfaces that fall in the range. Then, form adjacencies with connected  RIP neighbours. Then, advertise the network prefix of the interface<br><br>The RIP network command is classful, it will automatically convert to classful networks. No need to enter network mask | R(config-router)# network [ip-addr]                                                            |
| 5      | Tells the router to stop sending RIP advertisements out of the specified interface. Use on interfaces which don't have any RIP neighbours and loopback                                                                                                                                                                                                                              | R(config-router)#passive-interface [interface]                                                 |
| 6      | Advertise a default route into RIP. Setup the default route first then perform the 2nd step                                                                                                                                                                                                                                                                                         | R(config)# ip route 0.0.0.0 0.0.0.0 [next-hop]<br><br>R(config)# default-information originate |
| 7      | Shows the routing protocol information                                                                                                                                                                                                                                                                                                                                              | R#show ip protocols                                                                            |
| 8      | configure the maximum paths that RIP will insert for the same destination in the routing table if they have the same metric. This if for ECMP load-balancing                                                                                                                                                                                                                        | R(config-router)# maximum-paths <br><1-32>                                                     |
| 9      | set the administrative distance of RIP                                                                                                                                                                                                                                                                                                                                              | R(config-router)# distance <1-255>                                                             |
## What is EIGRP (Enhanced Gateway Routing Protocol)
- Was Cisco Proprietary, but was published openly so other vendors can implement it on their equipment
- Much faster than RIP in reacting to changes in the network
- Does not have 15 hop count limit of RIP
- Sends messages to multicast address 224.0.0.10
- Is the only IGP that can perform unequal-cost load balancing (by default it performs ECMP load-balancing over 4 paths like RIP)
- Has two separate AD values: 90 for internal routes, 170 for external routes. Internal routes are normal EIGRP routes while external routes are routes from outside EIGRP
- Uses the letter D for ip route codes

### Wildcard masks
- basically and inverted subnet mask
- all 1's in the subnet mask are 0 in the equivalent wildcard mask and all 0's in the subnet mask are 1 in the equivalent wildcard mask.
- Therefore, the trick is to identify the wildcard mask then expand it in binary then turn all 0's to 1's and 1's to 0's
	- trick is to subtract all octet values from 255 to get the wildcard mask
- 0's in the wildcard mask must match and 1's don't have to match when we expand both addresses in binary and align them

### EIGRP metric
- the bandwidth of the slowest link in the route and the total delay of all links in the route are used
- K1 (interface bandwidth) and K3 (interface delay) are used to calculate the metric by default

### Router ID order of priority
- Router has a unique router ID which identifies it within the AS for EIGRP and OSPF
-  Not actually an IP address just a dotted 32 bit address
	- we can set it to 1.1.1.1 or something else
- The order of priority is shown below:
1. Manual configuration
2. Highest IP address on a loopback interface
3. Highest IP address on a physical interface

### Table of Commands for EIGRP
| Number | Reason                                                                                                                                                       | Command                                                                                        |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| 1      | Enter EIGRP configuration mode. the AS number must match between routers, or they will not form an adjacency and share route information                     | R(config)#router eigrp [AS-number]                                                             |
| 2      | Disable auto-conversion of address to classful networks. Might be disabled/enabled by default depending on the router/IOS version                            | R(config-router)# no auto-summary                                                              |
| 3      |                                                                                                                                                              |                                                                                                |
| 4      | Tells router to look for interfaces with an IP address within specified range. Then activate EIGRP on the interfaces that fall in the range.                 | R(config-router)# network [ip-addr] [wildcard-mask]                                            |
| 5      | Tells the router to stop sending RIP advertisements out of the specified interface. Use on interfaces which don't have any RIP neighbours and loopback       | R(config-router)#passive-interface [interface]                                                 |
| 6      | Advertise a default route into RIP. Setup the default route first then perform the 2nd step                                                                  | R(config)# ip route 0.0.0.0 0.0.0.0 [next-hop]<br><br>R(config)# default-information originate |
| 7      | Shows the routing protocol information                                                                                                                       | R#show ip protocols                                                                            |
| 8      | configure the maximum paths that RIP will insert for the same destination in the routing table if they have the same metric. This if for ECMP load-balancing | R(config-router)# maximum-paths <br><1-32>                                                     |
| 9      | set the administrative distance of RIP                                                                                                                       | R(config-router)# distance <1-255>                                                             |
| 10     | Manually set the EIGRP router ID. Should be in the format of 4 binary octets                                                                                 | R(config-router)# eigrp router-id [A.B.C.D]                                                    |