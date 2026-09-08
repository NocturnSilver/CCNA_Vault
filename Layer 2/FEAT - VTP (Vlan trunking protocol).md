#layer2
#vlan 

# Caution
- This is one of the few things that has a number of downsides
- does not operate on access ports, only on trunk ports
- Revision Number Trap can occur - and old switch with a high revision number that has not been reset will cause all the other servers and clients to overwrite their VLAN Databases.

## What does it do?
- allows you to configure VLANs on a central server switch, and other switches (clients) will synchronize their VLAN database to the server

## Why Use it?
- Manage and sychronize VLAN configurations across multiple switches in a single network domain. Saves time an allows for centralized management.

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
## Commands
| Number | Reason                                                              | Commands                                               |
| ------ | ------------------------------------------------------------------- | ------------------------------------------------------ |
| 1      | Show the VTP configurations                                         | SW# show vtp status                                    |
| 2      | Configure the VTP version                                           | SW(config)# vtp version [vernum]                       |
| 3      | Configure the VTP domain. Must match on all switches to take effect | SW(config)# vtp domain [domain-name]                   |
| 4      | Configure the VTP mode the switch will take                         | SW(config)# vtp mode [server \| client \| transparent] |
| 5      | Configure a password. Must match across all domain if used          | SW(config)# vtp password [password]                    |
