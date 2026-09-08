## IEEE Table standards
| Number | Standard | Description            |
| ------ | -------- | ---------------------- |
| 1      | 802.1D   | Spanning Tree protocol |
| 2      |          |                        |
| 3      |          |                        |
## MAC Addresses 

| Number | MAC Address    | Reason                                          |
| ------ | -------------- | ----------------------------------------------- |
| 1      | 0180.c200.000  | Regular STP uses this destination MAC address   |
| 2      | 0100.0ccc.cccd | PVST +BPDUs are sent to destination MAC address |
|        |                |                                                 |

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

## STP Toolkit Information

| STP Tool            | Reaction to receiving BPDUs                                                     |
| ------------------- | ------------------------------------------------------------------------------- |
| PortFast            |                                                                                 |
| PortFast Default    |                                                                                 |
| BPDU guard          | Err-disable the port                                                            |
| BPDU guard Default  | Err-disable the port                                                            |
| BPDU Filter         | Ignores the BPDU                                                                |
| BPDU Filter Default | Turns off PortFast and BPDU Filter. Reverts to acting like an ordinary STP port |
| Root guard          | (superior BPDU) broken (root inconsisten)                                       |
| Root guard default  | (superior BPDU) broken (root inconsisten)                                       |
| Loop guard          |                                                                                 |
| Loop guard Default  |                                                                                 |
|                     |                                                                                 |

