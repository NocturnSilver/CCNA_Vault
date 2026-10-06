#layer5

## Troubleshooting
- A Cisco router can be configured as a DNS server and a DNS client at the same time

## What is DNS
- used to resolve human-readable names (google.com) to IP addresses.
- DNS server(s) your device uses can be manually configured or learned via DHCP.
- Uses port 53

## Why DNS
- Names are easier for humans to remember, even if machines use addresses.

## How Does DNS work?
- devices will have the DNS server's response to a local DNS cache. This means that they don't have to query the server every single time they want to access a particular destination

## Wireshark Capture
- DNS 'A' record - used to map names to IPv4 addresses
- DNA 'AAAA' record = Used to map names to IPv6 addresses
- Standard DNS queries/responses typically use UDP
- TCP is used for DNS messages greater than 512 bytes


### Windows Commands
- in Windows/System32/driver/etc/hosts - you can manually type DNS (ip_addr dom_name)

| Number | Reason                                                                                           | Command                                         |
| ------ | ------------------------------------------------------------------------------------------------ | ----------------------------------------------- |
| 1      | Shows the Physical address, IPv4, Subnet Mask as well as the PCs DNS server                      | > ipconfig /all                                 |
| 2      | View DNS cache                                                                                   | > ipconfig /displaydns                          |
| 3      | Flush the DNS cache                                                                              | > ipconfig /flushdns                            |
| 4      | Translates a domain name to its ip address from the DNS server. Also displays the PCs DNS server | > nslookup [dom-name]                           |
| 5      | Ping n number of packets to an address                                                           | > ping [dom_name \| ip_addr] -n \[# of packets] |
### DNS in Cisco IOS configs
- if you ping a device with a default domain it will ping the default domain
	- ping pc1 -> ping pc1 \[default_domain_name]
- the old version of the command: ip domain-name
- The steps below are in order
- not that most of the commands dont have dns prefixed after ip for the command

| Number | Reason                                                                                                                      | Command                                      |
| ------ | --------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| 1      | Configure a Router to act as a DNS server                                                                                   | R(config)# ip dns server                     |
| 2      | Configure a list of hostname/IP address mappings                                                                            | R(config)# ip host \<dom-name> \<ip_address> |
| 3      | Configure a DNS server that R1 will query if the requested record isn't in its host table                                   | R(config)# ip name-server \<ip-address>      |
| 4      | Enable the router to perform DNS queries. It is enabled by default. The old version has a dash in between domain and lookup | R(config)# ip domain lookup                  |
| 5      | Configure the default domain name                                                                                           | R(config)# ip domain name \[default_domain]  |

### Show commands for Cisco IOS
| Number | Reason                                                                 | Command       |
| ------ | ---------------------------------------------------------------------- | ------------- |
| 1      | View configured hosts, as well as the hosts learned and cached via DNS | R# show hosts |
|        |                                                                        |               |
