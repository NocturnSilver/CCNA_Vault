#layer3
#routingprotocol

## OSPF addresses
| number | Type               | Address                | Description                                                              |
| ------ | ------------------ | ---------------------- | ------------------------------------------------------------------------ |
| 1      | OSPF Hello Message | multicast<br>224.0.0.5 | Encapsulated in an IP header<br>with a value of 89 in the protocol field |
| 2      | Messages to DR/BDR | multicast<br>224.0.0.6 | DRother routers send messages to DR and BDR in this multicast address    |
|        |                    |                        |                                                                          |
|        |                    |                        |                                                                          |
## Definitions
- Link State Advertisements (LSAs)
- Link State Database (LSDBs)
- area - set of routers and links that share the same LSDB
- backbone area (area 0) - area that all other areas must connect to
- internal routers - routers with all interfaces in the same area
- area border routers (ABRs) - routers with interfaces in multiple areas
- backbone routers - connected to the backbone area (area 0)
- intra-area route - route to a destination inside the same OSPF area
- interarea route - route to a destination in a different OSPF area
- autonomous system boundary router (ASBR) is an OSPF router that connects the OSPF network to an external network.
- DCE - Data communication equipment
- DTE - Data terminal equipment

## Troubleshooting Areas
- Small networks can be single-area without any negative effects on performance.
- By dividing a large OSPF network into several smaller areas, you can avoid the negative effects
- In larger networks, a single-area design can have negative effects:
	- The SPF algorithm takes more time to caclulate routes
	- The SPF algorithm requires exponentially more processing power on the routers
	- The larger LSDB takes up more memory on the routers 
	- any small change in the network causes every router to flood LSAs and run the SPF algorithm again
- Area border routers (ABRs) maintains a separate LSDB for each area they are connected to
	- recommended that you connect an ABR to a maximum of 2 areas.
	- connecting an ABR to 3+ areas can overburden the router
- OSPF areas should be contiguous (not split into non-connecting areas)
## Troubleshooting Configuration
- The main goal is to make sure routers successfully become OSPF neighbours in configuring and troubleshooting.
- Once the DR/BDR are selected they will keep their roles until OSPF is reset.
- MTU mismatch will cause the routers to be stuck in the exstart, exchange, or loading states
- OSPF configurations requires you to specify the area and the area specified must be the same for all interfaces participating in OSPF
-  Configure a reference bandwidth greater than the fastest link in your network (to allow for future upgrades), and configure it on all OSPF routers in the network
- If two routers have network type mismatch, they will form an OSPF adjacency but won't learn OSPF routes

## What is OSPF (Open Shortest Path First)
- Uses the Shortest Path First algorithm of Dutch computer scientist Edsger Dijkstra (Dijkstra's algorithm)
- Has 3 versions
	- OSPFv1 (1989): OLD, not in use anymore
	- OSPFv2 (1998): Used for IPv4 
	- OSPFv3 (2008): Used for IPv6 (can also be used for IPv4, but usually v2 is used)
- Router store information about the network in LSAs (Link State Advertisments) which are organized in a structure called LSDB (Link State Database)
- Routers will Flood LSAs until all routers in the OSPF area develop the same map of the network (LSDB)

### LSA Flooding
- When OSPF is enabled on an interface, the router creates an LSA to tell its neighbours about the network.
- The LSA is flooded throughout the network until all routers have received it
- This results in all routers sharing the same LSDB
- Each router then uses SPF algorithm to calculate the best route

### Steps in the process of sharing LSAs
1. Become Neighbours - with other routers connected to the same segment
2. Exchange LSAs - with neighbour routers.
3. Calculate the best routes - to each destination, and insert them into the routing table

### OSPF Areas Definition
- OSPF areas are used to divide the up the network.
- An area is a set of routers and links that share the same LSDB
- The backbone area (area 0) is an area that all other areas must connect to 
- Routers with all interfaces in the same area are called internal routers
- Routers with interfaces in multiple areas are called area border routers (ABRs)
- backbone routers - connected to the backbone area (area 0)
- intra-area route - route to a destination inside the same OSPF area
- interarea route - route to a destination in a different OSPF area

### OSPF Areas Rules
- OSPF areas should be contiguous (not split into non-connecting areas)
- All OSPF areas must have at least one ABR connected to the backbone area
- OSPF interfaces in the same subnet must be in the same area

### Router ID order of priority (same with EIGRP)
1. Manual configuration
2. Highest IP address on a loopback interface
3. Highest IP address on a physical interface

#### Loopback Interfaces
- a loopback interface is a virtual interface in the router
- it is always up/up (unless you manually shut it down)
- it is not dependent on a physical interface
- It provides a consistent IP address that can be used to reach/identify the router

### OSPF Metric (Cost)
- automatically calculated based on bandwidth (speed) of the interface
- It is calculated by dividing a reference bandwidth value by the interface's bandwidth
- The default reference bandwidth is 100mbps
- all values less than 1 is converted to 1
- Therefore FastEthernet, Gigabit Ethernet, 10G Ethernet, etc. are equal and all have a cost of 1 by default
- OSPF cost to a destination is the total cost of the outgoing/exit interfaces
- loop back interfaces have the cost of 1

| Reference | Interface (in mbps) | Cost (reference divided by interface) |
| --------- | ------------------- | ------------------------------------- |
| 100       | 10                  | 10                                    |
| 100       | 100                 | 1                                     |
| 100       | 1000                | 1                                     |
| 100       | 10000               | 1                                     |

#### OSPF Cost Commands
| Number | Reason                                                                                                                                                                                                                                                                                                                                                                                                       | Command                                                     |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------- |
| 1      | Show the following information:<br>- cost, router-id, process-id, area<br>- DR/BDR interface address<br>- priority                                                                                                                                                                                                                                                                                           | R# show ip ospf interface                                   |
| 2      | Change the cost of the reference bandwidth. Configure a reference bandwidth greater than the fastest link in your network (to allow for future upgrades)                                                                                                                                                                                                                                                     | R(config-router)# auto-cost reference bandwidth <1-4294967> |
| 3      | Change the OSPF cost of an interface. this cost will take priority over the automatically calculated cost. Manual configuration                                                                                                                                                                                                                                                                              | R(config-if)# ip ospf cost <1-65535>                        |
| 4      | Cost can also be changed by changing the bandwidth of the interface bandwidth. It is to be noted that although the bandwidth matches the interface speed by default, changing the interface bandwidth doesn't change the speed at which the interface operates. The bandwidth is just a value used to calculate the OSPF cost, EIGRP metric, etc.<br>(NOT RECOMMENDED since it is used in other calculation) | R(config-if)# bandwidth <1-10000000>                        |
|        |                                                                                                                                                                                                                                                                                                                                                                                                              |                                                             |

## OSPF Messages

### Message Types
| Message Type | Description                                                      |
| ------------ | ---------------------------------------------------------------- |
| Hello        | For neighbour discovery and and maintenance                      |
| DBD          | summary of LSDB of the router. Used to check if LSDB is the same |
| LSR          | Requests specific LSAs to neighbor                               |
| LSU          | Send specific LSAs to neighbour                                  |
| LSAck        | Used to acknowledge that router received the message             |

### Message Timers
- The dead timers are always 4x the hello timer amount

| Network Type                                                 | Timer Type  | Time |
| ------------------------------------------------------------ | ----------- | ---- |
| Broadcast Timer                                              | Hello Timer | 10s  |
| Broadcast Timer                                              | Dead Timer  | 40s  |
| Point-to-Point                                               | Hello Timer | 10s  |
| Point-to-Point                                               | Dead Timer  | 40s  |
| Non-Broadcast Multi-Access <br>(NBMA) / Point-to-Multipoint: | Hello Timer | 30s  |
| Non-Broadcast Multi-Access <br>(NBMA) / Point-to-Multipoint: | Dead Timer  | 120s |


## OSPF Neighbour Process

### Neighbour Requirements
1. Area number match for 2 routes to be neighbours
2. Interfaces must be in same subnet
3. OSPF process must not be shutdown
4. OSPF router-ids must be unique
5. Hello and dead timers must match
6. Authentication settings must match
7. IP MTU settings must match
	1. can become OSPF neighbours but wont operate properly

### Neighbour roles
- Designated router (DR)
	- The main router elected on a network segment
	- acts as a central hub
	- all other routers send their routing updates (LSAs) to the DR, and the DR distributes those updates to the rest of the network
- Backup Designated Router (BDR)
	- The backup router for the DR
	- listens to all updates and stays synchronised with the DR
	- If the DR fails, the BDR immediately takes over its role without needing a brand-new election 
- DROther 
	- any router on the network segment that is not the DR nor the BDR
	- only form full adjacencies with the DR and BDR
	- remain in the 2 way state
	- do not exchange routing updates directly with each other


### MTU mismatch
- will cause the routers to be stuck in the exstart, exchange, or loading states

### Neighbour states

| number | state    | description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | Down     | neighbor in down state has not yet sent a hello packet<br>sends an OSPF Hello packet to 224.0.0.5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| 2      | Init     | Hello packet is received by the neighbor router but the hello packet does not contain the receiving router's ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| 3      | 2way     | Neighbour router replies with a hello packet that contains the receiving router's ID<br><br>if both routers reach the 2-way state, it means that all of the conditions have been met for them to become OSPF neighbours. They are now ready to share LSAs to build a common LSDB.<br><br>at the end of the 2 way state, the DR and BDR are elected for broadcast and nonbroadcast (NBMA) networks<br><br>In broadcast and NBMA only the DR and BDR proceed to the full state. Other neighbors remain in the 2way state<br><br>Verify whether all routers on the segment are set to a priority of 0, which prevents any of them from becoming the DR or BDR |
| 4      | Exstart  | choose which router will start LSDB info exchange<br>routes with higher RID = master, lower RID = slave<br><br>To decide who becomes master or slave they exchange Database Descriptors (DBD) packets<br><br>master initiates exchange                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 5      | Exchange | routers exchange DBD which contain a list of their LSAs in their LSDB.<br><br>Only basic info is exchange not entire LSA<br><br>compare info from DBD to their own LSDB to find out which LSA to receive from neighbour                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| 6      | Loading  | Routers send link state requests (LSR) to neighbours to request to send them LSAs they dont have.<br><br>LSAs are sent in link state update (LSU) messages<br><br>routers send LSAck messages to acknowledge they have received the LSAs                                                                                                                                                                                                                                                                                                                                                                                                                   |
| 7      | Full     | Routers have a full OSPF adjacent and identical LSDBs<br><br>continue to send and listen for hello packet every 10s by default to maintain neighbour adjacency.<br><br>if dead timer = 0, then remove neighbour. Dead timer is 40s default. It is reset when hello packet is received<br><br>the routers will continue to share LSAs as the network changes to make sure each router has a complete and accurate map of the network (LSDB)                                                                                                                                                                                                                 |
### Message Types
| Type | Name                                  | Purpose                                                                                    |
| ---- | ------------------------------------- | ------------------------------------------------------------------------------------------ |
| 1    | Hello                                 | Neighbour Discovery and maintenance                                                        |
| 2    | Database Description<br>(DBD)         | Summary of the LSDB of the router<br>Used to checkc if the LSDB of each router is the same |
| 3    | Link-State Request<br>(LSR)           | Requests specific LSAs from the neigbour                                                   |
| 4    | Link-State Update<br>(LSU)            | Sends specific LSAs to the neighbour                                                       |
| 5    | Link-State Acknowledgement<br>(LSAck) | Used to acknowledge that the router received a message                                     |
#### LSA types
- There are 11 types of LSAs, but only there are applicable as of the moment.
- Type 1 - Router LSA
	- Every OSPF router generates this type of LSA
	- It identifies the router using its router ID
	- It also lists network attached to the router's OSPF-activated interfaces.
- Type 2 - Network LSA
	- Generated by the DR of each 'multi-access' network (i.e the broadcast network type).
	- Lists the router which are attached to the multi-access network
- Type 5 - AS External LSA
	- Generated by ASBRs to describe the routes to destinations outside of the AS (OSPF domain)

#### OSPF LSA types commands
| Number | Reason                                                                                                       | Command                  |
| ------ | ------------------------------------------------------------------------------------------------------------ | ------------------------ |
| 1      | Shows the LSA types and other<br>information such as Link ID, ADV router,<br>Age, Seq#, Checksum, Link count | R# show ip ospf database |

### OSPF Neighbour commands

| Number | Reason                                                             | Command                               |
| ------ | ------------------------------------------------------------------ | ------------------------------------- |
| 1      | Shows the Neighbour ID, Priority,<br>Dead time, Address, interface | R# show ip ospf neighbour             |
| 2      | Show more detailed OSPF information on a per-interface basis.      | R# show ip ospf interface [interface] |

## OSPF Network Types
- connection between OSPF neighbors  (ethernet, etc.)
- Three main types
	- broadcast-enabled by default on ethernet and FDDI interfaces (FDDI - fiber distributed data interfaces)
	- point-to-point - enabled by default on PPP and HDLC
		- PPP - point to point protocol
		- HDLC - high level data link control
	- non-broadcast - enabled by default on frame relay and x.25 interfaces

### Summary
|                                     | Broadcast               | Point-to-point              | Non-Broadcast                |
| ----------------------------------- | ----------------------- | --------------------------- | ---------------------------- |
| interface default                   | Ethernet, FDDI          | HDLC, PPP serial interfaces | frame relay, x.25 interfacse |
| DR/BDR elected?                     | yes                     | No                          | yes                          |
| Dynamically Discover<br>Neighbours? | yes                     | yes                         | Nofoun                       |
| default timers                      | Hello - 10<br>dead - 40 | Hello - 10<br>dead - 40     | Hello - 10<br>dead - 40      |

### Broadcast Network Type
- enabled on ethernet and FDDI (by default)
- routers dynamically discover neighbours by sending/listening for OSPF hello msges using multicast address 224.0.0.5
- a DR (designated router) and BDR (backup designated router) must be elected on each subnet (only DR if there are no OSPF neighbors)
- routers which arent the DR or BDR become a DRother
- hello timer def: 10s
- dead timer def: 40s

#### DR/BDR election order of priority
1. Highest OSPF interface priority
2. Highest OSPF Router ID
- 1st place will become the DR
- 2nd place will become the BDR
- default OSPF interface priority is 1 on all interfaces
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

| number | reason                                                                                                                       | Where to find information?             |
| ------ | ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| 1      | Highest OSPF interface priority<br>Highest OSPF router ID                                                                    | R# show ospf interface [interface]     |
| 2      | Change OSPF priority of an interface. Set the priority if you want the OSPF priority to be the highest so that it becomes DR | R(config-if)# ip ospf priority <0-255> |
|        |                                                                                                                              |                                        |



## OSPF Commands

### Basic OSPF Configurations
| number | reason                                                                                                                                                                                                                                                                                                                                                                                                               | Command                                                                                                                                                                                            |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | OSPF Process must not be shutdown.<br>OSPF process ID is locally significant. Routers with different process IDs can become OSPF neighbors                                                                                                                                                                                                                                                                           | R(config)# router ospf [process-number 1-65535]<br><br>R(config-router)# no shutdown                                                                                                               |
| 2      | Look for any interface with an IP address contained in the range specified in the network command.<br><br>OSPF uses wildcard masks. OSPF network requires you to specify the area.                                                                                                                                                                                                                                   | R(config-router)# network \[ip-addr] \[netmask] \[area-number]                                                                                                                                     |
| 3      | Activate OSPF directly on an interface                                                                                                                                                                                                                                                                                                                                                                               | R(config-if)# ip ospf [process-id] area [area]                                                                                                                                                     |
| 4      | Tells the router to stop sending OSPF 'hello' messages out of the interface.<br>However, the router will continue to send LSAs informing it's neighbours about the subnet configured on the interface<br><br>Use this command on interfaces which don't have any OSPF neighbours<br><br>Another way to configure passive interfaces is to make all interfaces passive then configuring specific interfaces as active | R(config-router)# passive-interface [interface-id]<br><br>R(config-router)# passive-interface default<br><br>R(config-router)# no passive-interface [interface-id]<br><br><br><br><br><br><br><br> |
| 5      | Set the default gateway                                                                                                                                                                                                                                                                                                                                                                                              | R(config)# ip route 0.0.0.0 0.0.0.0 [interface \|next-hop]                                                                                                                                         |
| 6      | Allow the interface to advertise the default gateway to other OSPF devices                                                                                                                                                                                                                                                                                                                                           | R(config-router)#default-information originate                                                                                                                                                     |
| 7      | Shows the codes, gateway of last resort, and protocol set on the interfaces                                                                                                                                                                                                                                                                                                                                          | R# show ip route                                                                                                                                                                                   |
| 8      | Configure the router ID in ip address format                                                                                                                                                                                                                                                                                                                                                                         | R(config-router)# router-id [A.B.C.D]                                                                                                                                                              |
| 9      | Reset all OSPF process. Usually a bad idea for real systems                                                                                                                                                                                                                                                                                                                                                          | R# clear ip ospf process                                                                                                                                                                           |
| 10     | Configure the maximum paths for load balancing                                                                                                                                                                                                                                                                                                                                                                       | R(config-router)# maximum-paths <1-32>                                                                                                                                                             |
| 11     | Change the administrative distance                                                                                                                                                                                                                                                                                                                                                                                   | R(config-router)# distance <1-255>                                                                                                                                                                 |
| 12     | Shows the protocol applied for dynamic routing                                                                                                                                                                                                                                                                                                                                                                       | R# show ip protocols                                                                                                                                                                               |


### Broad cast network type List of priority Commands

| number | reason                                                                                                                                                   | Command                               |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| 1      | Check Interface, PID, Area, IP addr/mask, cost, state, Nbrs F/C (F - number of full neighbours, C - total number of OSPF neighbouts), priority, and more | R# show ip ospf interface [interface] |
| 2      | Change priority                                                                                                                                          | R(config-if)# ip ospf <0-255>         |
| 3      | Check Interface, PID, Area, IP addr/mask, cost, state, Nbrs F/C (F - number of full neighbours, C - total number of OSPF neighbouts)                     | R# show ip ospf interface brief       |
### Broadcast Network Troubleshooting
| number | reason                                                                                                                                     | Command                                                                                                                                                                                              |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | OSPF Process must not be shutdown.<br>OSPF process ID is locally significant. Routers with different process IDs can become OSPF neighbors | R(config)# router ospf [process-number 1-65535]<br><br>R(config-router)# no shutdown                                                                                                                 |
| 2      | Hello and Dead timers must match.<br>Using the no option resets it back to the default seconds used.                                       | R(config-if)# ip ospf hello-interval \<seconds><br>R(config-if)# n ip ospf hello-interval \<seconds><br><br>R(config-if)# ip ospf dead-interval \<seconds><br>R(config-if)# no ip ospf dead-interval |
| 3      | Authentication settings must match<br>1. first command enables auth<br>2. 2nd command sets the password                                    | R(config-if)# ip ospf authentication<br><br>R(config-if)# ip ospf authentication-key \<password>                                                                                                     |
| 4      | IP MTU settings must match. Can become OSPF neighbors but wont operate properly                                                            | R(config-if)# ip MTU y \<bytes>                                                                                                                                                                      |

### Point to point clock settings
| number | reason                                                                          | Command                                                                                                                |
| ------ | ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| 1      | Set the encapsulation type since both sides need to have the same encapsulation | R(config-if)# encapsulation <PPP/HDLC>                                                                                 |
| 2      | Check for DCE or DTE                                                            | R# show controllers [serial-interface]                                                                                 |
| 3      | DCE side needs to specify the clock rate                                        | R(config-if)# clock rate [$2^n*1200$ bits-per-second]                                                                  |
| 4      | DCE side needs to set ip address<br>                                            | R(config-if)# ip address \[addr] \[netmask]                                                                            |
| 5      | DCE side needs to be enabled                                                    | R(config-if)# no shutdown                                                                                              |
| 6      | configure the network type used by the interface                                | R(config-if)# ip ospf network <broadcast \| non-broadcast \| point-to-point (p2p) \| point-to-multiport (p2multiport)> |
| 7      | The output must show encapsulation: HDLC                                        | R(config-if)# show interface [interface]                                                                               |
|        |                                                                                 |                                                                                                                        |

