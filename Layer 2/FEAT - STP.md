#layer2
#STP 

# Caution

## Definitions
- IEEE 802.1D
- BPDU - bridge protocol data unit

## Context
- most PCs only have a single network interface card (NIC), so they can only be plugged into a single switch. However, important servers typically have multiple NICs, so they can be plugged into multiple switches for redundancy
- spanning tree protocol still uses the term bridge (even if switch is meant)

## What is Spanning Tree Protocol (STP)
- a protocol, IEEE 802.1D, that prevents layer 2 loops by placing redundant ports in a blocking state, essentially disabling the interface
	- these interfaces act as backups that can enter a forwarding state if an active (currently forwarding) interface fails.
	- Interfaces in a forwarding state behave normally. They send and receive all normal traffic.

## Why use Spanning Tree Protocol
- it solves an effect that comes with network redundancy when you want to connect multiple switches together
	- broadcast storm
	- TTL is there for layer 3 to avoid this for packets
	- MAC Address Flapping
		- When frames with the same source MAC address repeatedly arrive on different interfaces, the switch is continuously updating the interface in its MAC address table

## How does STP operate (what are the mechanisms)?
- Selects which ports are forwarding and which ports are blocking
	- this creates a single path to/from each point in the network. This prevents layer 2 loops
- STP-enabled switches send/receive Hello BPDUs out of all interfaces
	- default timer is 2 seconds - which means that the switch will send a hello BPDU out of every interface, once every 2 seconds
	- receiving a Hello BPDU on an interface signals that an interface is connected to another switch (only switches send Hello BPDUs)
- There is a set process that STP uses to determine which ports should be forwarding and which should be blocking
	- Switches use one field in the STP BPDU, the Bridge ID field to elect a root bridge for the network
	- switch with the lowest Bridge ID becomes the root bridge
	- all ports on the root bridge are put in a forwarding state, and other switches in the topology must have a path to reach the root bridge

### Bridge ID and Election process for root bridge
- Bridge ID - 64 bits - used to elect a root bridge for the network
	- Bridge priority - 16 bits - has been update to 2 parts
		- Bridge priority - 4 bits
			- the 4 bits signify (32768, 16384, 8192, 4096)
			- which are $2^{15},2^{14},2^{13},2^{12}$
		- Extended System ID (VLAN ID) - 12 bits
			- the 12 bits signify 1 to 2048
	- MAC address  - 48 bits
- Default bridge ID priority is 32768 = ($2^{15}$) on all switches
	- so by default the MAC address is used as the tie breaker 
	- lowest MAC address becomes the root bridge (ex. A.A.A is lower than B.B.B)
- note: Cisco switches use a version of STP called PVST (per-vlan Spanning Tree)
	- it runs a separate STP instance in each VLAN, so in each VLAN different interfaces can be forwarding/blocking



## Interface States

| state      | mechanics |
| ---------- | --------- |
| blocking   |           |
| listening  |           |
| learning   |           |
| forwarding |           |