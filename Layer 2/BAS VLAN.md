#vlan 
#dot1q

# Caution
- for safety purposes, its best to change the native VLAN to an unused VLAN
## Context
- domain - group of devices which will receive a frame with the destination MAC address FFFF.FFFF.FFFF
- access port - untagged ports- switchport which belongs to a single VLAN, and usually conntects to end hosts
- trunks ports - aka tagged ports - 

## What are VLANs (Virtual LANs)
- Used to split up layer 2 broadcast domains
- VLANs that exist by default on a Cisco switch are 1,1002-105
- Cisco switch interfaces are in VLAN 1 by default

## How do VLANs work?
- switches will tag frames sent out of trunk ports with a VLAN number
- A layer 2 switch will not forward traffic between VLANs (inter-vlan routing)
- by tagging using a trunk encapsulation protocol to determine for which VLAN it is for

## Native VLANs
- The default native VLAN is VLAN 1 on all trunk ports
- The switch does not add an 802.1Q tag to frames in the native VLAN over a trunk link
- when a switch receives an untagged frame on a trunk port, it assumed the frame belongs to the native VLAN

## Different types of switchports
- access ports
- trunk ports
	- switchportst that carry multiple VLANs

## Trunking Protocols (encapsulation type)

### Inter-Switch Link (ISL)
- Cisco proprietary trunking protocol

### 802.1q VLAN tag
- inserted in between source address and type/length in an ethernet frame
- The 802.1Q tag is 4 bytes in length
- Parts of a VLAN tag:
	- TPID (tag protocol identifier) - 16 bits
		- 0x8100 -  802.1q- standard default for VLAN tagging 
		- 0x88A8 -  802.1aD (QinQ) - Used for outer service provider tag (S-TAG) in stacked VLANs
		- 0x9100 or 0x9200 - 
	- TCI (tag control information)
		- PCP - priority code point (3 bits)
			- used for class of service
		- DEI  - drop eligible indicator (1 bit)
			- Indicates that a frame can be dropped if the network is congested
		- VID- VLAN ID - ()
			- identifies the VLAN the frame belongs to

## VLAN Ranges
- VLANs 0 and 4095 are reserved and cannot be used
- normal VLAN range = 1 - 1005
- extended VLAN range = 1006 - 4094

## Inter-VLAN Routing

### Router with multiple interfaces for intervlan routing
- uses each interface to correspond to a vlan
- need to configure each PC to use the interface address as the default gateway
- each interface on the switch and router that plays a part in the vlan must be assigned a vlan
### Router on a Stick (ROAS)
- Using a router and creating subinterfaces for each VLAN

### Switch Virtual Interface (SVI)
- are shutdown by default

## List of Commands on a switch

### List of commands for VLAN configs

| Number | Reason                       | Command                        |
| ------ | ---------------------------- | ------------------------------ |
| 1      | Create a VLAN                | SW(config)# vlan [vlan-number] |
| 2      | Configure the name of a VLAN | SW(config-vlan)# name [name]   |


### List of commands for trunk ports

| Number | Reason                                             | Command                                                             |
| ------ | -------------------------------------------------- | ------------------------------------------------------------------- |
| 1      | Configure the interface as a trunk port            | SW(config-if)# switchport mode trunk                                |
| 2      | Configure the encapsulation type on a trunk port   | SW(config-if)# switchport trunk encapsulation  [encapsulation-type] |
| 3      | Configure the VLAN number on a router subinterface | R(config-subif)# encapsulation dot1q vlan-number                    |
| 4      | Configure the Native VLAN on a trunk port          | SW1(config-if)# switchport trunk native vlan [vlanID]               |
| 5      | Configure the allowed VLANs on a trunk port        | SW1(config-if)# switchport trunk allowed vlan [vlanIDs]             |

### List of access port commands

| 1   | Configure a switch interface to be an access port | SW(config-if) switchport mode access                |
| --- | ------------------------------------------------- | --------------------------------------------------- |
| 2   | Configure the VLAN of a switch access port        | SW(config-if)# switchport access vlan [vlan-number] |

## List of Commands on a Router

| Number | Reason                                             | Command                                              |
| ------ | -------------------------------------------------- | ---------------------------------------------------- |
| 1      |                                                    |                                                      |
| 2      | Configure the native VLAN on a router subinterface | R(config-subif)# encapsulation dot1q [vlanID] native |
| 3      | Create a subinterface                              | R(config)# interface [interface with decimal]        |
| 4      | Configure the VLAN number on a router subinterface | R(config-subif)# encapsulation dot1q vlan-number     |
|        | Configure an ip address for the subinterface       | R(config-subif)# ip address [ipaddr] [netmask]       |
## List of commands on a multilayer switch
| Number | Reason                                        | Command                      |
| ------ | --------------------------------------------- | ---------------------------- |
| 1      | Enable layer 3 routing on a multilayer switch | SW(config)# ip routing       |
| 2      | Configure a swtich interface as a router port | SW(config-if)# no switchport |

## List of Show commands and Misc

| Number | Reason                                          | Command                                  |
| ------ | ----------------------------------------------- | ---------------------------------------- |
| 1      | Display all trunk ports on the SW               | SW1# show interfaces trunk               |
|        | Reset an interface to its default configuration | R(config)# default interface [interface] |
