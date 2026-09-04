#layer2
#layer2sec 
#STP
#STPtoolkit

## BPDU Guard
- BPDU guard is a STP enhancement that is typically applied to edge ports that have PortFast enabled
	- because PortFast automatically places ports in a forwarding state, a switch that has been connected to a PortFast enabled port could cause switching loops
	- when BPDU guard detects a bridge protocol data unit (BPDU) on a port, it will place the port into the err-disabled state. BPDU guard is used to disable ports that erroneously receive BPDUs that may cause switch loops.
- BPDU guard prevents a rogue switch from modifying the STP topology.
- Port that is in err-disabled state must be either manually re-enabled or recovered automatically by configuring the 

### Command
| number | reason                                    | command                                                                     |
| ------ | ----------------------------------------- | --------------------------------------------------------------------------- |
| 1      |                                           |                                                                             |
| 2      |                                           |                                                                             |
| 3      | manually re-enable an err-disabled port   | no shutdown                                                                 |
| 4      | automatically enable an err-disabled port | # errdusable recovery cause bpduguard<br><br># errdisable recovery interval |
| 5      |                                           |                                                                             |