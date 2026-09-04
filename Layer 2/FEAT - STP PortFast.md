#layer2
#STP 
#STPtoolkit
# Caution
- only apply portfast to ports that are connected to end hosts
- when postfast is applied also apply other security measures like loop guard, root guard, bpdu guard, and bpdu filter
## What is Portfast
- it is a spanning tree optional feature that allows a port to move immediately to the forwarding state, bypassing listening and learning

## Commands
| Number | reason                                                 | command                                        |
| ------ | ------------------------------------------------------ | ---------------------------------------------- |
| 1      | activates portfast on the interface<br>                | SW(config-if)# spanning-tree portfast<br>      |
| 2      | Enable portfast on all access ports                    | SW(config-if)# spanning-tree portfast default  |
| 3      | Configure BPDU guard on an interface                   | SW(config-if)# spanning-tree bpduguard enable  |
| 4      | Configure BPDU guard on all portfast-enabled interface | SW(config-if)# spanning-tree bpduguard default |
