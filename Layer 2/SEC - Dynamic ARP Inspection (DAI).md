#layer2
#layer2sec 
## Commands
| Number | Reason                                                                     | Command                                                   |
| ------ | -------------------------------------------------------------------------- | --------------------------------------------------------- |
| 1      | Configure Dynamic ARP inspection (DAI) globally for selected VLANs         | SW(config)#ip arp inspection vlan [vlan-ID \| vlan-range] |
| 2      | Configure a port to be in a VLAN                                           | SW(config-if)#switchport access vlan [vlan]               |
| 3      | configure an interface to be a trusted port after assigning it into a vlan | SW(config-if)# ip arp inspection trust                    |
|        |                                                                            |                                                           |