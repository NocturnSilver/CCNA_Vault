#layer3


## Static NAT
- translates a single inside local address to a single inside global address or a single outside local address to a single outside global address

## Dynamic NAT
- translates local addresses to global addresses that are all allocated from a pool.
## NAT Overloading or PAT
- Uses ports to translate inside local address to one or more inside global addresses. NAT router uses port numbers to keep track of which packets belong to each host.
- NAT overloading is also called Port Address Translation (PAT)

## NAT Overlapping
- use it when the addresses on an internal network conflict with the addresses on another network
- The internal addresses must be translated to unique addresses on the external network
- Addresses on the external network must be translated to unique addresses on the internal network
- translation can be performed by either static dynamic N

## Commands

| number | reasons                                                                 | commands                                                                              |
| ------ | ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| 1      | Configure a static inside local to inside global IP address translation | ip nat inside source static \[inside-local] \[inside-global]                          |
| 2      | Configure outside local to outside global                               | ip nat outside source static \[outside global] \[outside-local]                       |
| 3      | Creates a NAT pool                                                      | ip nat pool \[nat-pool] \[start-ip] \[end-ip] {netmaask mask \| prefix-length prefix} |
| 4      | Enable Translation of inside local addresses                            | ip nat inside source list [access-list] pool [nat-pool] \[overload]                   |
| 5      | Configure NAT overload or PAT with a single inside global address       | ip nat inside source list [access-list] interface [outside-interface] overload        |
| 6      | Configure NAT overload with a NAT pool                                  | ip nat inside source list [access-list] pool [nat-pool] overload                      |