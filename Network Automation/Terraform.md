## Infrastructure as Code (IAC)
- is the practice of provisioning and managing infrastructure (servers, networks, cloud resources) using machine-readable configurations files (code) instead of manual configuratoin (e.g., CLI/GUI)
- Ansible, Puppet, and Chef are examples of IaC configuration management tools.
	- Ansible manages device configurations using several files:![[Pasted image 20260817175736.png]]
- Terraform is an IaC-based provisioning tool that automates the creation of infrastructure resources
- IaC automates infrastructure deployment and management, ensuring consistency, scalability, and repeatability.

## Provisioning and Management
- Configuration management (e.g., Ansible, Puppet, Chef)
	- Manages existing infrastructure by installing software, configuring settings, and maintaining system state.
	- Ensures consistency by applying and enforcing configurations across multiple devices.
- Infrastructure provisioning (e.g. Terraform)
	- Creates, modifies, and deletes infrastructure resources such as servers and network infrastructure.
	- Focuses on initial setup rather than ongoing configuration management.
- Configuration management tools work on already existing systems, whereas provisioning tools build infrastructure from scratch.
- Terraform (provisioning) and Ansible (configuration management) can work together:
	- Terraform provisions infrastructure (VMs, network, storage, etc.), and Ansible provides ongoing configuration and management
	- ![[Pasted image 20260817180354.png]]

## Mutable/Immutable infrastructure
- Configuration management tools typically use a mutable infrastructure approach
	- infrastructure can be modified after deployment (e.g., applying updates, patches, or configuration changes)
	- Changes are made in place, meaning existing resources are updated rather than replaced.![[Pasted image 20260817181711.png]]
- Provisioning tools employ an immutable infrastructure approach
	- Infrastructure cannot be changed after deployment.
		- "Changes" involve replacing the previous resource with a new version.
	- No configuration drift, since each deployment starts from a fresh, predefined state![[Pasted image 20260817181721.png]]

## Procedural vs Declarative
- Procedural approach (a.k.a imperative)
	  follow explicit steps in a specific order to achieve the desired outcome.
	  The user must define each action to configure the infrastructure
	  Provides greater control compared to a declarative approach
- Declarative approach
	- Defines the desired end state
	- the tool (e.g. Terraform) figures out the steps needed to achieve the goal.
	- Easier to maintain and ensures the consistency across deployments
- A procedural Approach focuses on how to make changes (explicit steps).
	- "configure router's hostname with hostname R1"
	- "Configure the IP address of G0/1 with ip address 192.168.1.1 255.255.255.0"
	- "Enables the interface with no shutdown"
	- etc.
- A declarative approach focuses on what the final state should be 
	- "Create a router named R1 with IP address 192.168.1.1/24 on its G0/1 interface, ensuring the interface is enabled"

| Procedural | Declarative |     |
| ---------- | ----------- | --- |
| Ansible    | Terraform   |     |
| Chef       | Puppet      |     |

## Terraform Basics
- is an open-source IaC tool developed by HashiCorp (acquired by IBM in 2025).
- It is primarily a provisioning tool, focused on deploying infrastructure resources on various cloud and on-prem platforms.
	- These platforms are called providers and include AWS, Azure, GCP, Kubernetes, and many more (1000+).
		- This includes integrations with Cisco platforms liek Catalyst Center, ACI, and IOS XE.
- Like Ansible, Terraform uses a push model and is agentless; it doesn't require a software agent on a infrastructure it provisions or manages.
![[Pasted image 20260817182755.png]]
- The four key components that work together to provision and manage infrastructure:
	- Terraform Core - the core software developed by Hashicorp
	- Configuration Files - files which an engineer creates to define the end state file, which tracks the current state of the deployed infrastructure 
	- State File - tracks the current state of the deployed infrastructure
	- Providers - the platforms on which the infrastructure is deployed. 
- The basic terraform workflow consists of three main steps:
	- Write: Define the desired state of your infrastructure resources in configuration files.
	- Plane: Verify the changes that will be executed before applying them
	- Apply: Execute the plan to provision and manage thee infrastructure resources.
- While Terraform Core is written in Go, configuration files are written in HashiCorp Configuration Language (HCL).
	- HCL is an example of a domain-specific language (DSL), a type of language specialized for a particular purpose
		- Because DSLs are specialized, they allow users to perform complex tasks with much less effort than a general language (like Python or Go) would require.

## Summary
- IaC is the practice of provisioning and managing infrastructure (servers, networks, cloud resources) using machine-readable configuration files (code) instead of manual configuration (e.g., CLI/GUI)
- Configuration management tools (e.g., Ansible, Puppet, Chef) focus on managing existing infrastructure by installing software, configuring settings (e.g., router configurations), and maintaining system state.
- Infrastructure provisioning tools (e.g. Terraform) focus on creating, modifying, and deleting infrastructure resources.
- Mutable Infrastructure - can be modified after deployment (e.g. , applying updates, patches, or configuration changes).
- Immutable Infrastructure  - cannot be changed after deployment; changes involve replacing the previous resource with a new version.
- Procedural (imperative) - follows explicit steps in a specific order to achieve the desired outcome.
- Declarative - defined the desired end state, and the IaC tool figures out the steps needed to achieve the goal.
- Terraform is an open-source IaC tool developed by HashiCorp
	- It is primarily a provisioning tool used for deploying infrastructure on providers like AWS, Azure, GCP, Kubernetes, etc.
		- It interacts with these providers via their APIs
	- Like Ansible, it uses a push model and is agentless
	- Main components: Terraform core, configuration files, state file, providers.
	- The basic terraform workflow consists of three main steps:
		- Write: Define the desired state of your infrastructure resource in configuration files.
		- Plan: Verify the changes that will be executed before applying them.
		- Apply: Execute the plan to provision and manage the infrastructure resources.
	- Terraform core is written in Go, and configuration files are written in HashiCorp Configuration Language (HCL), a DSL