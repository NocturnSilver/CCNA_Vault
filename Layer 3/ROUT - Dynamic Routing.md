#layer3

## Definitions
- Autonomous System (AS) 

## What is a Dynamic Routing Protocol
- It is a protocol that automates the addition of routes in the routing table

## How does the Dynamic Routing Protocol do this?
- It learns routes by advertising an IP address to a neighbour

## Types of Dynamic Routing Protocol
- IGP - Interior Gateway Protocol
	- share routes withi a single autonomous system
	- Types of IGP
		- distance vector
			- RIP - routing information protocol
			- EIGRP - Enhanced Interior Gateway Routing Protocol
		- Link state - every router creates a connectivity map of the network (uses more resources - cpu- on the router because more information is shared)
			- OSPF - open shortest path first
			- IS-IS - Intermediate System-to-Intermediate System
- EGP - Exterior Gateway Protocol
	- share routes between different autonomous systems
	- Types of EGP
		- Path Vector
			- BGP - border gateway protocol

## Dynamic Routing Protocol Metric
- lowest metric = the best
- ECMP (equal cost multi-path)
	- dynamically load balances on both paths


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

