## Enabling SSH for virtual terminal (VTY) lines on a Cisco Router 

| step | Reason                                                                                                  | Command                                 |
| ---- | ------------------------------------------------------------------------------------------------------- | --------------------------------------- |
| 1    | Configure the router with a hostname other than router                                                  | Router(config)#hostname                 |
| 2    | Configure the router with a domain name                                                                 | Router(config)#ip domain-name           |
| 3    | Generate an RSA key pair for the router (The command will also automatically eneable SSH on the router) | Router(config)#crypto key generate rsa  |
| 4    | configure the VTY lines to use SSH                                                                      | Router(config-line)#transport input ssh |
