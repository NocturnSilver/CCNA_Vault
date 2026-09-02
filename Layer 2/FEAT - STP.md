#layer2
#STP 

# Caution

## What is Spanning Tree Protocol (STP)
- a protocol, IEEE 802.1D, that prevents layer 2 loops by placing redundant ports in a blocking state, essentially disabling the interface

## Why use Spanning Tree Protocol
- it solves an effect that comes with network redundancy when you want to connect multiple switches together
	- broadcast storm
	- TTL is there for layer 3 to avoid this for packets
	- MAC Address Flapping
		- When frames with the same source MAC address repeatedly arrive on different interfaces, the switch is continuously updating the interface in its MAC address table
- 