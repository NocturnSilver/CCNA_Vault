## MAC Addresses 

| Number | MAC Address   | Reason                                        |
| ------ | ------------- | --------------------------------------------- |
| 1      | 0180.c200.000 | Regular STP uses this destination MAC address |

## STP Information

### Interface States
| STP port state | Stable/<br>Transitional | port role      | send/<br>receive<br>regular network traffic? | SEND/Receive  BPDUs? | learn MAC address? | Default<br>Duration |
| -------------- | ----------------------- | -------------- | -------------------------------------------- | -------------------- | ------------------ | ------------------- |
| blocking       | Stable                  | non-designated | NO                                           | NO/YES               | NO                 | N/A                 |
| listening      | Transitional            | N/A            | NO                                           | YES/YES              | No                 | 15s                 |
| learning       | Transitional            | N/A            | NO                                           | YES/YES              | YES                | 15s                 |
| forwarding     | Stable                  | designated     | YES                                          | YES/YES              | YES                | N/A                 |
| Disabled       | Stable                  | N/A            | NO                                           | NO/NO                | NO                 | N/A                 |

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

#### STP Table of  Root Path Costs  and STP timers

| Root path      | cost     | ~   | Spanning      | Tree                                                                 | Timers            |
| -------------- | -------- | --- | ------------- | -------------------------------------------------------------------- | ----------------- |
| ~              | ~        | ~   | ~             | ~                                                                    | ~                 |
| Speed          | STP cost | ~   | STP Timer     | Purpose                                                              | Duration          |
| 10 Mbps        | 100      | ~   | Hello         | frequency root bridge sends Hello BPDUs                              | 2 sec             |
| 100 Mbps       | 19       | ~   | Forward Delay | How long switch stays in (each) Listening and Learning states        | 15 sec            |
| 1 Gbps         | 4        | ~   | Max Age       | wait after ceasing to receive Hello BPDUs to change the STP topology | 20 sec (10*hello) |
| 10 Gbps        | 2        | ~   |               |                                                                      |                   |
| root path cost | 0        | ~   |               |                                                                      |                   |


