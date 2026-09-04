#layer2
#STP 

# Caution

## Definitions and Summary
- IEEE 802.1D
- BPDU - bridge protocol data unit
- Superior BPDU - BPDU with a lower bridge ID
- Bridge ID - used to elect a root bridge (Bridge priority + MAC addr)
- Bridge Priority - the part of bridge ID that is first looked at to determine which should be elected as the root bridge
- MAC address - used as the tie breaker is same bridge priority
## Context
- most PCs only have a single network interface card (NIC), so they can only be plugged into a single switch. However, important servers typically have multiple NICs, so they can be plugged into multiple switches for redundancy
- spanning tree protocol still uses the term bridge (even if switch is meant)
- All interfaces on the root bridge are designated ports.
- Switches only forward BPDUs out of their designated ports (not root or non designated ports)
- PVST only uses ISL trunk encapsulation
- PVST+ supports 802.1Q

## Troubleshooting
- Wireshark can be used to check STP BPDU

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
		- lowest bridge ID gets chosen
		- #### Bridge priority - 4 bits
			- the 4 bits signify (32768, 16384, 8192, 4096)
			- which are $2^{15},2^{14},2^{13},2^{12}$
		- #### Extended System ID (VLAN ID) - 12 bits
			- the 12 bits signify 1 to 2048 from right to left
			- the because the extended system exists, if the native vlan is 1 and the default bridge priority is used, then we actually have 32768 + 1 = 32769
			- the minimum unit of increase and decrease for bridge priority is 4096
	- MAC address  - 48 bits
- Default bridge ID priority is 32768 = ($2^{15}$) on all switches
	- so by default the MAC address is used as the tie breaker 
	- lowest MAC address becomes the root bridge (ex. A.A.A is lower than B.B.B)
- note: Cisco switches use a version of STP called PVST (per-vlan Spanning Tree)
	- it runs a separate STP instance in each VLAN, so in each VLAN different interfaces can be forwarding/blocking

### Which Switch Sends BPDUs?
1. When a switch powers on it assumed it is the root bridge
2. It will only give up its position if it receives a superior BPDU (lower bridge ID)
3. Once the topology has converged and all switches agree on the root bridge, only the root bridge sends BPDUs
4. Other switches in the network will forward these BPDUs, but will not generate their own original BPDUs

### Determine the port roles
1. All ports on the root bridge are designated ports
2. All ports on connected to endhosts are designated ports
3. Each remaining SW will choose one of its interfaces to be its root port (forwarding state). Ports across from the root port are always designated ports. ROOT PORTS are selected based on:
	1. lowest root cost
	2. lowest neighbour bridge ID
	3. if there are 2 connections, lowest neighbor port ID (def = 128) + port number
4. Each remaining collision domain will select ONE interface to be a designated port (forwarding state). The other port in the collision domain will be non-designated (blocking). Designated port selection:
	1. Interface on switch with lowest root cost
	2. Interface on switch with lowest root bridge

#### STP Table of  Root Path Costs 
| Speed          | STP cost |
| -------------- | -------- |
| 10 Mbps        | 100      |
| 100 Mbps       | 19       |
| 1 Gbps         | 4        |
| 10 Gbps        | 2        |
| root path cost | 0        |
## STP Timers

| STP Timer     | Purpose                                                              | Duration         |
| ------------- | -------------------------------------------------------------------- | ---------------- |
| Hello         | frequency root bridge sends Hello BPDUs                              | 2 sec            |
| Forward Delay | How long switch stays in (each) Listening and Learning states        | 15 sec           |
| Max Age       | wait after ceasing to receive Hello BPDUs to change the STP topology | 20sec (10*hello) |
- purpose: make sure loops aren't accidentally created by an interface moving to forwarding state too soon
- Hello
	- notifies that the root bridge exists, and if topology needs to change when it is absent
- Forward Delay
	- How long switch stays in (each) Listening and Learning states to avoid loops.
- Max Age
	- how long an interface waits to change the STP topology after ceasing to receive Hello BPDUs.
	- timer resets if another BPDU is received before timer = 0
	- when timer hits 0 Port roles will be redetermined 
	- if non-designated is selected it may take 50 seconds
		- blocking to listening to learning to forwarding


## Interface States

| STP port state | Stable/<br>Transitional | port role      | send/<br>receive<br>regular network traffic? | SEND/Receive  BPDUs? | learn MAC address? | Default<br>Duration |
| -------------- | ----------------------- | -------------- | -------------------------------------------- | -------------------- | ------------------ | ------------------- |
| blocking       | Stable                  | non-designated | NO                                           | NO/YES               | NO                 | N/A                 |
| listening      | Transitional            | N/A            | NO                                           | YES/YES              | No                 | 15s                 |
| learning       | Transitional            | N/A            | NO                                           | YES/YES              | YES                | 15s                 |
| forwarding     | Stable                  | designated     | YES                                          | YES/YES              | YES                | N/A                 |
| Disabled       | Stable                  | N/A            | NO                                           | NO/NO                | NO                 | N/A                 |

## STP toolkit

| Tool         | Definition                                                                                                                              |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| PortFast     | instantly transitions a switchport to the forwarding state by bypassing listening and learning states                                   |
| UplinkFast   | Reduces STP convergence time to under 1 second after a direct link failure. Moves an alternate/backup port straight to forwarding state |
| BackboneFast | speeds up network recovery from indirect link failures by cutting the 20s max age timer reducing down time from 50s to 30s              |

## STP Commands
| number | reason                                                                                                     | commands                                                   |
| ------ | ---------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| 1      | shows the following:<br>1. interface role<br>2. interface state<br>3. root cost<br>4. port ID priority nbr | SW# show spanning-tree \[vlan] [vlan-num]                  |
| 2      | Configure the STP mode to use on the switch                                                                | SW(config)# spanning-tree mode [mst \| pvst \| rapid-pvst] |
| 3      | Configure STP priority to 24576 to make it the root bridge. If a SW has that priority lower it by 4096     | SW(config)# spanning-tree vlan \[vlan-num] root primary    |
| 4      |                                                                                                            |                                                            |
|        |                                                                                                            |                                                            |
