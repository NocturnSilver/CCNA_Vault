#vlan 
#dot1q

## Context
- domain - group of devices which will receive a frame with the destination MAC address FFFF.FFFF.FFFF
- access port - switchport which belongs to a single VLAN, and usually conntects to end hosts

## What are VLANs (Virtual LANs)
- Used to split up broadcast domains

## How do VLANs work?
- by tagging using a trunk encapsulation protocol to determine for which VLAN it is for

## 802.1q VLAN tag
- inserted in between source address and type/length in an ethernet frame
- Parts of a VLAN tag:
	- TPID (tag protocol identifier)
		- 
	- TCI (tag control information)
		- PCP - priority code point
		- DEI - 
		- VID- VLAN ID

## VLAN Ranges
- normal VLAN range = 1 - 1005
- extended VLAN range = 1006 - 4094


## List of Commands on a switch

### List of commands for VLAN configs

| Number | Reason                       | Command                        |
| ------ | ---------------------------- | ------------------------------ |
| 1      | Create a VLAN                | SW(config)# vlan [vlan-number] |
| 2      | Configure the name of a VLAN | SW(config-vlan)# name [name]   |


### List of commands for trunk ports

| Number | Reason                                             | Command                                                 |
| ------ | -------------------------------------------------- | ------------------------------------------------------- |
| 1      | Display all trunk ports on the SW                  | SW1# show interfaces trunk                              |
| 2      | Configure the VLAN number on a router subinterface | R(config-subif)# encapsulation dot1q vlan-number        |
| 3      | Configure the Native VLAN on a trunk port          | SW1(config-if)# switchport trunk native vlan [vlanID]   |
| 4      | Configure the allowed VLANs on a trunk port        | SW1(config-if)# switchport trunk allowed vlan [vlanIDs] |
| 5      | Configure a switch interface to be an access port  | SW(config-if) switchport mode access                    |
| 6      | Configure the name of a VLAN                       | SW(config-vlan)# name [name]                            |
| 7      | Configure the VLAN of a switch access port         | SW(config-if)# switchport access vlan [vlan-number]     |
| 8      |                                                    |                                                         |
| 9      |                                                    |                                                         |
|        |                                                    |                                                         |
### List of access port commands

| 1   | Configure a switch interface to be an access port | SW(config-if) switchport mode access                |
| --- | ------------------------------------------------- | --------------------------------------------------- |
| 2   | Configure the VLAN of a switch access port        | SW(config-if)# switchport access vlan [vlan-number] |

## List of Commands on a Router

| Number | Reason                                             | Command                                              |
| ------ | -------------------------------------------------- | ---------------------------------------------------- |
| 1      | Configure the VLAN number on a router subinterface | R(config-subif)# encapsulation dot1q vlan-number     |
| 2      | Configure the native VLAN on a router subinterface | R(config-subif)# encapsulation dot1q [vlanID] native |
## List of commands on a multilayer switch
| Number | Reason                                        | Command                |
| ------ | --------------------------------------------- | ---------------------- |
| 1      | Enable layer 3 routing on a multilayer switch | SW(config)# ip routing |

## List of Show commands and Misc

| Number | Reason                                          | Command                                  |
| ------ | ----------------------------------------------- | ---------------------------------------- |
| 1      | Display all trunk ports on the SW               | SW1# show interfaces trunk               |
|        | Reset an interface to its default configuration | R(config)# default interface [interface] |
