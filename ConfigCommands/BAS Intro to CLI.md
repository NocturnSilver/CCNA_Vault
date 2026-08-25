
## Basic Commands

| Number | Reason                                                                                                 | Command                                     |
| ------ | ------------------------------------------------------------------------------------------------------ | ------------------------------------------- |
| 1      | Configure and MD5-encrypted password                                                                   | Router(config)# enable secret [password]    |
| 2      | Configure an unencrypted password to protect privileged EXEC mode                                      | Router(config)# enable passowrd [password]  |
| 3      | Encrypt current and future passwords on the device                                                     | Router(config)# service password-encryption |
| 4      | Enter global configuration mode                                                                        | \# configure terminal                       |
| 5      | Enter Privilege EXEC mode                                                                              | \# enable                                   |
| 6      | Return to a level lower or to privileged EXEC mode                                                     | (config)# exit                              |
| 7      | Keyword to remove a configured command can be used in any mode followed by command to negate           | \# no                                       |
| 8      | View the available commands. Can be used to show complete command or what follows after a command too. | ?                                           |
| 9      | Keyword to execute a privileged EXEC command when in a config mode                                     | do                                          |
## Show Commands
| Number | Reason                         | Command                     |
| ------ | ------------------------------ | --------------------------- |
| 1      | View the running Configuration | Router# show running-config |
| 2      | View the startup-configuration | Router# show startup-config |
## Save Configuration Commands

| Number | Reason                                                          | Command                                                                                   |
| ------ | --------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| 1      | Save the current running configuration to startup configuration | Router# write<br><br>Router# write memory<br><br>Router# cp running-config startup-config |

