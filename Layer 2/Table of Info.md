
## STP Information

### Election steps
1. lowest bridge priority is the root bridge
	1. check bridge priority inside bridge priority (4096-32768)
	2. Check extended VLAN ID (1-2048)
2. Use MAC address as tie-breaker - lowest MAC wins

### Determine the port roles
1. All ports on the root bridge are designated ports
2. Each remaining SW will choose one of its interfaces to be its root port (forwarding state). Ports across from the root port are always designated ports. Root ports are selected based on:
	1. lowest root cost
	2. lowest neighbour bridge ID
	3. if there are 2 connections, lowest neighbor port ID (128-def)

#### STP Table of  Root Path Costs 
| Speed          | STP cost |
| -------------- | -------- |
| 10 Mbps        | 100      |
| 100 Mbps       | 19       |
| 1 Gbps         | 4        |
| 10 Gbps        | 2        |
| root path cost | 0        |
