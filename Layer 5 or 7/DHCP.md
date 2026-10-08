#layer5

## What is DHCP (Dynamic Host Configuration Protocol)
- DHCP is a network management protocol that is used to allow hosts to automatically./dynamically learn various aspects of their network configuration, such as IP address, subnet mask, default gateway, DNS server, etc, without manual/static configuration
- Typically used for client devices.

## Why use DHCP?
- Allows your device to get assigned an IP address without explicit configuration
- In small networks (SOHO) the router typically acts as DHCP server for hosts in the lan.
- In larger networks, the DHCP server is usually a windows/Linux server

## DHCP Mechanics
- DHCP servers lease IP address to clients - these leases are not permanent, and the client must give up the address at the end of the lease.

### DHCP offer (DORA)
- DHCP Discover - client asks if there are any DHCP servers in the network
- DHCP Offer - server asks 'How about this IP address'
- DHCP Response - 
- DHCP Acknowledge

### Diagnosis of messages sent (DORA)
- DHCP release message - client sends the release message to let go of the leased IP address. 
- DHCP Discover
	- IP header - src: 0.0.0.0 (no address yet), dst: 255.255.255.255 (broadcast)
	- UDP - src: 68 (DHCP client), dst: 67 (DHCP server)
	- bootp flag - 0x000 (unicast) - predecessor of DHCP
	- Client
- DHCP Offer - 

### DHCP (Wireshark fields)
- Bootp flags
- Client IP address - 0.0.0.0 when you don't have an IP assigned yet

### DHCP Options (find in wireshark)
- option 50 - Requested IP address for preferred IP address
- option 53 - DHCP message type
- option 54 - DHCP Server Identifier
- Option 61 - Client Identifier
- Option 255 - End
- Magic Cookie: DHCP


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

##  DHCP Config Command
| number | reason                                                                                                                          | command                                       |
| ------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| 1      | Configures DHCP relay. This enables an interface to forward DHCP broadcasts across a network to the IP address of a DHCP server | ip helper-address [ip_address of DHCP server] |
| 2      |                                                                                                                                 |                                               |
| 3      |                                                                                                                                 |                                               |
|        |                                                                                                                                 |                                               |
|        |                                                                                                                                 |                                               |
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