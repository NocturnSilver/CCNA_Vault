#layer2

## Definitions
 - Oversubscription  - when the bandwidth of the interfaces connected to end hosts is greater than the bandwidth of the connection to the distribution switches. Some oversubscription is acceptable, but too much causes congestion
 - flow (load balancing) - communication between two nodes in the network

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
- There are three methods of EtherChannel Configuration on Cisco Switches
	- PAgP (Port aggregation protocol)
	- LACP ()

## Commands 

| Number | Reason                                                                                                      | Commands                                     |
| ------ | ----------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| 1      |                                                                                                             | SW# show etherchannel load-balance           |
| 2      | Configure the load balancing in config mode. Types are written in the format [src \| dst]-[dst]-[ip \| mac] | SW(config)# port-channel load-balance [type] |
