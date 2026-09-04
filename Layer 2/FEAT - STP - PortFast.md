#layer2
#STP 
#STPtoolkit
# Caution
- only apply portfast to ports that are connected to end hosts
- when postfast is applied also apply other security measures like loop guard, root guard, bpdu guard, and bpdu filter
## What is Portfast
- it is a spanning tree optional feature that allows a port to move immediately to the forwarding state, bypassing listening and learning

## Why should we use portfast?
- to improve user experience. Users no longer need to wait 30 seconds for the internet to work when they connect their computer.
- The wait is unnecessary because there is no risk of layer 2 loops occurring between a switch/PC.

## Commands
| Number | reason                                                                                                                          | command                                              |
| ------ | ------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| 1      | activates portfast on the interface<br>                                                                                         | SW(config-if)# spanning-tree portfast [edge]<br>     |
| 2      | Enable portfast on all access ports                                                                                             | SW(config-if)# spanning-tree portfast [edge] default |
| 3      | Configure PortFast on a trunk port                                                                                              | SW(config-if)# spanning-tree portfast [edge] trunk   |
| 4      | View detailed STP information about an interface. On a line it shows if the port is in the portfast edge mode by default or not | SW# show spanning-tree interface [interface] detail  |


## Extra notes that may not be part of the CCNA
-  there are two kinds of portfast
	- edge
	- network
- edge is the one used for end hosts
- network is used for a feature called bridge assurance (not a CCNA topic)