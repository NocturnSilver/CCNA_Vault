#layer3

## IPV4 Addresses
- each octet has a max value of 255 since 11111111 = 255
- each ipv4 address has 3 pars 
	- the network address
	- the host address
		- broadcast address - when the binary of the host portion is all 1's
			- 192.168.1.255/24 where the 255 part is 11111111
		- Network address - when the binary of the host portion is all 0's
			- 192.168.1.0/24 where 0 is 00000000
	- the netmask - tells where we split the address into 2:
		- host address - the latter half
		- network address - the former half

## IPV4 Header
![[Pasted image 20260824214432.png]]
- Row 1 VIDET
	- Version (4 bits) 
	- IHL (4 bits) - Internet Header Length 
		- contains the size of the ipv4 header
		- minimum value = 5 (5 x 4 bytes) = 20 bytes
		- maximum value = 15 (15 x4 bytes) = 60 bytes
	- DSCP (6 bits) - Differentiated Service Code Point
		- Real time data streaming uses this
		- ex: voice over IP
	- ECN (2 bits) - Explicit Congestion Notification
		- allows end to end notification of network congestion without dropping packets
	- Total Length (16 bits)
		- defines entire packet size in bytes including header and data
		- minimum size = 20; maximum size = 65,535 bytes
- Row 2 IFF
	- Identification (16 bits)
		- uniquely identifies the group of fragments of a single IP datagram
	- Flags (3 bits)
		- There are three bit flags defined within this field
			- Reserved (R) 1 bit: Reserved, should be set to 0
			- Dont Fragment (DF) 1 bit: 
				- specifies whether datagram can be fragmented or not
				- if the DF flag is set, and fragmentation is required to route the packet, then the packet is dropped
			- More Fragment (MF) 1 bit:
				- for unfragmented packet, the MF flag is cleared. For fragmented packets, all fragments except the last have the MF flag set. The last fragment has a non-zero fragment offset field, so it can still be differentiated from an unfragmented packet.
	- Fragment offset (13 bits)
		- 
- Row 3 TPH
	- Time to Live (8 bits)
	- Protocol (8 bits)
	- Header Checksum (16 bits)
- Row 4 Source IP address
- Row 5  Destination IP address
## Classed networks
- Not in use anymore since Classless Inter-Domain Routing allow better flexibility
- common CIDR netmasks include: 
	- /8 = 255.0.0.0 = 11111111 00000000 00000000 00000000
	- /16 = 255.255.0.0 = 11111111 11111111 00000000 00000000
	- /24 = 255.255.255.0 = 11111111 11111111 11111111 00000000
	- /32 = 255.255.255.255 = 11111111 11111111 11111111 11111111

| Class | First Octet | First Octet <br>Numeric Range | Prefix<br>Length | Purpose      | Number<br>Of networks | Addr per<br>Network |
| ----- | ----------- | ----------------------------- | ---------------- | ------------ | --------------------- | ------------------- |
| A     | 0xxxxxxx    | 0-127                         | /8               | Commercial   | 128 (2^7)             | 16,777,216 (2^24)   |
| B     | 1xxxxxxx    | 128-191                       | /16              | Commercial   | 16,384 (2^14)         | 65,536 (2^16)       |
| C     | 110xxxxx    | 192-223                       | /24              | Commercial   | 2,097,152 (2^21)      | 256 (2^8)           |
| D     | 1110xxxx    | 224-239                       |                  | Multicast    |                       |                     |
| E     | 1111xxxx    | 240-255                       |                  | Experimental |                       |                     |
