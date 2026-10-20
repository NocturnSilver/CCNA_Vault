#layer5
#SNMP

## Context
- Originally released in 1988 
- RFC 1065 - Structure and identification of management information for TCP/IP-based internets
- RFC 1066 - Management information base for network management of FCP/IP-based internets
- RFC 1067 - A simple network management protocol
- These three RFCs make up SNMPv1
- Management Information Base (MIB) - database that contains information about a managed device
- Network Management System (NMS) - used to communicate with a managed device
- UDP 161 - SNMP agent
- UDP 162 - SNMP manager
## What is SNMP (Simple Network Management Protocol)?
- Protocol that a network management system (NMS) can use to communicate with a managed device

## Why SNMP
- SNMP can be used to monitor the status of devices, make configuration changes, etc.

## Types of Devices in SNMP
- Managed Devices 
	- devices being managed using SNMP, network devices like routers and switches
- Network Management Station (NMS)
	- device/devices managing the managed devices
		- usually a server/computer
	- This is the SNMP server
## How does SNMP do what it does?
- uses a Mangement Information Base (MIB) to store information

### SNMP Operations
- There are three main operations used in SNMP
- Notify - Managed devices can notify the NMS of events
- Ask - The NMS can ask the managed devices for information about their current status
- Change - The NMS can tell the managed devices to change the aspects of their configurations

### SNMP Components
- NMS components
	- SNMP Manager - software on the NMS that interacts with the managed devices
		- it receives notifications, sends requests for information, sends configuration changes, etc.
	- SNMP application - provides an interface for the network admin to interact with.
		- displays alerts, statistics, charts, etc. 
- Managed Devices components
	- SNMP agent - the SNMP software running on the managed devices that interacts with the SNMP manager on the NMS
		- sends notification to/receives messages from the NMS
	- Management Information Base (MIB) - the structure that contains the variables that are managed by SNMP
		- each variable is identified with an Object ID (OID)
		- Example variables: interface status, traffic throughput, CPU usage, temperature, etc.

### SNMP OIDs (object IDs)
- SNMP Object IDs are organised in a hierarchical structure
	- .1.3.6.1.2.1.1.5
		- 1 - Iso
		- 3 - Identified-organisation
		- 6 - dod
		- 1 - internet
		- 2 - management
		- 1 - mib-2
		- 1 - system
		- 5 - sysName

## SNMP versions (most widely used)
- SNMPv1  - original SNMP
- SNMPv2c - Allows the NMS to retrieve large amounts of info in a single request, so it is more efficient
	- 'c' refers to the 'community strings' used as passwords in SNMPv1, removed in SNMPv2, and then aded back for SNMPv2c
- SNMPv3 
	- A much more secure version of SNMP that supports strong encryption and authentication. Whenever possible, this version should be used

## SNMP Messages
- Notify, ask, change operations in more concrete form

| Message Class | Description                                                                  | Messages                  |
| ------------- | ---------------------------------------------------------------------------- | ------------------------- |
| Read          | Messages send by the NMS to read information from managed devices.           | Get<br>GetNext<br>GetBulk |
| Write         | Messages sent by the NMS to change information on the managed devices.       | Set                       |
| Notfication   | Messages sent by the managed devices to alert the NMS of a particular event. | Trap Inform               |
| Response      | Messages sent in response to a previous message/request                      | Response                  |
### SNMP Read Messages
- Get - a request sent from the manager to the agent to retrieve the value of a variable (OID), or multiple variables. The agent will send a Response with current value of each variable.
- GetNext - a request sent from the manager to the agent to discover the available variables in the MIB.
- GetBulk - a more efficient version of the GetNext message (introduced in SNMPv2)
### SNMP Write Messages
- Set - a request send from manager to the agent to change the value of one or more variables. The agent will send a Response message with new values.
### SNMP 'Notification' Messages
- Trap - A notification sent from the agent to the manager. The manager does not send a Response message to acknowledge that it received the Trap, so these messages are 'unreliable'
- Inform - a notification message that is acknowledged with a Response message 

## SNMP Configuration
| Number | Reason | Command |
| ------ | ------ | ------- |
|        |        |         |

