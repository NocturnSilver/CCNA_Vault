#layer3

# Table of Useful Values

| Protocol | syntax         | IP address                       | MAC address                              |
| -------- | -------------- | -------------------------------- | ---------------------------------------- |
| HSRP     | active/standby | V1: 224.0.0.2<br>V2: 224.0.0.102 | V1: 0000.0c07.acxx<br>V2: 0000.0c9F.Fxxx |
| VRRP     | mastery/backup | 224.0.0.18                       | 000.5e00.01xx                            |
| GLBP     | AVG/AVE        | 224.0.0.102                      | 0007.6400.xxyy                           |

## Context
- to differentiate virtual MAC addresses of various groups, HSRP uses a special format for the virtual MAC addr
	- 0000.0c07.acxx - xx is the grp number in hex. Ex.
		- 0000.0c07.ac0b; 0b means group 11

## Hot Standby Router Protocol (HSRP)
- Cisco Proprietary 
- active and standby elected
- 2 versions : v1 and v2
	  v2 adds IPV6 support, increases number of groups
- Multicast IPV4 address
	- v1 - 224.0.0.2
	- v2 - 224.0.0.102
- - default HSRP is 0

## Why do we need FHRP
- protects the default gateway used on a subnetwork by allowing 2 or more routers to provide backup for that address; in the event of a failure of an active router, the backup router will take over the address, usually within a few seconds

## How is FHRP performed

| number | reason                                                                                                                 | step                                                                                                                                  |
| ------ | ---------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | Vritual IP and generate virtual MAC                                                                                    | Configure a default virtual IP address for the default gateway                                                                        |
| 2      | active and standby elated                                                                                              | Router will negotiate which one becomes the default gateway                                                                           |
| 3      | end hosts config to use virtual IP address                                                                             | FHRP uses a virtual mac-address                                                                                                       |
| 4      | active router replies to ARP                                                                                           | Gratuitous ARP replies sent withoout being requested (broadcasted) to update all switches mac-address tables                          |
| 5      | If active R fails next active router sends gratitous ARP to update SW<br><br>Non-preemptive in case old router returns | FHRP is non -preemptive. The current active routers will not automatically give up its role, even if the former active router returns |

## HSRP Mechanics and Trouble Shooting
- Based on priority value, HSRP elects a single active router and a standby router for each group
	- To participate in the active and standby router election process, each HSRP router must be a member of the same group
	- active router is the router with the highest priority
		- forwards packets
		- responds to ARP requests with a virtual MAC address
		- can be the only router that is explicitly configured with the virtual IP address
		- sends hello packets every 3 seconds
	- standby router is the router with 2nd highest priority
		- if multiple HSRP routers have the same priority, the router with the highest IP address is elected as the active router
		- only the standby router monitors the hello packets
		- if the standby router does not receive a hello packet from the active router for the duration configured in the holdtime, the standby router will takeover as teh active router
		- default holdtime is set to 10s

### HSRP Commands

| number | reasons                                                                                                                                                                   | command                                        |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| 1      | Identifies the specific HSRP group on an interface to provide FHRP (base version)                                                                                         | R(config-if)# standby <0-255>                  |
| 2      | change the HSRP version in use                                                                                                                                            | R(config-if)# standby version 2                |
| 3      | Configure virtual IP address                                                                                                                                              | R(config-if)# standby <0-255> ip [addr]        |
| 4      | active router determined by highest priority                                                                                                                              | R(config-if)# standby <0-255> priority <0-255> |
| 5      | allow takeover when it comes back online if it has higher priority level                                                                                                  | R(config-if)# standby <0-255> preempt          |
| 6      | Check the following:<br>- HSRP group number<br>- check virtual IP address<br>- check standby router<br>- check virtual mac addr<br>- check preemption<br>- check priority | R# show standby                                |
|        |                                                                                                                                                                           |                                                |

## Virtual Router Redundancy Protocol (VRRP)
- open standard
- master and backup router are elected
- multicast ipv4 : 224.0.018
- virtual mac address: 0000.5e00.01xx 
	- xx is the VRRP group number
- in a situation with multiple subnets/vlans you can configure a different master router in each subnet/VLAN to load balance

## Gateway Load Balancing Protocol (GLBP)
- cisco proprietary 
- multicast ipv4: 224.0.0.102
- virtual MAC address:: 0007.b400.XXYY
	- XX - GLBP grop number
	- YY - AVF number
- load balances among multiple routers within a single subnet
- an AVG (active virtual gateway) is elected
- up to four AVF (active virtual forwarders) are assigned by the AVG 
	- the AVG can be an AVF too 
- Each AVF acts as the default gateway for a portion of the hosts in the subnet