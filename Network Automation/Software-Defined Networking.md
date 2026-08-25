#automation
## SDN Review
- Software-Defined Networking (SDN) is an approach to networking that centralizes the control plane into an application called a controller.
- Traditional control planes use a distributed architecture
- An SDN controller centralizes control plane functions like calculating routes.
- the controller can interact programmatically with the network devices using APIs
- The SBI is used for communications between the controller and the network device it controls.
- The NBI is what allows us to interact with the controller with our scripts and applications.![[Pasted image 20260817152438.png]]

### SDN architecture
- Applications Layer
	- contains scripts/applications that tell the SDN controller what network behaviours are desired.
- Control Layer
	- Contains the SDN controller that receives and processes instructions from the applications layer.
- Infrastructure Layer
	- Contains the network devices that are responsible for forwarding messages across then network.

 
## Cisco SD-Access (Cisco-Software Defined Access)
- Cisco SD-Access is Cisco's SDN solution for automating campus LANs
	- ACI (application Centric Infrasturcture) is their SDN solution for automating data center networks.
	- SD-WAN is their SDN solution for automating WANs.
- Cisco DNA (Digital Network Architecture) Center is the controller at the center of SD-Access.
- ![[Pasted image 20260817150802.png|334]]
- Underlay - is the underlying physical network of devices and connections (including) wired and wireless which provide IP connectivity (ie using IS-IS).
	- Multilayer switches and their connections
- Overlay - is the virtual network built on top of the physical underlay network.
	- SD-Access uses VXLAN (Virtual Extensible LAN) to build tunnels.
- The fabric is the combination of the overlay and underlay; the physical and virtual network as whole![[Pasted image 20260817151303.png]]

### SD-Access Underlay
- the underlay's purpose is to support the VXLAN tunnels of the overlay.
- There are three different roles for switches in the SD-Access:
	- Edge nodes: Connect to end hosts.
	- Border nodes: Connect to devices outside of the SD-Access domain, ie. WAN routers.
	- Control nodes: Use LISP (Locator ID Separation Protocol) to perform various control plane functions.
- You can add SD-Access on top of an existing network (brownfield deployment) if your network hardware and software supports it.
	- Google 'Cisco SD-Access compatiblity matrix' if you're curious
	- In this case DNA Center won't configure the underlay
- A new deployment (greenfield deployment) will be configured by DNA Center to use the optimal SD-Access underlay:
	- All switches are layer 3 and use Is-Is as their routing protocol.
	- All links between switches are routed ports. This means STP is not needed.
	- Edge nodes (access switches) at as a the default gateway of end hosts (routed access layer).![[Pasted image 20260817154250.png|493]]

### SD-Access Overlay
- LISP provides the control plane of SD-Access.
	- a list of mappings of EIDs (endpoint identifiers) to RLOCs (routing locators) is kept.
	- EIDs identify end hosts connetected to edge switches, and RLOCs identify the dge dwitch which can be used to reach the end host.
	- There is a LOT more detail to cover about LISP, it differs from the traidtional control plane/
- Cisco TrustSec (CTS) provides policy control (QoS, security policy, etc.)
- VXLAN provides the data plane of SD-Access

## Cisco DNA Center
- Cisco DNA center has two main roles: 
	- The SDN controller in SD-Access
	- A network managers in a traidtional network (non-SD-Access)
- DNA center is an application installed on Cisco USC server hardware.
- It has a REST API which can be used to interact with DNA center.
- The SBI supports protocols such as NETCONF and RESTCONF (as well as traditional protocols like Telnet, SSH, SNMP)
- DNA Center enables Intent-Based Networking (IBN).
	- the goal is to allow the engineer to communicate their intent for network behavior to DNA Center, adn then DNA Center will take care of the details of the actual configurations and policies on devices.
- Traditional security policies using ACLs can become VERY cumbersome.
	- ACLs can have thousands of entries
	- the intent of entries is forgotten with time and as engineers leave and new engineers take over
	- Configuring and applying the ACLs correctly across a network is cumbersome and leaves room for error.
- DNA center allows the engineer to specify the intent of the policy (this groups of users can't communicate with this group, this groups can access this server but not that server, etc.) and DNA Center will take care of the exact details of implementing the policy
- Policies are easily implemented and documented![[Pasted image 20260817155848.png]]

### Sandbox to Check out
- sandboxdnac.cisco.com
- User: devnetuser
- Password: Cisco123!
## DNA Center Network Management vs Traditional 
|                              | DNA Center Management                                                                                                                                       | Traditional Network Management                                                |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Device Configuration         | Devices are centrally managed and monitored from the DNA Center GUI or other applicatoins using its REST API                                                | Devices are configured one-by-one via SSH or console connection               |
| Method of Configuration      | The admin communicates their intended network behavior to the DNA Center, which changes those intentions into configurations on the managed network devices | Devices are manually configured via console connection before being deployed  |
| Device Management            | Configurations and policies are centrally managed                                                                                                           | Configurations and policies are managed per-device (distributed)              |
| New Network deployments      | New network deployments are much quicker. New devices can automatically receive their configurations from DNA center without manual coniguration            | New network deployments can take a long time due to the manual labor required |
| Error and failure likelihoog | Software versions are also centrally managed. DNA center can monitor cloud servers for new versions and then update the managed devices                     | Errors and failures are more likely due to increased manual effort            |
