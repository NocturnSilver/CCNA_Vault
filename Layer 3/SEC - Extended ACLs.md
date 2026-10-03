#layer3

## Troubleshooting
- when configuring/editing numbered ACLs from global config mode, you can't delete individual entries, you can only delete the entire ACL

## Configuration tips
- Don't create sequences that are too close to each other since you may need to insert an entry between two entries (inserting something between 1 and 2 doesn't work. But inserting between 10 and 20 does)
- In extended ACLs, to specify a /32 source or destination you have to use the host option or specify the wildcard mask. You can't just write the address without either of those
- Extended ACLs should be applied as close to the source as possible, to limit how far the packets travel in the network before being denied

## What are extended ACLs
- they are just like standard ACLs, but they can match traffic based on more parameters, so they are more precise than standard ACLs
	- Can match based on Layer 4 protocol/port, source address, and destination address

## Advantages of named ACL config mode
- you can easily delete individual entries in the ACL with no entry-number
- You can insert new entries in between other entries by specifying the sequence number.


## IP protocol number Table
| IP Protocol Number | IP Protocol Name |
| ------------------ | ---------------- |
| 1                  | ICMP             |
| 6                  | TCP              |
| 17                 | UDP              |
| 88                 | EIGRP            |
| 89                 | OSPF             |

## Modifying ACL entries 

| Number | Reason                                                                                                                                            | Command                                                                         |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| 1      | Delete individual entries in the ACL<br>for numbered entires                                                                                      | R(config-std-nacl)# no [entry-number]                                           |
| 2      | Resequence the ACL. The starting-seq-number changes the first entry into that number. The increment then increments from the starting-seq-number. | R(config)# ip access-list resequence \[acl-id] \[starting-seq-num] \[increment] |
## Configuring Extended ACLs
- binary_op = eq, gt, lt, neq, range \[start-port-num] \[end-port-num]
- for port numbers refer to the port number table for TCP and UDP
- after the destination port, you could add ack, fin, syn, ttl, dscp to match for the flags
- If you specify all the things to match, it must match all

| Number | Reason                                                                                                        | Command                                                                                                                                                                        |
| ------ | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1      | Configure an extended access-list                                                                             | R(config)# access-list number [permit \| deny] protocol src-ip dest-ip                                                                                                         |
| 2      | Enter the extended access-list configuration mode                                                             | R(config)# ip access-list extended {name \| number}<br><br>                                                                                                                    |
| 3      | Configure an extended access list in the extended access list configuration mode<br><br>Configure with a port | R(config-ext-nacl)#  \[seq-num] \[permit \| deny] protocol src-ip dest-ip                                                                                                      |
| 4      | Standard form for Configuring an ACL with all parameters                                                      | R(config-ext-nacl)# {permit \| deny} {port-name \| port- number} \[src-ip  wild-card-mask] \[binary_op] \[src-port-num] \[dest-ip] \[binary_op] \[dst-port-num wild-card-mask] |

## Useful show commands
| Number | Reason                                                             | Commands                |
| ------ | ------------------------------------------------------------------ | ----------------------- |
| 1      | shows all the access lists created                                 | R# show access-lists    |
| 2      | Shows the inbound and outgoing access list applied to an interface | R# show interface [int] |