## Duplex mismatches
- can cause the following problems
	- intermittent connectivity
	- performance problems
	- high number of collisions
	- late collisions
- error occurs when the ends of a network link are configured with different duplex settings
	- both ends of the link should be configured to use the same duplex settings
- Symptoms
	- the half duplex side of the connection will report late collisions, while the full duplex side of the connection will report runts, frame check sequence (FCS) errors, and alignment errors
- SHOW INTERFACE STATUS when symptoms occur

## Speed mismatches
- can prevent an interface from sending or receiving traffic.
- error occurs when one end of a network link is configured to use a different speed than the other end of the link
- A link between the two interfaces would not be able to be established and the links would remain in the down state
- USE SHOW INTERFACE STATUS when symptoms occur

## Broadcast Storm
- result of an extreme amount of broadcast traffic on a network, thereby causing congestion
- broadcast traffic is traffic, such as ICMP, that is sent to all hosts on a network
- A broadcast storm occurs when a device repeatedly sends out broadcast packets, resulting in continual broadcast responses from the other hosts on the network.
- This continuous flow of broadcast traffic can be the result of connecting multiple switches together in such a way as to create a loop or the result of connecting a patch cable to two different ports on the same switch
- Symptoms
	- sluggish or unresponsive network
	- unusually high CPU utilization by connected hosts as well as any affected switches. High CPU usage results from the devices processing the large volume of packets flowing around the network during the storm.
- SHOW INTERFACES
	- might show a high collision count
- SHOW PROCESSES
	- to view the CPU usage

## Late Collisions
- a collision that occurs after the 512th bit (64 byte) of a frame has been transmitted by a device.
- a switch detects such a collision if it begins sending a frame and a collision occurs after 512 bits of the outgoing frame have been sent.
- The amount of time it takes to send the first 512 bits of a frame is dependent on the network technology in use.
- similar to other collision errors, late collisions can occur as a result of duplex mismatch errors or a network segment that extends farther than the cable length supports
- Solutions
	- check duplex settings and network segments aren't too long
	- issue show interfaces [interface] to troubleshoot late collision

## Switching Loops
- There can only be one active path at any given time between any two endpoints on an Ethernet network
- if multiple paths between the same two endpoints exist at the same time, switching loops can occur
- SHOW SPANNING-TREE
	- examine STP configs on a SW