
## Dynamic Routing Protocol

| Route Source             | AD  |
| ------------------------ | --- |
| Directly connected route | 0   |
| Static Route             | 1   |
| EIGRP summary route      | 5   |
| eBGP                     | 20  |
| Internal EIGRP           | 90  |
| IGRP                     | 100 |
| OSPF                     | 110 |
| IS-IS                    | 115 |
| RIP                      | 120 |
| External EIGRP           | 170 |
| iBGP                     | 200 |
| Unknown                  | 255 |
## FHRP
| Protocol | syntax         | IP address                       | MAC address                              |
| -------- | -------------- | -------------------------------- | ---------------------------------------- |
| HSRP     | active/standby | V1: 224.0.0.2<br>V2: 224.0.0.102 | V1: 0000.0c07.acxx<br>V2: 0000.0c9F.Fxxx |
| VRRP     | mastery/backup | 224.0.0.18                       | 000.5e00.01xx                            |
| GLBP     | AVG/AVE        | 224.0.0.102                      | 0007.6400.xxyy                           |
