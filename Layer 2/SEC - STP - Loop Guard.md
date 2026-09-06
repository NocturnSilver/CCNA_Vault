#layer2
#layer2sec 
#STP 
#STPtoolkit

# Caution
- loop guard and root guard can't be enabled on the same port at the same time.
	- the latest command of either will replace the the first command
## Context
- unidirectional link - a network link where data transmission occurs only in one direction.
	- typically caused by layer 1 issues
- 
## What is Loop Guard
- Protects against loops caused by unidirectional lnks
- A security measure where even if an interface stops receiving BPDUs, it will not start forwarding. The interface will be disabled

## Loop Guard Mechanics
- When a loop guard-enabled port's max age counts down to 0, it enters the broken (loop inconsistent) state
- spanning-tree loopguard default is enabled on all ports
- loop
- A loop-guard enabled port automatically recovers when it starts receiving BPDUs again

## Commands
| Number | Reason                       | Command                                        |
| ------ | ---------------------------- | ---------------------------------------------- |
| 1      | Enable loop guard on a port  | SW(config-if)# spanning-tree guard loop        |
| 2      | Enable loop guard by default | SW(config-if)# spanning-tree loopguard default |
| 3      | Disable loop guard on a port | SW(config-if)# spanning-tree guard none        |
