#layer2

# Caution
- Because they share information in the network, they can be considered a security risk and are often not used. It is up to the network engineer/admin to decide if they want to use them in the network or not.
 - CDP is globally enabled by default
- CDP is also enabled on each interface by default

## Troubleshooting
- LLDP is usually disabled on Cisco devices by default, so it must be enabled manually
- A device can run CDP and LLDP at the same time
- CDP/LLDP can be applied to all applicable network devices since its a disovery protocol.
- default CDP version = 2

## Context 
- CDP and LLDP are layer 2 discovery protocols
-  Layer 2 discovery protocols  share information with and discover information about neighboring (connected) devices
	- they share information that includes host name, IP address, device type, etc.
- ARPA Encapsulation - Cisco's term for standard Ethernet II (DIX) framing. Serves as the default encapsulation method for Ethernet interfaces on Cisco routers and switches
- CDP/LLDP sends advertisements directly to physically connected local neighbors using MAC-level framing

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
- usually disabled on Cisco devices by default, so it must be enabled manually
- A device can run CDP and LLDP at the same time
- LLDP messages are periodically sent to multicast MAC address 0180.c200.000e
- when a device receives and LLDP message, it processes and discards the message. It does not forward it to other devices


## Discovery Protocol Timers

| Protocol | Purpose                                                                                                                | Default Time |
| -------- | ---------------------------------------------------------------------------------------------------------------------- | ------------ |
| CDP      | CDP Messages frequency                                                                                                 | 60s          |
| CDP      | CDP hold time. If a message isn't received from neighbor for 180s, the neighbor is removed from the CDP neighbor table | 180s         |
| LLDP     | LLDP message frequency                                                                                                 | 30s          |
| LLDP     | LLDP holdtime                                                                                                          | 120s         |
| LLDP     | Reinitialization delay. If LLDP is enable, this timer will delay the actual initialization of LLDP.                    | 2s           |

## Discovery Protocol MAC Addresses

| Protocol | Mac Address    |
| -------- | -------------- |
| CDP      | 0100.0CCC.CCCC |
| LLDP     | 0180:c200:000e |

## CDP Show Commands
| Number | Reason                                                                                                                                                                                                                                                       | Command                      |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------- |
| 1      | show global CDP information. Frequency, hold time, and version number                                                                                                                                                                                        | R# show cdp                  |
| 2      | show the CDP packets that are sent and received. Errors are also shown                                                                                                                                                                                       | R# show cdp Traffic          |
| 3      | Shows basic information about interfaces                                                                                                                                                                                                                     | R# show cdp interface        |
| 4      | Show the neighbour table. It shows the Device ID of the neighbours, Local Interface to which the Device ID is connected to on the local device,  hold time, Capability (codes are found on the table printed on terminal), Platform, Port ID                 | R# show cdp neighbors        |
| 5      | Shows you the VTP mangement domain (only CDP), software (IOS) version, native vlan of neighbour, duplex option. CDP helps identify mismatch of native vlan or interface duplex (cdp will display messaged if there is a mistmatch). IP address is also shown | R# show cdp neighbors detail |
| 6      | display the same info as show cdp neighbour but only for the specified neighbour                                                                                                                                                                             | R# show cdp entry [deviceID] |


### Important parts of show CDP neighbors to take note off
- Device ID
	- tells you the ID name of the neighbouring device
- Local Interface
	- tell you the what interface on the machine the neighbouring device is connected to
- Holdtime
	- Tells you the holdtime for the device
- Capabilities
	- R - router
	- B - source Route bridge
	- S - Switch
- Platform
	- tells you the product number of the neighbouring devices
	- tells what the device model it is connected to is
- Port ID
	- tells you the remote interface
	- tells you the connected interface on the neighboring device.
	- tells you the port ID of the neighbouring device
## CDP Configuration Commands
- CDP is globally enabled by default
- CDP is also enabled on each interface by default

| Number | Reason                                    | Command                           |
| ------ | ----------------------------------------- | --------------------------------- |
| 1      | Enable/Disable CDP globally               | R(config)# [no] cdp run           |
| 2      | Enable/Disable CDP on specific interfaces | R(config)# [no] cdp enable        |
| 3      | Configure the CDP timer                   | R(config)# cdp timer [seconds]    |
| 4      | Configure the CDP holdtime                | R(config)# cdp holdtime [seconds] |
| 5      | Enable/Disable CDPv2                      | R(config)# [no] cdp advertise-v2  |
|        |                                           |                                   |
## LLDP Configuration Commands
- LLDP is usually globally disabled by default.
- LLDP is also disabled on each interface by default.
- Separating the send and receive allows a device to be able to both send and receive or just do one of the functions.

| Number | Reason                                                                                                                  | Command                            |
| ------ | ----------------------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| 1      | Enable LLDP globally                                                                                                    | R(config)# lldp run                |
| 2      | Enable/Disable LLDP on specific interfaces (tx - transmit)                                                              | R(config)# [no] lldp transmit      |
| 3      | Enable/Disable LLDP on specific interfaces (rx - receive)                                                               | R(config)# [no] lldp receive       |
| 4      | Configure the LLD timer                                                                                                 | R(config)# lldp timer [seconds]    |
| 5      | Configure the LLDP holdtime                                                                                             | R(config)# lldp holdtime [seconds] |
| 6      | Configure the LLDP reinit timer                                                                                         | R(config)# lldp reinit [seconds]   |
| 7      | Shows the status, advertisements, hold time, interface reinit configurations                                            | R# show lldp                       |
| 8      | Shows the LLDP traffic statistics.                                                                                      | R# show lldp traffic               |
| 9      | Show the interfaces with LLDP applied and if it has Tx and Rx applied and the state they are in                         | R# show lldp interface             |
| 10     | Show lldp neighbors. Follows the same pattern as CDP with device ID, local interface, Hold-time, capability and port ID | R# show lldp neighbors             |
| 11     | Shows the operating system information, holdtime, system capabilities, enabled capabilities, etc.                       | R# show lldp neighbors detail      |
| 12     | Display the same info as show lldp neighbors detail but only for the entry                                              | R# show lldp entry name            |
### LLDP neighbors important information
- Device ID
	- show the neighbour device ID
- Local Interface
	- show the interface the neighbour is connected to on the device
- Hold-time
	- show the remaining hold time for a neighbour device
- Capability
	- R - router
	- B - bridge/switch
	- W - WLAN access point
	- P - Repeater
	- S - station
- Port ID
	- tell you the portID of the neighbour device

## Wireshark Captures
- CDP
	- Destination under the 802.3 tab
		- shows the mac address of the protocol uses so you can identify if its CDP/LLDP
	- Under the CDP tab
		- TTL - hold time
		- There are tabs the show information of the device ID, software version, platform, addresses, portID, capabilities, duplex, etc.
- LLDP
	- Destination under 802.3 tab shows 0180.c200.000e
	- under the lldp tab
		- time to live is the hold time
		- shows system name, port description, capabilities, enabled capabilities, end of LLDPDU