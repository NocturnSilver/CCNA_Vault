#layer2
#layer2sec

## Maximum number of devices
- The default maximum number of devices when port security is activated is 1. Therefore, all traffic from another host will be discarded, or port will shutdown.

## Configuration of Static MAC addresses

| Number | Reason                                                                                                                                                                          | Command                                                                                    |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| 1      | configure an interface to <br>- discard traffic from unauthorized hosts. <br>- Remains up if more than max number of addresses is learned                                       | SW(config-if)# switchport port-security violation protect                                  |
| 2      | configure an interface to <br>- remain up if more than the max number of addresses is learned, <br>- the traffic from violating devices is dropped and a log entry is generated | SW(config-if)# switchport port-security violation restrict                                 |
| 3      | configure an interface to shutdown if more than the maximum allowed MAC addresses are learned on the interface                                                                  | SW(config-if)# switchport port-security violation shutdown                                 |
| 4      | Configure max number of authorized MAC addresses for a particular interface                                                                                                     | SW(config-if)# switchport port-security maximum [number]                                   |
| 5      | Remove MAC address from an interface                                                                                                                                            | SW(config-if)# clear port-security dynamic [address mac-address \| interface type mod/num] |
|        |                                                                                                                                                                                 |                                                                                            |

## Effects of Issuing Port Security 
- Sticky address learning is disabled
- A maximum of one MAC address will be allowed on the port
- the shutdown violation mode is enabled
- Port security aging time is configured zero
- Port security static aging is disabled
- Port security aging type is configured to absoluteproto