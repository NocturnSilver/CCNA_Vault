#STP 

| IEEE Standard                       | Description                                                                                                                                                             | Cisco Equivalent                                | Description                                                                                                                        |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **802.1D - STP**                    | - all VLANs share one STP instance<br>- Therefore, cannot load balance                                                                                                  | Per-VLAN Spanning Tree (PVST)                   | - Cisco's upgrade to 802.1D<br>- Each VLAN has its own STP instance<br>- Can load balance by blocking different ports in each VLAn |
| **802.1W - Rapid Spanning Tree**    | - Much faster at converging/adapting to network changes than 802.1D<br>- All VLANs share one STP instance<br>- Cannot load balance                                      | Rapid Per-VLAN Spanning Tree Plus (Rapid PVST+) | - Cisco's upgrade to 802.1W<br>- Each VLAN has its own STP instance<br>- Can load balance by blocking different ports in each VLAN |
| **802.1S - Multiple Spanning Tree** | - Uses modified RSTP mechanics<br>- can group multiple VLANs into different instances (ie. VLANs 1-5 in instance 1, VLANs 6-10 in instance 2) to perform load balancing | None                                            | None                                                                                                                               |

#### STP frame
| STP Version         | Protocol Identifier | Protocol Version Identifier |
| ------------------- | ------------------- | --------------------------- |
| Spanning Tree       | 0x0000              | 0                           |
| Rapid spanning Tree | 0x0000              | 2                           |

## What is RSTP
- not a timer base based Spanning tree algorithm like 802.1D
- the heart of the protocol is a new bridge-bridge handshake mechanism, which allows ports to move directly to forwarding

## Why RSTP
- it offers an improvement over the 30s or more that 802.1D takes to move a link to forwarding.

## RSTP Mechanics

### Similarities between STP and RSTP
- Serves the same purpose - blockes specific ports to prevent layer 2 loops
- RSTP elects a root bridge with the same rules as STP
- RSTP elects root ports with the same rules as STP
- RSTP elects designated ports with the same rules as STP

### Differences between STP and RSTP
- In rapid STP, all switches originate and send their own BPDUs from their designated ports.
- for classic STP the BPDU flags is 0x00 
	- leftmost bit = 0 - topology chance acknowledgement (no)
	- rightmost bit = 0 - topology chance (no)
- for Rapid STP the BPDU flags is 0x3x
	- 0... .... - toology change acknowledgement: No
	- .0.. .... - Agreement: No
	- ..1. .... - Forwarding: Yes
	- ....1.... - Learning: yes
	- .... 11.. - Port Role: Designated (3)
	- .... ..0. - Proposal: No
	- .... ...0 - Topology Change: No

#### Table of costs
| Speed    | STP Cost | RSTP Cost     |
| -------- | -------- | ------------- |
| 10 Mbps  | 100      | $2\times10^6$ |
| 100 Mbps | 19       | $2\times10^5$ |
| 1 Gbps   | 4        | $2\times10^4$ |
| 10 Gbps  | 2        | $2\times10^3$ |
| 100 Gbps | -        | $2\times10^2$ |
| 1 Tbps   | -        | $2\times10^1$ |
#### Table of Port States

| STP port state | Stable/<br>Transitional | port role      | send/<br>receive<br>regular network traffic? | SEND/Receive  BPDUs? | learn MAC address? |
| -------------- | ----------------------- | -------------- | -------------------------------------------- | -------------------- | ------------------ |
| Discarding     | Stable                  | non-designated | NO                                           | NO/YES               | NO                 |
| learning       | Transitional            | N/A            | NO                                           | YES/YES              | YES                |
| forwarding     | Stable                  | designated     | YES                                          | YES/YES              | YES                |

#### Port Roles
| Role            | description                                                                                                                                                                                                                                       |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| root port       | the port that is closest to the root bridge (in cost) becomes the root port for the switch                                                                                                                                                        |
| designated port | The port on a segment (collision domain) that sends the best BPDU is that segment's designated port (only one per segment)                                                                                                                        |
| alternate port  | - discarding port  that receives a superiod BPDU from another switch. <br>- Functions as Backup root port<br>- if root port fails, switch can immediately move its best alternate port to forwarding                                              |
| backup port     | -  discarding port that receives a superior BPDU from ANOTHER interface on the same switch (same switch)<br>- happens when two interfaces are connected to the same collision domain (via a hub)<br>- Functions as a backup for a designated port |

### Features
- uplinkFast 
	- classic STP feature that allows the immediate move to forwarding state (built-in for RSTP)
- BackboneFast
	- Allows a switch to expre the made age timer on its interface and rapidly forward the superior BPDU received from one interface to another that is receiving an inferior BPDU.
- PortFast
	- edge ports dont need a waiting time to be up/up

### RSTP Timers
- all switches running will send their own BPDUs every hello time (2 seconds)
- Switches 'age' the BPDU information much more quickly.
	- Rapid STP considers a neighbor lost after 3 miss Hellos (6s) compared to STP which is 10 hello intervals (20s)
	- It then flushes all MAC addresses learned on that interface

### RSTP Link Types
- Edge - a port that is connected to an end host. Moves directly to forwarding without negotiation.
- Point-to-point - a direct connection between two switches.
- Shared - a connection to a hub. Must operate in half-duplex mode.

## Commands
| Number | Reason                                                                                                   | Commands                                              |
| ------ | -------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| 1      | Shows stp detials like protocol, role, sts, cost, port cost, type, etc.                                  | SW# show spanning-tree                                |
| 2      | Configure rapid PVST on a switch                                                                         | SW(config)# spanning-tree mode rapid-pvst             |
| 3      | Configures the interface as point-to-point (it should be detectedd)                                      | SW(config-if)# spanning-tree link-type point-to-point |
| 4      | Configure the interface as a shared port. Shared ports connect to another siwtch (or switches) via a hub | SW(config-if)# spanning-tree link-type shared         |
