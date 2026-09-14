#layer3

## Definitions
- Autonomous System (AS) -
- Network route - a route to a network/subnet (mask length < /32)
- Host route - A route to a specific host (/32)
- distance vector (IGRP) - the term comes from the idea that the routers only learn the 'distance' (metric) and 'vector' (direction, the next hop-router) of each route
- link state (IGRP) - 

## What is a Dynamic Routing Protocol
- It is a protocol that automates the addition of routes in the routing table

## Why Use Dynamic Routing
- To allow the network to adapt when there are topology changes for changes in layer 3 addresses and routes.
## How does the Dynamic Routing Protocol do this?
- routers can use dynamic routing protocols to advertise information about the routes they know to other routers.
	- They form adjacencies/neighbour relationships/ neighborships with adjacent routers to exchange information.
	- if multiple routes to a destination are learned, the router determines which route is superior and adds it to the routing table. It uses the 'metric' of the route to decide which is superior (lower metric = superior)
- It learns routes by advertising an IP address to a neighbour

## Types of Dynamic Routing Protocol
- IGP - Interior Gateway Protocol
	- share routes within a single autonomous system (AS), which is a single organization (ie. a company)
	- Algorithm type
		- distance vector 
			- RIP - routing information protocol
			- EIGRP - Enhanced Interior Gateway Routing Protocol
		- Link state -
			- OSPF - open shortest path first
			- IS-IS - Intermediate System-to-Intermediate System
- EGP - Exterior Gateway Protocol
	- used to share routes between different autonomous systems
	- Types of EGP
		- Path Vector
			- BGP - border gateway protocol

### Description of Algorithm types
- Distance Vector
	- also known as 'routing by rumor' which means the router doesn't know about the network beyond its neighbours - invented before link state protocols.
	- Distance vector protocols operate by sending their known destination networks and their metric to reach said destination networks. 
- Link-State 
	-  every router creates a connectivity map of the network (uses more resources - cpu- on the router because more information is shared). 
	- It advertises information about its interfaces (connected networks) to its neighbours. 
	- These advertisements are passed along to other routers, until all routers in the network develop the same map of the network.
	- each router independently uses this map to calculate the best routes to each destination
	- Link state protocols use more resources (CPU) on the router, because more information is shared
	- However, link state protocols tend to be faster in reacting to changes in the network than distance vector protocols
## Dynamic Routing Protocol Metrics
- the metric value is shown in the format \[AD/metric]
- A router's route table contains the best route to each destination network it knows about
- It uses a metric values of the routes to determine which is best if there are two routes with the same destination
	- lowest metric = the best
	- If the router learns two (or more) routes via the same routing protocol to the same destination (same network address, same subnet mask) with the same metric, both will be added to the routing table. Traffic will be load-balanced over both routes.
- Each routing protocol uses a different metric to determine which route is the best.
- ECMP (equal cost multi-path)
	- dynamically load balances on both paths

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
### Dynamic Routing Protocol Metrics


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

