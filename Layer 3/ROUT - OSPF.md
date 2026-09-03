#layer3
#routingprotocol

# Important Information lookups
## OSPF addresses
| number | Type               | Address                | Description |
| ------ | ------------------ | ---------------------- | ----------- |
| 1      | OSPF Hello Message | multicast<br>224.0.0.5 |             |
| 2      | Messages to DR/BDR | multicast<br>224.0.0.6 |             |
|        |                    |                        |             |
|        |                    |                        |             |
## Troubleshooting
- Once the DR/BDR are selected they will keep their roles until OSPF is reset.

## Definitions
- DR
- BDR
- DRother

## Neighbor States

| number | state    | description                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | Down     | neighbor in down state has not yet sent a hello packet                                                                                                                                                                                                                                                                                                                                                                                                        |
| 2      | Init     | Hello packet is received from the neighbor router but the hello packet does not contain the receiving router's ID                                                                                                                                                                                                                                                                                                                                             |
| 3      | 2way     | Neighbour router replies with a hello packet that contains the receiving router's ID<br><br>at the end of the 2 way state, the DR and BDR are elected for broadcast and nonbroadcast (NBMA) networks<br><br>In broadcast and NBMA only the DR and BDR proceed to the full state. Other neighbors remain in the 2way state<br><br>Verify whether all routers on the segment are set to a priority of 0, which prevents any of them from becoming the DR or BDR |
| 4      | Exstart  |                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 5      | Exchange |                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 6      | Loading  |                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 7      | Full     |                                                                                                                                                                                                                                                                                                                                                                                                                                                               |

# Definitions and Theory

## What is OSPF
- 

## OSPF Network Types
- connection between OSPF neighbors  (ethernet, etc.)
- Three main types
	- broadcast-enabled by default on ethernet and FDDI interfaces (FDDI - fiber distributed data interfaces)
	- point-to-point - enabled by default on PPP and HDLC
		- PPP - point to point protocol
		- HDLC - high level data link control
	- non-broadcast - enabled by default on frame relay and x.25 interfaces
## Broadcast Network Types
- enabled on ethernet and FDDI
- routers dynamically discover neighbours by sending/listening for OSPF hello msges using multicast address 224.0.0.5
- a DR (designated router) and BDR (backup designated router) must be elected on each subnet (only DR if there are no OSPF neighbors)
- routers which arent the DR or BDR become a DRother

## DR/BDR election order of priority
- 1st place will become the DR
- 2nd place will become the BDR
- Once the DR/BDR c

| number | reason                          | Where to find information?         |
| ------ | ------------------------------- | ---------------------------------- |
| 1      | Highest OSPF interface priority | R# show ospf interface [interface] |
| 2      | Highest OSPF router ID          | R#show ospf interface [interface]  |

## Neighbour States
- Designated router (DR)
- Backup Designated Router (BDR)
- normal neigbour router
	- remain in the 2 way state

## MTU mismatch
- will cause the routers to be stuck in the exstart, exchange, or loading states

## OSPF Commands

### List of priority Commands

| number | reason          | Command                            |
| ------ | --------------- | ---------------------------------- |
| 1      | View priority   | R# show ospf interface [interface] |
| 2      | Change priority | R(config-if)# ip ospf <0-255>      |
