#layer3 

## Definitions
- Broadcast - messages are delivered to all devices on the local network
- Multicast messages are delivered only to devices that have joined that specific multicast group

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

| Number | Reason                                                                                                                                                                                                                                                                                                                                                                              | Command                             |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| 1      | Enter RIP configuration mode                                                                                                                                                                                                                                                                                                                                                        | R(config)#router tip                |
| 2      | Set the version number                                                                                                                                                                                                                                                                                                                                                              | R(config-router)# version 2         |
| 3      | Disable auto-conversion of address to classful networks                                                                                                                                                                                                                                                                                                                             | R(config-router)# no auto-summary   |
| 4      | Tells router to look for interfaces with an IP address within specified range. Then activate RIP on the interfaces that fall in the range. Then, form adjacencies with connected  RIP neighbours. Then, advertise the network prefix of the interface<br><br>The RIP network command is classful, it will automatically convert to classful networks. No need to enter network mask | R(config-router)# network [ip-addr] |
|        |                                                                                                                                                                                                                                                                                                                                                                                     |                                     |
