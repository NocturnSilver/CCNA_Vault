#wireless
#etherChannel
#portfast
#vlan
#SVI
#DHCP
## Switch config

| Number | Reason                                                                 | Commands                                 |
| ------ | ---------------------------------------------------------------------- | ---------------------------------------- |
| 1      | create a VLAN                                                          | SW(config)# vlan [vlan-number]           |
| 2      | Name the Vlan being created                                            | SW(config)# name [name]                  |
| 3      | for a Split-Mac architecture, we want switchport mode access to the AP | SW(config-if)# switchport mode access    |
| 4      | Assign the switchport to a VLAN                                        | SW(config-if)# switchport access vlan 10 |
| 5      | Activate portfast on the access ports on the dege                      | SW(config-if)# spanning-tree portfast    |

### Create and EtherChannel
| Number | Reason                                                                                                                                                               | Commands                                                    |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| 1      | Create an EtherChannel for the Wireless LAN controller (WLC) and assign the interface into the channel group (note that WLCs only suport static LAG no PAgP or LACP) | SW(config-if)# channel-group [group-number] mode [mode]     |
| 2      | Configure or enter the logical port-channel interface group number. Allows one to apply settings like trunking or IP addresses to the aggregated bundle              | SW(config-if)# interface port-channel [group-number]        |
| 3      | Make the switchport into a trunk port for the WLC to have more bandwidth and redundancy                                                                              | SW(config-if)# switchport mode trunk                        |
| 4      | Allow the VLANs for the WLC.                                                                                                                                         | SW(config-if)# switchport trunk allowed vlan [vlan-numbers] |


### Configure SVI
- The SVI is established between the switch and the WLC. SVI is needed since we are segratign the WLANs/VLANs into management (vlan10), Internal (vlan100), and Guest(vlan200). Each having a different SSID and subnet

| Number | Reason                                                                                    | Commands                                                                                                                             |
| ------ | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| 1      | Configure an SVI or each VLAN. This will be used as the default gateway for their subnets | SW(config)# interface vlan [vlan-num]                                          <br><br>SW(config-if)# ip address [ip-addr] [netmask] |
| 2      | Allow the VLANs for the WLC.                                                              | SW(config-if)# switchport trunk allowed vlan [vlan-numbers]                                                                          |
| 3      | Configure an SVI or each VLAN. This will be used as the default gateway for their subnets | SW(config)# interface vlan [vlan-num]                                          <br><br>SW(config-if)# ip address [ip-addr] [netmask] |

### Configure DHCP
- DHCP will be used for endhosts when they use the Access Points

| Number | Reason                                                                                                                                                                                                       | Commands                                        |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------- |
| 1      | Create a DHCP pool and add a name to the pool                                                                                                                                                                | SW(config)# ip dhcp pool [VLAN-NAME]            |
| 2      | add network range to the DHCP pool                                                                                                                                                                           | SW(dhcp-config)# network [ip-address] [netmask] |
| 3      | Setup the default gateway and use the SVI as the default gateway IP address                                                                                                                                  | SW(dhcp-config)# default-router [ip-addr]       |
| 4      | Option 43 can be used to tell the APs the IP address of their WLC. Not necessary in certain cases when the APS and WLC are in the same subnet. The WLC will hear the APs broadcast CAPWAP discovery messages | SW(dhcp-config)# option 43 ip [ip-addr]         |
### Configure NTP
| Number | Reason                                                                                                    | Commands               |
| ------ | --------------------------------------------------------------------------------------------------------- | ---------------------- |
| 1      | This will make the switch an NTP server. However, you have to tell the WLC to use the SW as an NTP server | SW(config)# ntp master |



## WLC configuration
- During bootup, the WLC will guide you through the installation settings.
	- Terminate the autoinstall by either typing yes or just enter
	- System name has to then be entered as well as the admin user name and password. A re-enter password also appears
	- Enable link aggregation: yes (pressing enter makes it a default NO)
	- Enter values for management interface
		- the IP address
		- the netmask
		- The default router
		- the VLAN indentifier
		- The DHCP Server IP addr
	- Virtual Gateway IP address:
		- is an ip address used when the WLC is communicating directly with wireless clients such as when relaying DHCP requests
	- Multicast IP address:
		- The multicast ip address is used when forwarding traffic to its APs
		- note: an ip address in the class D range is selected since it's reserved for multicast addresses
	- Mobility/RF Group name:
		- is used when you have multiple WLCs and you want them to work together.
	- Network Name (SSID): [Internal/external/management]
	- Configure DHCP Bridging mode \[yes]\[NO] no
	- Allow Static IP Addresses \[yes]\[NO] : yes
	- Configure a Radius server now? \[yes]\[NO]: no
		- Warning! the default WLAN security policy requires a RADIUS server. Please see documentation for more details.
	- Entry Country Code list (enter 'help' for a list of countries) [US]: \[Country-Code]
		- Look at the model name ex: AIR-CAP3502I-E-K9. The E is the regulatory domain of the device E indicates Europe
		- If the regulatory domain of the country specified in the WLC configuration doesn't match the regulatory domain of the AP, the AP won't be able to join the WLC.
		- https://www.cisco.com/c/dam/assets/prod/wireless/wireless-compliance-tool/index.html to check the regulatory domain of each country.
	- Enable 802.11b \[yes]\[NO]: yes
	- Enable 802.11a \[yes]\[NO]: yes
	- Enable 802.11g \[yes]\[NO]: yes
	- Enable auto-RF \[yes]\[NO]: yes
		- allows the WLC to automatically select which channels to use and how much transmit power to use. This is much better than doing everything manually.
	- Configure a NTP server now? \[YES]\[NO]: yes
	- Enter an NTP server''s IP address 192.168.1.1
	- Enter a polling interval between 3600 and 604800 secs: 3600
	- Configuration correct? If yes, system will save it and reset. \[yes]\[NO]: yes

- Since the WLC has a complete ip address in the management VLAN. We can connect to the WLC's by connecting the PC to the SW instead and using HTTP/HTTPS to access the WLC's GUI in a web browser. 
	- type the ip address and type the admin username and password
	- it should show interfaces that are up (green) and interfaces that are down (red) those green interfaces (setup for the demo) are forming a LAG and are connected to the switch. 
	- There is a summary of some information about the controllers, such as its management IP, system name, up time, temp, CPU, memory usage
	- There is a summary of the access points that have joined the WLC

### WLC configuration through web browser
- Create all the necessary interface for all the VLANs and fill in the details
	- go to controller tab
	- click on interfaces on the left
	- go to the controller tab on the top
		- on the top right corner click new
		- fill the interface name and VLAN ID
		- fill in the interface address and DHCP server address
- Configuring WLANs
	- click on the WLAN tab on the top part of GUI
		- some WLANs may already exist from the console configs
		- we have to configure preshared key authentication for the CCNA
		- click on the WLAN ID number on the leftmost column
			- ### In the general tab 
				- map the interface to the correct VLAN 
				- check status if it is enabled
			- ### In the security tab
				- layer 2 security tab allows the user to choose things like WEP, 802.1xm different version of WPA, etc.  Currently its WPA+WPA2 which is what should be selected for the CCNA
				- #### In authentication key management
					- change to PSK for SOHO
					- fill in the PSK format in either ASCII or HEX password should be in the range of 8 to 63 chars in length
				- #### we wont configure layer 3, but there is the web policy tab
					- However, when configured we can keep the layer 2 authentication as open. This is common for public wifi (like a cafe)
					- ##### web authentication
						- after the wireless clients get and IP address and tries to access a web page, they will have to enter a username and password to authenticate
					- ##### web passthrough 
						- similar to the above, but no username or password are required. A warning or statement is displayed and the client simply has to agree to gain access to the internet. 
					- ##### conditional and splash page
						- web direct options are similar, but additionally require 802.1x layer 2 authentication
				- #### There's a AAA tab, but since we are using PSK authentication, there's no need to set up anything like a RADIUS server.
			- ### In the QoS tab
				- QoS settings is silver (best effort) by default
					- platinum should be use for Voice (wifi ip phones)
					- gold should be used for video traffic
					- bronze is given low priority (for background traffic)
			- ### In the advanced tab
				- we can configure maximum number of clients (0 = no maximum)
				- enable flex connect
- ## Under the Monitor Tab at the top 
	- ### Check the Client summary
		- checks how many devices are connected with the associated AP
	- ### Click on Clients on the left side
		- it shows the list of clients and their
			- client MAC addr
			- IP address
			- AP name
			- WLAN profile
			- WLAN SSID
			- User Name
			- Protocol
			- Status
- ## Click on the Wireless tab
	- We are shown the list of APs that have joined the WLC
		- we are shown the AP name, IP addr, AP mac, AP up time, admin status, operational status
		- ### Click on AP name to check on the settings of the AP
			- check AP mode - it is the AP operational mode
- ## Check the Management Tab
	- first you can see the summary of the management settings   
		- you can see the ff:
			- which SNMP versions are enabled/disabled
			- if syslog is enabled/disabled
			- if HTTP mode is enabled/disabled
			- If HTTPS mode is enabled/disabled
			- If new telnet sesions are allowed: yes/no (best to leave disabled)
			- If new SSH sessions are allowed: yes/no
			- if maangement via wireless is allowed: enabled/disabled
	- ### Click on Mgmt via wireless on the left tab
		- enable controller management to be accessible from wireless clients
- ## Check the Security Tab
	- ### Click on the Access Control List
		- give the ACL a name and chose the ACL tpye (IPVS4 or IPV6)
		- #### on the top right click add new rule
			- add a rule on the top right to specify what traffic can access the WLC
				- This is where we specify the sequence number, source and destination ip address, protocol, DSCP, Direction, Action
	- ### Click on the CPU Access Control Lists
		- CPU ACLs are used to limit access to the CPU of the WLC. This limits which devices will be able to connect to the WLC via Telnet/SSH, HTTP/HTTPS, retrieve SNMP information from the WLC, etc. This doesn't affect traffic passing through the WLC, only traffic destined directly for the WLC.
		- This is to apply the ACL
		- click the checkbox for enable CPU ACL.
		- Give the ACL a name.
		- Click apply
		- 
		

| Number | Reason                                                                                                                                                                                                       | Commands                                        |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------- |
| 1      | Create a DHCP pool and add a name to the pool                                                                                                                                                                | SW(config)# ip dhcp pool [VLAN-NAME]            |
| 2      | add network range to the DHCP pool                                                                                                                                                                           | SW(dhcp-config)# network [ip-address] [netmask] |
| 3      | Setup the default gateway and use the SVI as the default gateway IP address                                                                                                                                  | SW(dhcp-config)# default-router [ip-addr]       |
| 4      | Option 43 can be used to tell the APs the IP address of their WLC. Not necessary in certain cases when the APS and WLC are in the same subnet. The WLC will hear the APs broadcast CAPWAP discovery messages | SW(dhcp-config)# option 43 ip [ip-addr]         |
