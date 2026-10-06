#layer5


## Definitions
- stratum - distance of an NTP server from the original reference clock
- reference clocks - very accurate clocks at stratum 0.
- primary servers - NTP servers that get their time directly from reference clocks
- secondary servers - servers which get their time from other NTP servers. They operate in server more and client mode at the same time

## Context
- all devices have an internal clock
- Default stratum for the NTP master command is 8
- * - in the output of show clock means that the time is not authoritative

## Troubleshooting
- the hardware calendar is the default-time source, however, the hardware clock and the software clock are separate and can be configured separately.
- It is recommended to sync the clock and the calendar.
- The hardware clock tracks the date and time on the device even if it restarts, power is lost, etc. When the system is restarted, the hardware clock is used to initialise the software clock
- when configuring NTP master with the stratum number, note that on show ntp associations it shows -1 the number you set
## What is NTP
- allows automatic syncing of time over a network

## Why do we use NTP?
- accurate time on a device gives accurate logs for troubleshooting
- manually configured clocks will drift

## NTP Mechanics
- NTP clients request the time from NTP servers
- A device can be an NTP server and an NTP client at the same time
- NTP allows accuracy of time within ~1 ms if the NTP server is the the same LAN, or within ~50ms if connecting to the NTP server over a WAN/the Internet
- Some NTP servers are better than others. The 'distance' of an NTP server from the original reference clock is called the stratum.
- NTP uses UDP port 123
- Symmetric active mode - Devices can also peer with devices at the same stratum to provide more accurate time

### Cisco Device NTP modes
- Server mode
- Client mode - can sync to multiple NTP servers
- Symmetric active mode - can peer with other devices at the same stratum

### Reference Clocks
- usually very accurate time devices like an atomic clock or a GPS clock
- Reference clocks are stratum 0 within the NTP hierarchy
- NTP servers directly connected to reference clocks are startum 1

### NTP Hierarchy
- stratum 0 - reference clocks
- stratum 1-  Primary Servers, NTP servers get their time from reference clocks
- stratum 2 - Secondary Servers, NTP servers get their time from stratum 1 NTP servers
- stratum 3 - Secondary Servers, NTP servers get their time from stratum 2 NTP servers
- stratum 15  - Secondary Servers, maximum. Anything above is considered unreliable.


## Commands
- NTP peer - configured active-symmetric mode 
- NTP master - configures server more
- NTP server - configures client mode

### Clock Config Commands
- When configuring daylight saving we can think of it as
	- clock summer-time \[start-of-DST] \[end-of-DST] \[offset]
	- in each DST field you have to specify the following in the order below
		1. week to stard/end
		2. day to start/end
		3. month to start/end
		4. time to start/end <hh:mm>

| Number | Reason                                                                                                                                                              | Command                                                                                                                                                   |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | Manually set the time one the device                                                                                                                                | R# clock set \[hh:mm:ss] <1-31> \<Jan-Dec> \<YYYY>                                                                                                        |
| 2      | Manually set the hardware clock                                                                                                                                     | # calendar set \[hh:mm:ss] <1-31> \<Jan-Dec> \<YYYY>                                                                                                      |
| 3      | Synchronise the calendar to the clock time                                                                                                                          | # clock update-calendar                                                                                                                                   |
| 4      | Synchronise the clock to the calendar's time                                                                                                                        | # clock read-calendar                                                                                                                                     |
| 5      | Configure the timezone                                                                                                                                              | (config)# clock timezone \[time zone] <br><-23 - 23 hours offset from UTC> [minutes-offset]                                                               |
| 6      | Configuring daylight saving time (summer time). Where [time-zone] is the name of the timezone in summer. week-#-to-start can use \<first\|last> instead of a number | # clock summer-time [time-zone] <date \| recurring> \<week-#-to-start>  \<day> \<month> <hh:mm> \<week-#-to-end> \<day> \<month> \<hh:mm> <offset-in-min> |
|        |                                                                                                                                                                     |                                                                                                                                                           |

### Windows Commands
| Number | Reason                                                                                                                                                          | Command          |
| ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- |
| 1      | tool used to query the DNS. Retrieves IP adressess or specific DNS records for a domain name. We can lookup the server where we get our time (time.windows.com) | > nslookup [dns] |
### NTP configuration commands

| Number | Reason                                                                                                                                                                     | Command                                          |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| 1      | configure the NTP server to use. Add the word prefer at the end to prefer an NTP server compared to other NTP servers.                                                     | R#(config)# ntp server [ip-addr] [prefer]        |
| 2      | Configure a source IP address for all outgoing NTP packets. Using a stable interface like a loopback prevents time synchronisation loss if a physical interface goes down. | R# ntp source [interface ip-addr \|  loopback_#] |
### NTP server mode commands

| Number | Reason                                                                                                         | Command                           |
| ------ | -------------------------------------------------------------------------------------------------------------- | --------------------------------- |
| 1      | Manually configure a Cisco device to operate as an NTP server, even if it hasn't synced to another NTP server. | R#(config)# ntp master \<stratum> |
### Configuring NTP symmetric active mode
| Number | Reason                                                                                                         | Command                       |
| ------ | -------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| 1      | Manually configure a Cisco device to operate as an NTP server, even if it hasn't synced to another NTP server. | R(config)# ntp peer [ip-addr] |

### Configure NTP authentication
| Number | Reason                                                                                           | Command                                                  |
| ------ | ------------------------------------------------------------------------------------------------ | -------------------------------------------------------- |
| 1      | Enable NTP authentication                                                                        | R(config)# ntp authenticate                              |
| 2      | Create the NTP authentication key(s)                                                             | R(config)# ntp authentication-key [key-number] md5 [key] |
| 3      | Specify the trusted key(s)                                                                       | R(config)# ntp trusted-key key-number                    |
| 4      | Specify which key to use for the server. Don't need to specify this if this is the server itself | R(config)# ntp server \[ip-address] key \[key-number]    |


### Show commands

| Number | Reason                                                                              | Command                  |
| ------ | ----------------------------------------------------------------------------------- | ------------------------ |
| 1      | shows the time, timezone, day, date                                                 | R# show clock            |
| 2      | Shows the hardware clock                                                            | R# show calendar         |
| 3      | shows more including time source                                                    | R# show clock detail     |
| 4      | show the NTP server ip addresses configured to the device                           | R# show ntp associations |
| 5      | show if the clock is synced, the stratum, and the reference NTP server's ip address | R# show ntp status       |
| 6      | Show the logs of the device                                                         | R# show logging          |
