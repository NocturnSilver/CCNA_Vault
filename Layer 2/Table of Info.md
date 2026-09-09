#layer2 
## IEEE Table standards
| Number | Standard | Description                                    |
| ------ | -------- | ---------------------------------------------- |
| 1      | 802.1AB  | Link Layer Discovery Protocol (LLDP)           |
| 2      | 802.1D   | Spanning Tree protocol                         |
| 3      | 802.12   | Rapid Spanning Tree Protocol                   |
| 4      | 802.1s   | Multiple Spanning Tree protocol                |
| 5      | 802.3    | Wired Ethernet Networks                        |
| 6      | 802.3ad  | Link Aggregation Protocol (LACP -etherchannel) |
| 7      |          |                                                |
| 8      |          |                                                |
|        |          |                                                |
|        |          |                                                |
|        |          |                                                |
## MAC Addresses 

| Number | MAC Address    | Reason                                          |
| ------ | -------------- | ----------------------------------------------- |
| 1      | 0180.c200.000  | Regular STP uses this destination MAC address   |
| 2      | 0100.0ccc.cccd | PVST +BPDUs are sent to destination MAC address |
| 3      | 0100.0ccc.cccc | CDP                                             |
| 4      | 0180.c200.000e | LLDP                                            |

## STP Information

### STP versions

| IEEE Standard                       | Description                                                                                                                                                             | Cisco Equivalent                                | Description                                                                                                                        |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **802.1D - STP**                    | - all VLANs share one STP instance<br>- Therefore, cannot load balance                                                                                                  | Per-VLAN Spanning Tree (PVST)                   | - Cisco's upgrade to 802.1D<br>- Each VLAN has its own STP instance<br>- Can load balance by blocking different ports in each VLAn |
| **802.1W - Rapid Spanning Tree**    | - Much faster at converging/adapting to network changes than 802.1D<br>- All VLANs share one STP instance<br>- Cannot load balance                                      | Rapid Per-VLAN Spanning Tree Plus (Rapid PVST+) | - Cisco's upgrade to 802.1W<br>- Each VLAN has its own STP instance<br>- Can load balance by blocking different ports in each VLAN |
| **802.1S - Multiple Spanning Tree** | - Uses modified RSTP mechanics<br>- can group multiple VLANs into different instances (ie. VLANs 1-5 in instance 1, VLANs 6-10 in instance 2) to perform load balancing | None                                            | None                                                                                                                               |

### STP (Interface) Port States
| STP port state | Stable/<br>Transitional | port role      | send/<br>receive<br>regular network traffic? | SEND/Receive  BPDUs? | learn MAC address? | Default<br>Duration |
| -------------- | ----------------------- | -------------- | -------------------------------------------- | -------------------- | ------------------ | ------------------- |
| blocking       | Stable                  | non-designated | NO                                           | NO/YES               | NO                 | N/A                 |
| listening      | Transitional            | N/A            | NO                                           | YES/YES              | No                 | 15s                 |
| learning       | Transitional            | N/A            | NO                                           | YES/YES              | YES                | 15s                 |
| forwarding     | Stable                  | designated     | YES                                          | YES/YES              | YES                | N/A                 |
| Disabled       | Stable                  | N/A            | NO                                           | NO/NO                | NO                 | N/A                 |

### RSTP Port States
| STP port state | Stable/<br>Transitional | port role      | send/<br>receive<br>regular network traffic? | SEND/Receive  BPDUs? | learn MAC address? |
| -------------- | ----------------------- | -------------- | -------------------------------------------- | -------------------- | ------------------ |
| Discarding     | Stable                  | non-designated | NO                                           | NO/YES               | NO                 |
| learning       | Transitional            | N/A            | NO                                           | YES/YES              | YES                |
| forwarding     | Stable                  | designated     | YES                                          | YES/YES              | YES                |

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
3. Remaining collision domains pairs will select one to be designated and the other to be non-designated. Designated port selection:
	1. interface on switch with lowest root cost
	2. interface on switch with lowest bridge ID

#### STP timers

| Spanning       | Tree                                                                 | Timers            |
| -------------- | -------------------------------------------------------------------- | ----------------- |
| STP Timer      | Purpose                                                              | Duration          |
| Hello          | frequency root bridge sends Hello BPDUs                              | 2 sec             |
| Forward Delay  | How long switch stays in (each) Listening and Learning states        | 15 sec            |
| Max Age        | wait after ceasing to receive Hello BPDUs to change the STP topology | 20 sec (10*hello) |
| 10 Gbps        | 2                                                                    | ~                 |
| root path cost | 0                                                                    | ~                 |
#### STP and RSTP Root Costs

| Speed     | STP Cost | RSTP Cost     |
| --------- | -------- | ------------- |
| 10 Mbps   | 100      | $2\times10^6$ |
| 100 Mbps  | 19       | $2\times10^5$ |
| 1 Gbps    | 4        | $2\times10^4$ |
| 10 Gbps   | 2        | $2\times10^3$ |
| 100 Gbps  | -        | $2\times10^2$ |
| 1 Tbps    | -        | $2\times10^1$ |
| root cost | 0        | -             |

### STP Toolkit Information

| STP Tool            | Reaction to receiving BPDUs (not receiving for loop guard)                      |
| ------------------- | ------------------------------------------------------------------------------- |
| PortFast            | Reverts back into an ordinary STP port                                          |
| PortFast Default    | Reverts back into an ordinary STP port                                          |
| BPDU guard          | Err-disable the port                                                            |
| BPDU guard Default  | Err-disable the port                                                            |
| BPDU Filter         | Ignores the BPDU                                                                |
| BPDU Filter Default | Turns off PortFast and BPDU Filter. Reverts to acting like an ordinary STP port |
| Root guard          | (superior BPDU) broken (root inconsisten)                                       |
| Root guard default  | (superior BPDU) broken (root inconsisten)                                       |
| Loop guard          | (does not receive BPDU) broken (loop inconsistent)                              |
| Loop guard Default  | (does not receive BPDU) broken (loop inconsistent)                              |
|                     |                                                                                 |

## EtherChannel Information
- 1st prob: Solves the problem of oversubscription
	- solution: add more wires
- 2nd prob: STP happens if we add more wires
	- solution: etherchannel - treat as one wire
- PAgP and LACP(802.3ad) dynamically negotiates and maintains etherchannel like how DTP does for trunking

### EtherChannel Load-Balancing
- depends on the flow. the flow is a single connection between the two switches
- We want a single class of information into one flow else reassembly of information becomes a problem
- Calculation of which interface should be used depends on the input which can be set as either
	- src - ip/mac
	- dst - ip/mac
	- src and dst - ip/mac

### Table of  EtherChannel Modes

| EtherChannel conf | Mode      | Description                                   |
| ----------------- | --------- | --------------------------------------------- |
| LACP              | active    | enable LACP unconditionally                   |
| LACP              | passive   | Enable LACP only if a PAgP device is detected |
| PAgP              | auto      | Enable PAgP only if a PAgP device is detected |
| PAgP              | desirable | enable PAgP unconditionally                   |
| N/A               | on        | enable etherchannel only                      |
|                   |           |                                               |
### Commands 
- etherchannel is used for show commands
- port-channel - used in config
- channel-group - used in interface config - sets the grp and mode
- channel-protocol - used in interface config - sets the protocol

| Number | Reason                                                                                                                                      | Commands                                                                                           |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| 1      | Show the configurations of the load-balancing configuration. Flags are shown for indicators.                                                | SW# show etherchannel load-balance                                                                 |
| 2      | Show the the group, protocol, ports, and port-channel. the most common command to use to see config.                                        | SW# show etherchannel summary                                                                      |
| 3      | shows the number of ports and protocol. It shows the channel group mode which cant be shown in the summary                                  | SW# show etherchannel port-channel                                                                 |
| 4      | Configure the load balancing in config mode. Types are written in the format [src\|dst]-[dst]-[ip\|mac]                                     | SW(config)# port-channel load-balance [type]                                                       |
| 5      | Configures the channel mode for a channel group. it is a good idea to set all at once since all of them should have the same configurations | SW(config)# interface range [int-range]<br><br>SW(config-if)# channel-group [groupnum] mode [mode] |
| 6      | Configure the protocol to be used by the interfaces part of the EtherChannel                                                                | SW(config)# channel-protocol [lacp \| pagp]                                                        |
| 7      | Enter interface config for the port channel                                                                                                 | SW(config)# interface port-channel [grp-num]                                                       |
| 8      | Configure, inside the port-channel interface, trunk encapsulation                                                                           | SW(config-if)# switchport trunk encapsulation dot1q                                                |
| 9      | Configure the switchport mode to trunk                                                                                                      | SW(config-if)# switchport mode trunk                                                               |
| 10     | Check if the interface is a trunk                                                                                                           | # show interfaces trunk                                                                            |
