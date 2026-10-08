#layer5


## Definitions
- DHCP pool - subnet of addresses that can be assigned to DHCP clients. It includes other info such as DNS server and default gateway

## Troubleshooting
- Create a separate DHCP pool for each network the router is acting as a DHCP server for.

## What is DHCP (Dynamic Host Configuration Protocol)
- DHCP is a network management protocol that is used to allow hosts to automatically./dynamically learn various aspects of their network configuration, such as IP address, subnet mask, default gateway, DNS server, etc, without manual/static configuration
- Typically used for client devices.

## Why use DHCP?
- Allows your device to get assigned an IP address without explicit configuration
- In small networks (SOHO) the router typically acts as DHCP server for hosts in the lan.
- In larger networks, the DHCP server is usually a windows/Linux server

## DHCP Mechanics
- DHCP servers lease IP address to clients - these leases are not permanent, and the client must give up the address at the end of the lease.

### DHCP messages (DORA)
- DHCP Discover - client asks if there are any DHCP servers in the network
- DHCP Offer - server asks 'How about this IP address'
- DHCP Request - client tells server 'I want to use the IP address you offered me.' Client needs to tell which server (there may be multiple DHCP servers) it wants to accept the offer from. Client will typically accept the first offer it receives
- DHCP Acknowledge - Server sends acknowledgement and client sets its ip address 

| DHCP Message | From -> to       | Broadcast/unicast |
| ------------ | ---------------- | ----------------- |
| Discover     | client -> server | broadcast         |
| Offer        | server -> client | broadcast/unicast |
| Request      | client -> server | broadcast         |
| Ack          | server -> client | broadcast/unicast |
### DHCP (Wireshark fields)
- Bootp flags
- Client IP address - 0.0.0.0 when you don't have an IP assigned yet
- option 3 - Router
- option 6 - Domain Name Server
- option 50 - Requested IP address for preferred IP address
- option 53 - DHCP message type
- option 54 - DHCP Server Identifier
- Option 61 - Client Identifier
- Option 255 - End
- Magic Cookie: DHCP
### Diagnosis of messages sent (DORA)
- DHCP release message - client sends the release message to let go of the leased IP address. 
- DHCP Discover
	- IP header - src: 0.0.0.0 (no address yet), dst: 255.255.255.255 (broadcast)
	- UDP - src: 68 (DHCP client), dst: 67 (DHCP server)
	- bootp flag - 0x000 (unicast) - predecessor of DHCP
	- Client
- DHCP Offer - 
	- sent as a unicast frame since it learnt the address from discovery
	- src port: 67, dst port 68
	- bootp flags: 0x0000 (unicast/broadcast - some clients wont accept unicast messages until their IP address is set hence broadcast)
	- client IP address - 0.0.0.0
	- Option 51 - Addres lease time
	- option 6 - Domain Name Server
	- Option 3 - Router
- DHCP Request
	- IP address src: 0.0.0.0, DST: 255.255.255.255
	- UDP src: 68, dst: 67
	- bootp flags: 0x0000 (unicast - tell server to use unicast)
	- Option 54 - DHCP Server Identifier (server ip address that the client accepted)
- DHCP Ack
	- once this message is received the client configures the IP address
	- option 53 - DHCP Message type: ack

### DHCP Server Configuration

| number | reason                                                                                      | command                                                                                           |
| ------ | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| 1      | Specify a range of addresses that won't be given to DHCP clients                            | R(config)# ip dhcp excluded-address \<start-addr> \<last-addr>                                    |
| 2      | Create a DHCP pool. Enters DHCP config mode                                                 | R(config)# iop dhcp <pool_name>                                                                   |
| 3      | Specify the subnet of addresses to be assigned to the clients (except the excluded address) | R(dhcp-config)# network \<ip-address> \<netmask \| prefix length>                                 |
| 4      | Specify the DNS server that DHCP clients should use                                         | R(dhcp-config)# dns-server \<ip-address>                                                          |
| 5      | Specify the domain name of the network.                                                     | R(dhcp-config)#domain-name \<domain-name>                                                         |
| 6      | Specify the default gateway the DHCP clients will use                                       | R(dhcp-config)# default-router \<ip-address>                                                      |
| 7      | Specify the lease time                                                                      | R(dhcp-config)# lease \<days> \<hours> \<minutes><br><br>or<br><br>R(dhcp-config)# lease infinite |
| 8      | Shows all of the DHCP clients that are currently assigned IP addresses                      | R# show ip dhcp binding                                                                           |


## DHCP Relay - centralised DHCP server solution
- Some network engineers might choose to configure each router to act as the DHCP server for its connected LANs
- However, large enterprises often choose to use a centralized DHCP server
- If the server is centralised, it won't receive the DHCP client's broadcast DHCP messages. (broadcast messages don't leave the local subnet)
- To fix this, you can configure a router to act as a DHCP relay agent.
- The router will forward the client's broadcast DHCP messages to the remote DHCP server as unicast messages.
- The relay turns the broadcast message from the client into a unicast message using the address of the (router) interface it received the message from before relaying it to the DHCP server.

## DHCP Relay Config

| number | reason                                                                                                                          | command                                       |
| ------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| 1      | Configures DHCP relay. This enables an interface to forward DHCP broadcasts across a network to the IP address of a DHCP server | ip helper-address [ip_address of DHCP server] |
| 2      |                                                                                                                                 |                                               |
| 3      |                                                                                                                                 |                                               |
|        |                                                                                                                                 |                                               |
|        |                                                                                                                                 |                                               |

## Windows configuration
- go to networks and sharing centre
- click on the connection on go to properties
- find the IPv4 properties and select it
- make sure the 'Obtain an IP address automatically' and 'Obtain DNS server address automatically' is selected 

## Windows Commands
| number | reason                                                             | command             |
| ------ | ------------------------------------------------------------------ | ------------------- |
| 1      | shows the ipv4 address, subnetmask, lease obtained, lease expires. | > ipconfig /all     |
| 2      | Release the DHCP learned ip address                                | > ipconfig /release |


## Other notes
- ip helper-address command configures an interface to forward broadcasts to the following User Datagram Protocol (UDP) ports:
	- port 37 - Time protocol
	- port 49 - Terminal Access Controller Access-Control Systems (TACACS)
	- port 53 - Domain Name System (DNS)
	- port 67 - Bootstrap Protocol (BOOTP) and DHCP server
	- port 68 - BOOTP and DHCP Client
	- port 69 - Trivial File Transfer Protocol (TFTP)
	- port 137 - Network Basic Input/Output System (NetBIOS) Name service
	- port 138 - NetBIOS Datagrama