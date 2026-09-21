## Context
- Lightweight Access Point Protocol (LWAPP) communications
- IEEE 802.11 standards used for wireless LANs
- Wi-Fi Alliance - trademarks Wi-Fi and tests and certifies equipment for 802.11 standards compliance interoperability with other devices.

## Wireless Issues
- all devices within range receives all frames, like devices connected to an ethernet hub
	- privacy of data within the LAN is a greater concern.
	- CSMA/CA (carrier sense multiple access with collision avoidance) is used to facilitate half-duplex communications

## 802.11 Header

## Wireless LAN Controller
- the service port interface of a wireless LAN controller (WLC) is the only available interface when the WLC is booting.
- WLC can contain up to 4 static interfaces:
	- management interface
		- used for in-band management information.
		- Used for all Layer 2 LWAPP communications between the controller and the lightweight APs
		- used to communicate with other WLCs on the wireless network
	- AP-manager interface
		- contains the IP address that is used as the source IP address by which the lightweight APs communicate with the WLC
		- Because the AP-manager interface communicates with lightweight APs on the wireless network, the IP address assigned to the AP-manager interface should be unique to the network
		- AP-manager uses this interface to listen for Lightweight Access Point Protocol (LWAPP) communications
	- virtual interface
		- supports mobility management by providing a specific internet protocol (IP) address that is the same across multiple controllers when wireless clients roam among the controllers
	- service port interface
		- used for maintenance purposes in a WLC
		- is a physical interface on the WLC that can be used to recover the WLC in the event that the WLC fails