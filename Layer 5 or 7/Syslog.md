#layer5


## Troubleshooting
- without any specific syslog configurations, syslog messages are sent to the console line and buffer by default.
- when logging buffered n is used, syslog will show messages from 0-n
- depending on device configuration, seq and timestamp may not show

## What is Syslog
- industry standard protocol for message logging
- logs events such as changes in interface status, changes in OSPF neighbour status, system restarts, etc.
- Syslog and SNMP are both used for monitoring and troubleshooting of devices
- logs are essential when troubleshooting issues, examining the cause of incidents, etc.

## Syslog vs SNMP
- both are used for monitoring and troubleshooting of devices. complementary but different functions
- syslog - used for message logging
	- events that occur within the system are categorised based on facility/severity and logged
	- used for system management, analysis, and troubleshooting
	- messages are sent form the device to the server
	- Server cannot pull info from devices or modify variables
- SNMP - retrieve and organise info about SNMP managed devices
	- IP addresses, current interface status, temp, CPU usage, etc.
	- SNMP servers can use Get to query the clients and Set to modify variables on the clients

## Message Format
![[Pasted image 20261027074622.png]]
- example
	- 00:00:46: %LINK-3-UPDOWN: Interface Port-channel1, changed state to up

## Severity Levels in Syslog
- console - limits messages logged to the console
	- by default the console receives debugging messages and numerically lower levels
- monitor - limits messaged logged to the terminal lines
	- by default the terminal receives debugging messages and numerically lower levels
- trap - limits messages logged to the syslog servers.
	- by default, syslog servers receive informational messages and numerically lower levels

### Table of Syslog Severity
- RFC 5452 (Syslog protocol) - Because severities are very subjective, a relay or collector should not assume that all originators have the same definition of severity.
- Acronym - Every Awesome Cisco Engineer Will Need Ice cream Daily
	- Emergencies Are Critical Even When Nobody Is Dying

| alert num | name          | command                        | desc                                            |
| --------- | ------------- | ------------------------------ | ----------------------------------------------- |
| 0         | emergencies   | logging console/monitor/trap 0 | system is unusable                              |
| 1         | alerts        | logging console/monitor/trap 1 | Action must be taken                            |
| 2         | critical      | logging console/monitor/trap 2 | Critical conditions                             |
| 3         | errors        | logging console/monitor/trap 3 | Error conditions                                |
| 4         | warnings      | logging console/monitor/trap 4 | Warning conditions                              |
| 5         | notifications | logging console/monitor/trap 5 | Normal but significant condition (Notification) |
| 6         | informational | logging console/monitor/trap 6 | Informational messages                          |
| 7         | debugging     | logging console/monitor/trap 7 | Debug-level messages                            |
### Syslog Logging Locations
- console lines - syslog messages will be displayed in the CLI when connected to the device via the console port. By default, all messages (level 0 - 7) are displayed 
- VTY lines - Syslog messages will be displayed in the CLI when connected to the device via Telnet/SSH. Disabled by default
- buffer - Syslog messages will be saved to RAM. By default, all messages (level 0 - 7) are displayed. You can view the messages with show logging
- External server - you can configure the device to send syslog messages to an external server. 
	- syslog servers will listen form messages on UDP port 514
## Commands

### Syslog configurations
- to remember easily it follows the format:
	- logging console/monitor/host and trap

| num | reason                                                                                                                     | command                                                                                                         |
| --- | -------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| 1   | Configure logging to CONSOLE lines. The <0-7> is to signify which severity levels to enable logging                        | (config)# logging console <0-7>                                                                                 |
| 2   | Configure logging to VTY lines                                                                                             | (config)# logging monitor \<severity>                                                                           |
| 3   | Configure logging to the BUFFER. the size are in powers of 2. Suggested: 8192 or multiples of 1024                         | (config)# logging buffered \<size> <0-7>                                                                        |
| 4   | Configure logging to an external server. The first two commands are the same. We then need to set trap for external server | (config)# logging \<server-ip><br><br>config# logging host \<server-ip><br><br>config# logging trap \<severity> |
|     |                                                                                                                            |                                                                                                                 |
### Terminal monitor and Quality of life commands

| num | reason                                                                                                                                                                                                                                                                                                          | command                                                                                                                                                          |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Even if logging monitor level is enabled, by default syslog messages will not be displayed via Telnet or SSH. You must enter the command on the right for the messages to be displayed. Command must be used every time you connect to the device via Telnet/SSH                                                | R# terminal monitor                                                                                                                                              |
| 2   | Prevent logging messages to be displayed in the CLI while you are in the middle of typing a command. It will print in a newline                                                                                                                                                                                 | R(config)# line console 0<br><br>R(config-line)# logging synchronous                                                                                             |
| 3   | enables log timestamps. Showing time since the system was rebooted. we can stop at just specifying datetime without options.<br><br>datetime - timestamps will display the date/time when the event occured.<br><br>uptime - timestamps will display how long the devce had been running when the event occured | (config-if)# service timestamps log uptime <br><br>or<br><br>(config-if)# service timestamps log \<datetime\|uptime> \[msec] \[localtime] \[show-timezone]  <br> |
| 4   | Enable sequence numbers                                                                                                                                                                                                                                                                                         | (config-if)# service sequence-numbers                                                                                                                            |
###  show commands

| num | reason                  | command                          |
| --- | ----------------------- | -------------------------------- |
| 1   | disable message logging | (config-if)# no logging on       |
| 2   | verifies entries        | (config-if)# show running-config |
| 3   | verifies entries        | (config-if)# show logging        |
