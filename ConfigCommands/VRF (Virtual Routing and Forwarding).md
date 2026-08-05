- Virtual Routing and Forwarding is applied on a router.
## 1. VRF commands

| Number | Reason                                                                                                     | Command                                     |
| ------ | ---------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| 1      | Create a VRF                                                                                               | (config)# ip vrf [vrf-name]                 |
| 2      | Assign interfaces in each VRF                                                                              | (config-if)# ip vrf forwarding vrf-name     |
| 3      | Configure the IP address after assigning the interface to the VRF since it removed existing IP address too | (config-if)# ip address [ip-addr] [netmask] |
| 4      | Make sure to turn the interface on in case it was set to shutdown                                          | (config-if)# no shutdown                    |
| 5      | Test the Reachability of devices in a VRF                                                                  | # ping vrf [vrf-name] [ip-addr]             |
|        |                                                                                                            |                                             |
## 2. Show commands

| Number | Reason                                                                             | Command                        |
| ------ | ---------------------------------------------------------------------------------- | ------------------------------ |
| 1      | Shows the following columns: VRF name, Default RD, Interfaces assigned to the VRF  | # show ip vrf                  |
| 2      | Check the routing table of a VRF since a VRF does not share a Global routing table | # show ip route VRF [vrf-name] |
| 3      | Test the Reachability                                                              | # ping [ip-addr]               |

### Steps
- Create VRFs
- Assign interfaces to VRFs
- Check if the VRF is created and interfaces assigned
### Notes:
- Without the use of VRF, two interfaces on the same router cannot be in the same subnet
- If an interface has an IP address configured, the IP address will be removed when you assign the interface to a VRF.
- Show ip route will not display routes since it displays the global routing table
- You can have a mix of interfaces using and not using VRFs
- If an interface is not in a VRF its routes will appear in the global routing table, and it will be isolated from the interfaces in VRFs, just like interfaces in different VRFs are isolated from each other