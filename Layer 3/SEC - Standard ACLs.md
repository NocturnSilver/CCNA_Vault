#layer3

## Definition


## What are ACLs
- ACLs actually have a lot of functions, but in general they are a set of filter rules configured on network devices to permit and deny data packets based on predefined criteria
- They are an ordered sequence of ACEs (Access Control entries)

## Functions of ACL
- packet filter - instructs routers to permit or discard specific traffic
	- filter based on source/destination IP addresses, ports, etc.

## How is the ACL configured
- ACLs are configured on globally on the router
- However, they must be applied to an interface forit to take effect.
- They are applied either inbound or outbound
- A maximum of one ACL can be applied to a single interface per direction.
	- inbound - max 1
	- outbound - max 1

## How ACLs work
- When the router checks a packet against the ACL, it processes the ACEs in order, from top to bottom
- If the packet matches one of tehe ACEs in the ACL, the router takes the action and stops processing the ACL. All entries below the matching entry wil be ignored.
### Implicit Deny
- If a packet doesn't match any of the entries in an ACL there is an implicit deny at the end of all ACLs
- The implicit deny tells the router to deny all traffic that doesn't match any of the configured entries in the ACL
- There's a hidden (if source IP = any, then deny)
## ACL Types

### Standard ACLs
- match based on source IP address only (of the packet)
- Standard Numbered ACLs
	- Identified with a number (e.g. ACL 1, etc.)
	- range 1-99 and 1300-1999
- Standard Names ACLs

#### ACL Config Commands
- Standard ACLs should be applied as close to the destination as possible

| Number | reason                                                                                                                                                                                                                                        | Command                                                                                                               |
| ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| 1      | Configure a standard numbered ACL                                                                                                                                                                                                             | R(config)# access-list \[acl-num] {deny/permit} ip wildcard-mask                                                      |
| 2      | Another way to Configure a standard numbered ACL for a host                                                                                                                                                                                   | R(config)# access-list \[acl-num] {deny/permit} host [ip-address]                                                     |
| 3      | Create a standard named ACL. It enters the standard ACL config mode. In this mode all commands will work without specifying which ACL you are going to put the rule in. It's good to name it with the interface IP the ACl will be applied to | R(config)# ip access-list standard [name]<br><br>R(config-std-nacl)#\[entry-number] {deny \| permit} ip wildcard-mask |
| 4      | Create a remark for the access list                                                                                                                                                                                                           | R(config)# access-list \[acl-num] remark [remark]                                                                     |
| 5      | Shows the access list configured                                                                                                                                                                                                              | R# show [ip] access-lists                                                                                             |
| 6      | Apply the access-list on an interface                                                                                                                                                                                                         | R(config-if)# ip access-group [number] {in/out}                                                                       |
| 7      | Show the interfaces that the access-list is applied on                                                                                                                                                                                        | R# show running-config \| include access-list                                                                         |
| 8      | To view the whole access list and the remarks. the entry numbers aren't shown                                                                                                                                                                 | R# show running-config \| section access-list                                                                         |
|        |                                                                                                                                                                                                                                               |                                                                                                                       |
|        |                                                                                                                                                                                                                                               |                                                                                                                       |

### Extended ACLs
- Matched based on Source/Destination IP, Source/Destination port, etc.
- Extended Numbered ACLS
- Extended Named ACLs



