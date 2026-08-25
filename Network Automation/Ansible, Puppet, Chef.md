## Intro to Configuration Management Tools
- Why are configuration management tools useful?
	- Configuration Drift - individual changes made over time cause a device's configuration to deviate from the standard/correct configurations as defined by the company.
		- Although each device will have unique parts of its configuration (IP addresses, host name, etc.), most of a device's configuration is usually defined in standard templates designed by the network architects/engineers of the company
		- As individual engineers make changes to devices (for example to troubleshoot and fix network issues, test configurations, etc.), the configuration of a device can drift away from the standard.
			- Records of these individual changes and their reasons aren't kept.
			- This can lead to future issues
		- Even without automation tools, it is best to have standard configuration management practices.
			- When a change is made, save the config as a text file and place it in a shared folder.
			- A standard naming system like hostname__yymmdd might be used.
			- There are flaws to this system, as an engineer might forget to place new config in the folder after making changes. Which one should be considered the 'correct' config?
			- Even if configurations are properly saved like this, it doesn't guarantee that the configurations actually match the standard.

### Configuration Provisioning
- refers to how configuration changes are applies to devices.
	- This includes configuring new devices, too
- Traditionally, configuration provisioning is done by connecting to devices one-by-one via SSH.
	- This is not practical in large networks.
- Configuration management tools like Ansible, Puppet, and Chef allows us to make changes to devices on a mass scale with a fraction of the time/effort
- Two essential components: templates and variables![[Pasted image 20260817163636.png]]

### Configuration Management Tools
- are network automation tools that facilitate the centralized control of large numbers of network devices
- The options you need to be aware of for the CCNA are Ansible, Puppet, and Chef.
- These tools were originally developed after the rise of VMs, to enable server system admins to automate the process of creating, configuring, and removing VMs.
	- However, they are also widely used to manage network devices
- These tools can be used for perform tasks such as:
	- Generate configurations for new devices on a large scale.
	- Perform configuration changes on devices (all devices in your network or a certain subset of devices).
	- Check device configurations for compliance with defined standards.
	- Compare configurations between devices, and between different versions of configurations on the same devices

## Ansible
- Is a configuration management tool owned by Red Hat.
- Ansible itself is written in Python
- Ansible is agentless
	- it doesn't require any special software to run on the managed devices
- Ansible uses SSH to connect to devices, make configuration changes, extract information, etc.
- uses a push model. The ansible server (control node) uses SSH to connect to managed devices and push configuratoin changes to them
	- Puppet and Chef uses a pull model
- After installing Ansible itself, you must create several text files:
	- Playbooks: These files are blueprints of automation tasks. They outline the logic and actions of the tasks that Ansible should do. Written in YAML
	- Inventory: These files list the devices that will be managed by Ansible, as well as characteristics of each device such as their device role (access switch, core switch, WAN router, firewall, etc.). Written in INI, YAML, or other formats
	- Templates: These files represent a device's configuration file, but specific values for variables are not provided. Written in Jinja2 format
	- Variabes: these files list variables and their values. These values are substituted into the templates to create complete configuration files. Written in YAML.
![[Pasted image 20260817171046.png]]

## Puppet
- Puppet is a configuration management tool written Ruby.
- Puppet is typically agent-based
	- specific software must be installed on the managed devices
	- Not all Cisco devices support a Puppet agent.
- it can be run agentless, in which a proxy agent runs on an external host, and the proxy agent uses SSH to connect to the managed devices and communicate with them.
- The Puppet server is called the 'Puppet master'.
- Puppet uses a pull model (clients 'pull' configurations from the Puppet master).
	- Clients use TCP 8140 to communicate with the Puppet master
- Instead of YAML, it uses a proprietary language for files.
- Text files required on the Puppet master include:
	- Manifest: This file defines the desired configuration state of a network device
	- Templates: similar to Ansible templates. Used to generate Manifests.
	-  ![[Pasted image 20260817173557.png]]

## Chef
- Chef is a configuration management tool written in Ruby
- Chef is agent-based
	- specific software must be installed on the managed devices.
	- Not all Cisco devices support a Chef agent.
- Chef uses a pull model
- The server uses TCP 10002 to send configurations to clients
- Files use a >DSL (domain-specific Language) based on Ruby
- Text files used by Chef incude:
	- Resources: The 'ingredients' in a recipe. Configuration objects managed by Chef
	- Recipes: The 'recipes' in a cookbook. Outline the logic and actions of the tasks performed on the resources.
	- Cookbooks: A set of related recipes grouped together.
	- Run-list: An ordered list of recipes that are run to bring a device to the desired configuration state. ![[Pasted image 20260817174128.png]]

## Comparison Chart 
![[Pasted image 20260817174229.png]]