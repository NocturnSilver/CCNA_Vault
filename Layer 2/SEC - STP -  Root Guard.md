#layer2
#layer2sec 
#STP
#STPtoolkit

## Context
- BKN (Root Inconsistent) - show in the output of show spanning-tree. It means broken or disabled by root guard
- Root_inc - shows in output of show spanning-tree. It means root inconsistent
## What is Root Guard?
- a security measure where even if an interface receives a superior BPDU (lower bridge ID) on that interface, the switch will not accept the new switch as the root bridge. The interface will be disabled

## Root Guard Mechanics
- a port disabled by root guard will be re-enabled when it stops receiving superior BPDUs
- if a Root guard-enabled port receives a superior BPDU, it will enter the broken (root inconsistent) state

## Commands

| Number | Reason                      | Command                                 |
| ------ | --------------------------- | --------------------------------------- |
| 1      | Enable Root Guard on a port | SW(config-if)# spanning-tree guard root |
| 2      |                             |                                         |