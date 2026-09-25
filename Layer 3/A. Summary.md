 
## Dynamic Routing Protocols

|                                    | EIGRP                                            | OSPF                                                                                  | RIP                          |
| ---------------------------------- | ------------------------------------------------ | ------------------------------------------------------------------------------------- | ---------------------------- |
| description                        | Enhanced Interior Gateway Protocol               | Open Shortest Path First                                                              | Routing Information Protocol |
| metric                             | slowest link + delay of all links                | cost = total cost of outgoing interfaces<br>reference bandwidth / interface bandwidth | Number of Hops               |
| Administrative <br>Distance        | 90                                               | 110                                                                                   | 120                          |
| versions                           | v0<br>v1 - modern version                        | OSPFv1<br>OSPFv2<br>OSPFv3                                                            | RIPv1<br>RIPv2<br>RIPng      |
| Router-ID<br>order or priority<br> |                                                  |                                                                                       |                              |
| Messages                           |                                                  | Hello<br>DBD<br>LSR<br>LSU<br>LSAck                                                   |                              |
| Broadcast address                  | v1 255.255.255.255<br>v2 224.0.0.9<br>ng FF02::9 | 224.0.05                                                                              |                              |
| Timers                             |                                                  | Hello: 10s<br>Dead: 40s                                                               |                              |
|                                    |                                                  |                                                                                       |                              |
