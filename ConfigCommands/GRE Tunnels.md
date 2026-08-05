Goal: Connect two routers together
- We can assume since it's two routers it can't be a remote access VPN
- It should be a site-to-site VPN using IPsec using GRE tunnel

Tun

## Tunnel configuration

| Number | Reason                                                                             | Command                                                                                       |
| ------ | ---------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| 1      | Create a virtual interface for tunneling                                           | (config)# Interface tunnel 0                                                                  |
| 2      | Specify the interface that will be the sender/receiver of msges sent to the tunnel | (config-if)# tunnel source [interface connected to service provider]                          |
| 3      | Specify the ip-address of the other router you want to tunnel to (WAN ip-addr)     | (config-if)# tunnel destination [ip-address of other-end of the tunnel]                       |
| 4      | The Virtual interface itself needs an ip address. (Not the actual ip address)      | (config-if)# ip address [ip-addr] [netmask] - you can do a 252 for netmask for point to point |
| 5      | Setup a route for the tunnel using the WAN ip-address                              | (config)# ip route 0.0.0.0 0.0.0.0 [WAN-ip-addr]                                              |
| 6      | Activate OSPF for automatic routing                                                | (config)# router ospf 1                                                                       |
| 7      | Activate Range of Networks to add for OSPF Routing                                 | (config-router)# network [ip-addr] [wildcard-mask] area [area-number]                         |
| 8      | Declare passive-interfaces                                                         | (config-router)# passive-interface [interface]                                                |

### OSPF activation

| 1   | Activate OSPF for automatic routing                | (config)# router ospf 1                                               |
| --- | -------------------------------------------------- | --------------------------------------------------------------------- |
| 2   | Activate Range of Networks to add for OSPF Routing | (config-router)# network [ip-addr] [wildcard-mask] area [area-number] |
| 3   | Declare passive-interfaces                         | (config-router)# passive-interface [interface]                        |

### Note
1. Make sure to add network address of both the tunnel and the other interface inside the LAN in order for it to work properly.

## Show Commands

| Number | Reason                                                                      | Show Command        |
| ------ | --------------------------------------------------------------------------- | ------------------- |
| 1      | Check the status of the virtual tunnel interface                            | # do show ip int br |
| 2      | Check if there is a route between the two routers participating in a tunnel | # do show ip route  |

