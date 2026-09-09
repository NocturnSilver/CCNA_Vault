#layer2

# Caution
- Because they share information in the network, they can be considered a security risk and are often not used. It is up to the network engineer/admin to decide if they want to use them in the network or not.
- CDP is enabled in cisco devices by default

## Context 
- CDP and LLDP are layer 2 discovery protocols
-  Layer 2 discovery protocols  share information with and discover information about neighboring (connected) devices
	- they share information that includes host name, IP address, device type, etc.
- ARPA Encapsulation - Cisco's term for standard Ethernet II (DIX) framing. Serves as the default encapsulation method for Ethernet interfaces on Cisco routers and switches

## What is CDP
- Cisco proprietary protocol for neighbor discovery
- enabled on cisco devices by default
- CDP messages are periodically sent to multicast MAC address 0100.0CCC.CCC
- when a device receives a CDP message, it processes and discards the message. 
- It does not forward it to other devices.
- By default, CDP messages are sent once every 60s
- Default CDP holdtime is 180 seconds

## What is LLDP
- Industry standard protocol (IEEE 802.1AB)


## Discovery Protocol Timers

| Protocol | Purpose                                                                                                                | Time |
| -------- | ---------------------------------------------------------------------------------------------------------------------- | ---- |
| CDP      | CDP Messages frequency                                                                                                 | 60s  |
| CDP      | CDP hold time. If a message isn't received from neighbor for 180s, the neighbor is removed from the CDP neighbor table | 180s |
| LLDP     |                                                                                                                        |      |
| LLDP     |                                                                                                                        |      |

## Discovery Protocol MAC Addresses

| Protocol | Mac Address    |
| -------- | -------------- |
| CDP      | 0100.0CCC.CCCC |
| LLDP     |                |

## Commands
| Number | Reason                                                                                                                                                                                                                                       | Command               |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| 1      | show global CDP information. Frequency, hold time, and version number                                                                                                                                                                        | R# show cdp           |
| 2      | show the CDP packets that are sent and received. Errors are also shown                                                                                                                                                                       | R# show cdp Traffic   |
| 3      | Shows basic information about interfaces                                                                                                                                                                                                     | R# show cdp interface |
| 4      | Show the neighbour table. It shows the Device ID of the neighbours, Local Interface to which the Device ID is connected to on the local device,  hold time, Capability (codes are found on the table printed on terminal), Platform, Port ID | R# show cdp neighbors |


