#layer3

## MAC address

| Protocol | syntax         | IP address                       | MAC address                              |
| -------- | -------------- | -------------------------------- | ---------------------------------------- |
| HSRP     | active/standby | V1: 224.0.0.2<br>V2: 224.0.0.102 | V1: 0000.0c07.acxx<br>V2: 0000.0c9F.Fxxx |
| VRRP     | mastery/backup | 224.0.0.18                       | 0000.5e00.01xx                           |
| GLBP     | AVG/AVE        | 224.0.0.102                      | 0007.6400.xxyy                           |
## IP Addresses


### Table of Multicast Addresses

| Multicast Address              | IPv4       | IPv6    |
| ------------------------------ | ---------- | ------- |
| All hosts                      | 224.0.0.1  | FF02::1 |
| All Routers                    | 224.0.0.2  | FF02::2 |
| All OSPF Routers               | 224.0.0.5  | FF02::5 |
| All OSPF DRs                   | 224.0.0.6  | FF02::6 |
| All RIP Routers (except RIPv1) | 224.0.0.9  | FF02::9 |
| All EIGRP routers              | 224.0.0.10 | FF02::A |

### Table of IPv6 Multicast Scopes
| Multicast Address    | IPv6 address | Description                                                                                                         |
| -------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------- |
| interface/node-local | FF01::/16    | packet doesn't leave the local device. Can be used to send traffic to a service within the local device             |
| link-local           | FF02::/16    | the packet remains in the local subnet. Routers will not route the packet between subnets.                          |
| site-local           | FF05::/16    | The packet can be forwarded by routers. Should be limited to a single physical location. (not forwarded over a WAN) |
| organizational-local | FF08::/16    | Wider in scope than site-local (an entire company/organisation)                                                     |
| global               | FF0E::/16    | No boundaries. Possible to be routed over the internet                                                              |


## Dynamic Routing Protocol

| Route Source             | AD  |
| ------------------------ | --- |
| Directly connected route | 0   |
| Static Route             | 1   |
| EIGRP summary route      | 5   |
| eBGP                     | 20  |
| Internal EIGRP           | 90  |
| IGRP                     | 100 |
| OSPF                     | 110 |
| IS-IS                    | 115 |
| RIP                      | 120 |
| External EIGRP           | 170 |
| iBGP                     | 200 |
| Unknown                  | 255 |

### Addresses used by Routing Protocols

| Protocol | Address                | Additional info                                                                                                                                                                                                                                                                         |
| -------- | ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RIPv1    | 255.255.255.255        |                                                                                                                                                                                                                                                                                         |
| RIPv2    | 224.0.0.9              |                                                                                                                                                                                                                                                                                         |
| RIPng    | FF02::9                | Address is used for periodic routing updates                                                                                                                                                                                                                                            |
| EIGRP    | 224.0.0.10<br>FF02::A  | - Multicast IPv4 address. Used to send hello packets and routing updates to all adjacent EIGRP routers on the same segment                                                                                                                                                              |
| OSPF     | 224.0.0.5<br>224.0.0.6 | - Used by all OSPF routers to send and receive Hello Packets for neighbor discovery and maintenance. DR and BDR use this address to send routing updates to all other routers<br>- Used by non-DR and non-BDR routers to send link state updates and ack specifically to the DR and BDR |
|          |                        |                                                                                                                                                                                                                                                                                         |
### Dynamic Routing Protocol Codes
| Code | Definition            | Code | Definition                | Code | Definition                       |
| ---- | --------------------- | ---- | ------------------------- | ---- | -------------------------------- |
| L    | Local                 | M    |                           | su   | IS-IS summary                    |
| C    | Connected             | B    |                           | L1   | IS-IS level-1                    |
| S    | Static                | EX   |                           | L2   | IS-IS level-2                    |
| R    | RIP                   | IA   | OSPF inter area           | ia   | IS-IS inter area                 |
| D    | EIGRP                 | N1   | OSPF NSSA external type 1 | P    | periodic downloaded static route |
| O    | OSPF                  | E2   | OSPF external type 2      | H    | NHRP                             |
| i    | IS-IS                 | O    | ODR                       | l    | LISP                             |
| *    | Candidate <br>default | U    | per-user static route     | +    | replicated route                 |
|      |                       |      |                           | %    | next hope override               |
### ### Dynamic Routing Protocol Metrics

| IGP   | Metric                                               | Explanation                                                                                                                                                               |
| ----- | ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RIP   | Hop count                                            | Each router in the path counts as one hop. Total metric is the total number of hops to the destination. Links of all speeds are equal                                     |
| EIGRP | Metric Based on bandwidth <br>and delay (by default) | Complex formula that can take into account many values. By default, the bandwidth of the slowest link in the route and the total delay of all links in the route are used |
| OSPF  | Cost                                                 | Cost of each link is calculated based on bandwidth. The total metric is the total cost of each link in the route                                                          |
| IS-IS | Cost                                                 | The total metric is the total cost of each link in the route. The cost of each link is not automatically calculated by default. All links have a cost of 10 by default.   |

## FHRP
| Protocol | syntax         | IP address                       | MAC address                              |
| -------- | -------------- | -------------------------------- | ---------------------------------------- |
| HSRP     | active/standby | V1: 224.0.0.2<br>V2: 224.0.0.102 | V1: 0000.0c07.acxx<br>V2: 0000.0c9F.Fxxx |
| VRRP     | mastery/backup | 224.0.0.18                       | 000.5e00.01xx                            |
| GLBP     | AVG/AVE        | 224.0.0.102                      | 0007.6400.xxyy                           |
