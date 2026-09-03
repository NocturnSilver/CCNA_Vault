#layer3

## Administrative distances for routing protocols

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

## Commands

| number | reason                                       | command                            |
| ------ | -------------------------------------------- | ---------------------------------- |
| 1      | configure the routing protocol               | R(config)#router rip               |
| 2      | Configure the distance in router config mode | R(config-router)#distance [number] |
| 3      | View the AD of the best route to a network   | R#show ip route                    |
|        |                                              |                                    |

