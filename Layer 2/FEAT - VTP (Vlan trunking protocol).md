#layer2
#vlan 

# Caution
- This is one of the few things that has a number of downsides
- does not operate on access ports, only on trunk ports

## What does it do?
- allows you to configure VLANs on a central server switch, and other switches (clients) will synchronize their VLAN database to the server
- 

## Why Use it?
- sad

## Three VTP modes
- server
	- CAN add/modify/delete VLANs
	- WILL synchronize VLAN database with VTP servers with a higher revision number
- client
	- CAN'T add/ modify/delete VLANs
	- WILL synchronize VLAN database with VTP servers with a higher revision number
- transparent
	- CAN add/modify/delete VLANs
	- WONT synchronize VLAN database with VTP servers with a higher revision number

## Revision Number
- every time a VLAN is added/modified/deleted, the revision number increases
- Two methods to reset the VTP revision number to 0 are:
	- Change the VTP mode to transparent 
	- Change the VTP domain to an unused domain
## VTP Versions
- v1 (default ) and v2
	- does not support extended VLAN range (1006-4094)
	- does not maintain its VLAN database in NVRAM
- v3 
	- support the extended VLAN range (1006-4094)
	- maintains its VLAN database in NVRAM