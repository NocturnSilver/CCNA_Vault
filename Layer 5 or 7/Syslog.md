#layer5


## Message Format
seq no:timestamp: %facility-severity-MNEMONIC:description
00:00:46: %LINK-3-UPDOWN: Interface Port-channel1, changed state to up

## Severity Levels in Syslog
- console - limits messages logged to the console
	- by default the console receives debugging messages and numerically lower levels
- monitor - limits messaged logged to the terminal lines
	- by default the terminal receives debugging messages and numerically lower levels
- trap - limits messages logged to the syslog servers.
	- by default, syslog servers receive informational messages and numerically lower levels

| alert num | name          | command                        |
| --------- | ------------- | ------------------------------ |
| 0         | emergencies   | logging console/monitor/trap 0 |
| 1         | alerts        | logging console/monitor/trap 1 |
| 2         | critical      | logging console/monitor/trap 2 |
| 3         | errors        | logging console/monitor/trap 3 |
| 4         | warnings      | logging console/monitor/trap 4 |
| 5         | notifications | logging console/monitor/trap 5 |
| 6         | informational | logging console/monitor/trap 6 |
| 7         | debugging     | logging console/monitor/trap 7 |

## Commands

| num | reason                                                             | command                                                                                                                                               |
| --- | ------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | disable message logging                                            | (config-if)# no logging on                                                                                                                            |
| 2   | verifies entries                                                   | (config-if)# show running-config                                                                                                                      |
| 3   | verifies entries                                                   | (config-if)# show logging                                                                                                                             |
| 4   | enables log timestamps. Showing time since the system was rebooted | (config-if)# service timestamps log uptime <br><br>or<br><br>(config-if)# service timestamps log datetime \[msec] \[localtime] \[show-timezone]  <br> |
| 5   | Enable sequence numbers                                            | (config-if)# service sequence-numbers                                                                                                                 |
| 6   |                                                                    |                                                                                                                                                       |
|     |                                                                    |                                                                                                                                                       |