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
## Message Timers
- The dead timers are always 4x the hello timer amount

| Network Type                                                 | Timer Type  | Time |
| ------------------------------------------------------------ | ----------- | ---- |
| Broadcast Timer                                              | Hello Timer | 10s  |
| Broadcast Timer                                              | Dead Timer  | 40s  |
| Point-to-Point                                               | Hello Timer | 10s  |
| Point-to-Point                                               | Dead Timer  | 40s  |
| Non-Broadcast Multi-Access <br>(NBMA) / Point-to-Multipoint: | Hello Timer | 30s  |
| Non-Broadcast Multi-Access <br>(NBMA) / Point-to-Multipoint: | Dead Timer  | 120s |
## Troubleshooting
- Once the DR/BDR are selected they will keep their roles until OSPF is reset.
- MTU mismatch will cause the routers to be stuck in the exstart, exchange, or loading states

## Definitions
- DR
- BDR
- DRother
- LSA
  

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

### Broadcast Network Type
- enabled on ethernet and FDDI
- routers dynamically discover neighbours by sending/listening for OSPF hello msges using multicast address 224.0.0.5
- a DR (designated router) and BDR (backup designated router) must be elected on each subnet (only DR if there are no OSPF neighbors)
- routers which arent the DR or BDR become a DRother
- hello timer def: 10s ; 
- dead timer def: 40s

#### DR/BDR election order of priority
- 1st place will become the DR
- 2nd place will become the BDR
- Once the DR/BDR are selected they will keep their roles until OSPF is reset.

### Point to Point network type
- enabled on serial interfaces using the PPP or HDLC encapsulation by default
- routers dynamically discover neighbors by sending/listening for OSPF Hello messages using multicast address  224.0.0.5
- A DR and BDR is not elected (bc not needed)
- the 2 routers will form a full adjacency with each other
- serial interfaces
	- both sides must have the same encapsulation +
	- one side functions as a DCE (data communications equipment)
		- this sides needs to specify the clock rate speed of connection
	- the other functions as a DTE (data terminal equipment)

### Non Broacast network type

| number | reason                          | Where to find information?         |
| ------ | ------------------------------- | ---------------------------------- |
| 1      | Highest OSPF interface priority | R# show ospf interface [interface] |
| 2      | Highest OSPF router ID          | R#show ospf interface [interface]  |

## OSPF Neighbour Requirements
1. Area number match for 2 routes to be neighbours
2. Interfaces must be in same subnet
3. OSPF process must not be shutdown
4. OSPF router-ids must be unique
5. Hello and dead timers must match
6. Authentication settings must match
7. IP MTU settings must match
	1. can become OSPF neighbours but wont operate properly


## Neighbour States
- Designated router (DR)
- Backup Designated Router (BDR)
- normal neigbour router
	- remain in the 2 way state

## MTU mismatch
- will cause the routers to be stuck in the exstart, exchange, or loading states

## OSPF Commands

### Broad cast network type List of priority Commands

| number | reason          | Command                            |
| ------ | --------------- | ---------------------------------- |
| 1      | View priority   | R# show ospf interface [interface] |
| 2      | Change priority | R(config-if)# ip ospf <0-255>      |
### Broadcast Network Troubleshooting
| number | reason                                                                                                  | Command                                                                                                                                                                                              |
| ------ | ------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | OSPF Process must not be shutdown                                                                       | R(config)# router ospf [number]<br><br>R(config-router)# no shutdown                                                                                                                                 |
| 2      | Hello and Dead timers must match.<br>Using the no option resets it back to the default seconds used.    | R(config-if)# ip ospf hello-interval \<seconds><br>R(config-if)# n ip ospf hello-interval \<seconds><br><br>R(config-if)# ip ospf dead-interval \<seconds><br>R(config-if)# no ip ospf dead-interval |
| 3      | Authentication settings must match<br>1. first command enables auth<br>2. 2nd command sets the password | R(config-if)# ip ospf authentication<br><br>R(config-if)# ip ospf authentication-key \<password>                                                                                                     |
| 4      | IP MTU settings must match. Can become OSPF neighbors but wont operate properly                         | R(config-if)# ip MTU y \<bytes>                                                                                                                                                                      |

### Point to point clock settings
| number | reason                                                                          | Command                                                                          |
| ------ | ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| 1      | DCE side needs to specify the clock rate                                        | R(config-if)# clock rate $2^n*1200$                                              |
| 2      | DCE side needs to set ip address<br>                                            | R(config-if)# ip address \[addr] \[netmask]                                      |
| 3      | DCE side needs to be enabled                                                    | R(config-if)# no shutdown                                                        |
| 4      | The output must show encapsulation: HDLC                                        | R(config-if)# show interface [interface]                                         |
| 5      | Set the encapsulation type since both sides need to have the same encapsulation | R(config-if)# encapsulation <PPP/HDLC>                                           |
| 6      | configure the network type used by the interface                                | R(config-if)# ip ospf network <broadcast \| non-broadcast \| p2p \| p2multiport> |

