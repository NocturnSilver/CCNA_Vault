#layer5

## Command
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