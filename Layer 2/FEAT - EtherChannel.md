#layer2

## Troubleshooting
-  Interface cannot be added to the channel group if Channel protocol mismatch for the interface is detected
- performing channel-group _ mode active/passive will automatically set the interfaces into LACP (same goes with auto/desirable and pagp)
- Member interfaces must have matching configurations
	- same duplex (full/half)
	- same speed
	- same switchport mode (access/trunk)
	- same allowed VLANs/native VLAN (for trunk interfaces)
- If an interface's configurations do not match the others, it will be excluded from the EtherChannel
- When performing show spanning-tree only the portchannel interface is shown instead of individual interfaces
- If a channel protocol is explicityl configured, each local switch port in the EtherChannel bundle must be configured to operate in a mode that is compatible with the channel protocol or the switch will display an error message and refuse to bundle the offending interface.
## Definitions and Key things
 - Oversubscription  - when the bandwidth of the interfaces connected to end hosts is greater than the bandwidth of the connection to the distribution switches. Some oversubscription is acceptable, but too much causes congestion
 - flow (load balancing) - communication between two nodes in the network
 - IEEE 802.3ad - standard for LACP 
 - LAG (link aggregation group) - is the generic term for an etherchannel
## What is an EtherChannel
- Etherchannel groups multiple interfaces together to act as a single interface. Acting as a single interface will not make STP disable the interfaces
## Why use an EtherChannel
- 1st problem:
	- given the context that an access layer switch has a lot of connection and only one connection to the distribution layer switch. This is a case of oversubscription
	- the single connection causes a bottle neck due to an imbalance of bandwidth 
	- the solution is to add another link to increase bandwidth so it can support all the endhosts
-  2nd problem
	- given that we made extra link connection, all except one will be disabled by STP. Otherwise, it will cause layer 2 loops which will cause broadcast storms
- Solution
	- Etherchannel groups multiple interfaces together to act as a single interface
	- STP will treat this as a single interface
	- Traffic using etherchannel will be load balanced among the physical interfaces in the group. An algorithm is used to determine which traffic will use which physical interface

## EtherChannel Load Balancing
- balances based on 'flows'
- frames in the same flow will be forwarded using the same physical interface
- If frames in the same flow were forwarded using different physical interfaces, some frames may arrive at the destination out of order, which can cause some problems
- Calculation of which interface should be used depends on the inputs and can be set:
	- src: IP or MAC
	- dst: IP or MAC
	- src and dst: IP or MAC
## EtherChannel Configurations
- There are three methods of EtherChannel Configuration on Cisco Switches. PAgP and LACP dynamically negotiates for the creation and maintenance of the EtherChannel
	- PAgP (Port aggregation protocol)
		- Cisco proprietary protocol
		- dynamically negotiates the creation/maintenance of the EtherChannel
	- LACP ()
		- IEEE 802.3ad Industry Standard Protocol
		- Dynamically negotiates the creation/maintenance of the EtherChannel
	- Static EtherChannel
		- a protocol isn't used to determine if an EtherChannel should be formed
		- Interfaces are statically configured to form an EtherChannel
- Note: Up to 8 interfaces can be formed into a single EtherChannel
	- LACP allows up to 16, but only 8 can be active at a time.
		- the other will be in standby mode, waiting for an active interface to fail.

## Layer 3 EtherChannel
- totally prevents spanning tree from being an issue if router ports are used.
- Loops can still occur between multiple switches even if all of them apply EtherChannel due to the fact that multiple etherchannels acting as a single wire can still form loops.

### Channel modes

| EtherChannel conf | Mode      | Description                                                                 |
| ----------------- | --------- | --------------------------------------------------------------------------- |
| LACP              | active    | enable LACP unconditionally                                                 |
| LACP              | passive   | Enable LACP only if a PAgP device is detected (2 passive = no Etherchannel) |
| PAgP              | auto      | Enable PAgP only if a PAgP device is detected (2 auto = no Etherchannel)    |
| PAgP              | desirable | enable PAgP unconditionally                                                 |
| N/A               | on        | enable etherchannel only. Will only work with another on                    |
|                   |           |                                                                             |

## Commands 
- etherchannel is used for show commands
- port-channel - used in config
- channel-group - used in interface config - sets the grp and mode
- channel-protocol - used in interface config - sets the protocol

| Number | Reason                                                                                                                                      | Commands                                                                                           |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| 1      | Show the configurations of the load-balancing configuration. Flags are shown for indicators.                                                | SW# show etherchannel load-balance                                                                 |
| 2      | Show the the group, protocol, ports, and port-channel. the most common command to use to see config.                                        | SW# show etherchannel summary                                                                      |
| 3      | shows the number of ports and protocol. It shows the channel group mode which cant be shown in the summary                                  | SW# show etherchannel port-channel                                                                 |
| 4      | Configure the load balancing in config mode. Types are written in the format [src\|dst]-[dst]-[ip\|mac]                                     | SW(config)# port-channel load-balance [type]                                                       |
| 5      | Configures the channel mode for a channel group. it is a good idea to set all at once since all of them should have the same configurations | SW(config)# interface range [int-range]<br><br>SW(config-if)# channel-group [groupnum] mode [mode] |
| 6      | Configure the protocol to be used by the interfaces part of the EtherChannel                                                                | SW(config)# channel-protocol [lacp \| pagp]                                                        |
| 7      | Enter interface config for the port channel                                                                                                 | SW(config)# interface port-channel [grp-num]                                                       |
| 8      | Configure, inside the port-channel interface, trunk encapsulation                                                                           | SW(config-if)# switchport trunk encapsulation dot1q                                                |
| 9      | Configure the switchport mode to trunk                                                                                                      | SW(config-if)# switchport mode trunk                                                               |
| 10     | Check if the interface is a trunk                                                                                                           | # show interfaces trunk                                                                            |
## Configuring a Layer 3 EtherChannel
| Number | Reason                                                                                                                        | Commands                                                                 |
| ------ | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| 1      | Always a good idea to configure multiple interfaces at the same time to avoid mismatches                                      | SW(config)# interface range [int-range]                                  |
| 2      | Allow routed ports ( turns a physical layer 2 switch port into a layer 3 router interface)                                    | SW(config-if)# no switchport                                             |
| 3      | Configure the channel group and the mode which automatically sets the protocol used                                           | SW(config-if)# channel-group [num] mode [mode]                           |
| 4      | Show the running configuration and scroll to portion for interfaces. It should have the no switchport tagged on the interface | SW# show running-config                                                  |
| 5      | Configure an IP address on the port channel since it is a layer 3 interface                                                   | SW(config)# interface po[group-num]<br><br>ip address \[addr] \[netmask] |
| 6      | Check the summary. It should have the 'RU' flag instead of 'SU'. R for layer3                                                 | SW# show etherchannel summary                                            |
