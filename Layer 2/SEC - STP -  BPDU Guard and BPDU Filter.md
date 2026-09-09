#layer2
#layer2sec 
#STP
#STPtoolkit

## Summary
- when a BPDU guard enabled port receives a bpdu
	- it is error-disabled (err-disabled)
- Standard STP BPDUs are sent to destination mac address
	- 01800.c200.0000

## What is BPDU Guard
- Automatically disbales a port if it receives a BPDU, protecting the STP topology by preventing unauthorized devices from becoming part of the network
- BPDU guard is a STP enhancement that is typically applied to edge ports that have PortFast enabled
	- because PortFast automatically places ports in a forwarding state, a switch that has been connected to a PortFast enabled port could cause switching loops
	- when BPDU guard detects a bridge protocol data unit (BPDU) on a port, it will place the port into the err-disabled state. BPDU guard is used to disable ports that erroneously receive BPDUs that may cause switch loops.
- BPDU guard prevents a rogue switch from modifying the STP topology.
- Port that is in err-disabled state must be either manually re-enabled or recovered automatically by configuring the 

### BPDU guard commands

| number | reason                                                 | command                                                                     |
| ------ | ------------------------------------------------------ | --------------------------------------------------------------------------- |
| 1      | Configure BPDU guard on an interface                   | SW(config-if)# spanning-tree bpduguard enable                               |
| 2      | Configure BPDU guard on all portfast-enabled interface | SW(config-if)# spanning-tree bpduguard default                              |
| 3      | manually re-enable an err-disabled port                | no shutdown                                                                 |
| 4      | automatically enable an err-disabled port              | # errdusable recovery cause bpduguard<br><br># errdisable recovery interval |


## What is BPDU Filter?
- Stops a port from sending BPDUs or processing received BPDUs


## BPDU Filter Mechanics
- if BPDU Filter has been enabled in interface config mode
	- it ignores the the BPDU it receives
- if BPDU Filter has been enabled in global config mode
	- if the port receives a BPDU PortFast and BPDU Filter are disabled and the port operates as a normal STP port
- - prevents a switch port from sending BPDUs


### BPDU filter Commands

| number | reason                                    | command                                                                     |
| ------ | ----------------------------------------- | --------------------------------------------------------------------------- |
| 1      |                                           | spanning-tree  bpduguard enable                                             |
| 2      |                                           | spanning-tree portfast bpduguard default                                    |
| 3      | manually re-enable an err-disabled port   | no shutdown                                                                 |
| 4      | automatically enable an err-disabled port | # errdusable recovery cause bpduguard<br><br># errdisable recovery interval |
| 5      |                                           |                                                                             |

## ErrDisable Recovery comands

| Number | Reason                                                                     | Command                                  |
| ------ | -------------------------------------------------------------------------- | ---------------------------------------- |
| 1      | Configure ErrDisable recover for a specific command                        | # errdisable recover cause [cause]       |
| 2      | Configure the interval for ErrDisable recovery (300s or 5m is the default) | # errdisable recovery interval [seconds] |
| 3      | Show the errdisable recovery settings                                      | # Show errdisable recover                |
|        |                                                                            |                                          |