#layer2
#vlan
#trunkport
## Context
- Trunk ports are used to carry traffic of more than one VLAN
- if port transmits and receives more than one VLAN then it should be made trunk
- by default all switch ports are access ports

## Trunk Encapsulation
- ISL - favored by DTP -
- 802.1q - 

## What does it do?
- it allows switches to negotiate status of switch ports
- Cisco proprietary protocol used for negotiating a trunk link between switches
- It is enabled by default on all CISCO switch interfaces

## Why do we want it?
- trunk ports are related to VLANs
- improve network performance by allowing switches to form trunk links automatically
- 

## How does it do it?
### Switch port modes
| Type                                                       | Reason                                                                                   | Command                                           |
| ---------------------------------------------------------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------- |
| Dynamic<br>Desirable<br>(default for older Cisco Switches) | forms a trunk if the other is either in switchport mode trunk, dynamic desirable, or aut | Switch(config)# switchport mode dynamic desirable |
| Dynamic<br>Auto (default for newer Cisco SW)               | Forms a trunk if the other is either in switchport mode trunk                            | Switch(config)#<br>switchport mode dynamic auto   |
| Trunk                                                      |                                                                                          |                                                   |
| Access                                                     |                                                                                          |                                                   |
### Other commands
| Type      | Reason                                                     | Command                                                                           |
| --------- | ---------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Desirable | Two commands that disable DTP negotiation on a switch port Switch(config)# switchport mode access<br>Switch(config)# switchport nonegotiate t  |
| Auto      |                                                                                                                                                |
|           |                                                                                                                                                |
|           |                                                                                                                                                |