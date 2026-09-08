#layer2
#layer2sec 
#STP 
#STPtoolkit

# Caution
- loop guard and root guard can't be enabled on the same port at the same time.
	- the latest command of either will replace the the first command

## Troubleshooting
- Unidirectional links are caused by layer 1 issues such as damaged cables and faulty connectors or transceivers (e.g. SFP modules)
	- more common with fiber-optic cables than copper UTP.
		- Fiber optic connection typically use two separate fibers
		- if one fiber is damaged, it can disrupt data flow in one direction while the other remains unaffected.
		- if there is a physical problem with either fibre, the devices should be able to detect it and disable their interfaces.
		- if there is failure to detect physical problem, it could result in unidirectional links
		

## Context
- unidirectional link - a network link where data transmission occurs only in one direction.
	- typically caused by layer 1 issues such as damaged cables and faulty connectors
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
