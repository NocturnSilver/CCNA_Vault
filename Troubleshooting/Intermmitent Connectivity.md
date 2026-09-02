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

## Speed mismatches
- can prevent an interface from sending or receiving traffic.
- error occurs when one end of a network link is configured to use a different speed than the other end of the link
- A link between the two interfaces would not be able to be established and the links would remain in the down state