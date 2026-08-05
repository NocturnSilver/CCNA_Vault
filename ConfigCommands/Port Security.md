#errdisableRecoveryCommands
#securityCommads
#showCommands

| Number | Reason                                                        | Command                                                                               |
| ------ | ------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| 1      | Configures the port security violation mode                   | (config-if)# switchport port-security violation {shutdown \| restrict \| protect}     |
| 2      | Enable Port Security on an interface                          | (config-if)# switchport port-security                                                 |
| 3      | Manually configure a **secure** MAC address on the interface. | (config-if)# **switchport port-security mac-address** _mac-address_                   |
| 4      | Enable sticky secure MAC address learning:                    | (config-if)# **switchport port-security mac-address sticky**                          |
| 5      | Configure the port security aging time                        | (config-if)# **switchport port-security aging time** _minutes_                        |
| 6      | Configure the port security aging type                        | (config-if)# **switchport port-security aging type** {**absolute** \| **inactivity**} |
| 7      | Enable aging of static secure MAC addresses                   | (config-if)# **switchport port-security aging static**                                |
|        |                                                               |                                                                                       |
## ErrDisable Recovery and Reset Commands

| 1   | Enable errdisable recovery for port security violations   | (config-if)# errdisable recovery cause psecure-violation |
| --- | --------------------------------------------------------- | -------------------------------------------------------- |
| 2   | Configure the errdisable recovery interval                | (config)# errdisable recovery interval seconds           |
| 3   | Manually re-enable an interface disabled by port security | (config-if)# shutdown<br>(config-if)# no shutdown        |
|     |                                                           |                                                          |

## Show Commands
| 1   | Show all secure MAC addresses on the switch                 | # show mac address-table secure            |     |
| --- | ----------------------------------------------------------- | ------------------------------------------ | --- |
| 2   | Show a summary of port security-enabled switchports         | # show port-security                       |     |
| 3   | Show port security information for an individual switchport | # show port-security interface [interface] |     |
| 4   | Show a summary of errdisable recovery information           | # show errdisable recovery                 |     |
