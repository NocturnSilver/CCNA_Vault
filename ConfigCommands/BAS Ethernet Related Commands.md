
## Commands to clear dynamic MAC address tabe
- This is done to forget learned device locations and relearn the connections
- It allows for clearing stale entries
- If a MAC spoofing attack occurs or unauthorized access happens
- testing and verifications

| Number | Reason                                                                             | Command                                                       |
| ------ | ---------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| 1      | Clear all dynamic MAC addresses from the MAC address table                         | SW1# clear mac address-table dynamic                          |
| 2      | Clear all dynamic MAC addresses on a specific interface from the MAC address table | SW1# clear mac address-table dynamic interface [interface-id] |
| 3      | Clear all entries for a specific dynamic MAC from the MAC address table            | SW1# clear mac address-table dynamic address [mac-address]    |
|        |                                                                                    |                                                               |

## Show Commands
| Number | Reason                               | Command                     |
| ------ | ------------------------------------ | --------------------------- |
| 1      | View MAC address Table               | SW1# show mac address-table |
| 2      | Show the arp table for Cisco Devices | \# show arp table           |
| 3      |                                      |                             |
|        |                                      |                             |
