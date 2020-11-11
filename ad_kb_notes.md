[toc]
# What is AD?
* A collection of machines and servers connected inside of domains. 
* A collective part of a bigger forest of domains
* Make up the AD network
* Pats of the AD Network
	* Domain Controllers (DCs)
	* Forests, Trees, Domains
	* Users + Groups (objects)
	* Trusts
	* Policies
	* Domain Services
# Why use AD?
* Allows for the control and monitoring of user's computers through a single DC
* Allows a single user to sign in to any computer on the AD network and have access to his/her stored files and folders in the server and the local machine storage
# AD DS Data Store
* Holds the databases and processes needed to store and manage directory information such as users, groups and services
	* Contains the NTDS.dit
		* Database that contains all of the information of an AD DC as well as password hashes for domain users
			* Stored by default in %SystemRoot%\NTDS
			* Accessible only by the DC
# The Forest
* Defines everything
	* Container that holds all of the other bits and pieces of the network together
	* Without the forest, trees and domains would not be able to interact
	* Just as much a physical thing as it is a figurative thing. 
	* Basically, just a way to describe the connection created between trees and domains by the network
## Heirarchy
* Forest: See above
* Trees: Heirarchy of domains in the AD DS
* Domains: Used to group and manage objects
* Organizational Units: Containers for groups, computers, users, printers, and other OUs
* Trusts: Allows users to access resources in other domains
	* Directional
	* Transitive
* Objects: Users, groups, printers, computers, shares
* Domain Services: DNS Server, LLMNR, IPv6
* Domain Schema: Rules for object creation
# Users and Groups
## What are users and groups?
* The users and groups that are inside of an Active Directory are up to you; when you create a domain controller it comes with default groups and two default users: Administrator and guest. It is up to you to create new users and create new groups to add users to.

## Users Overview
Users are the core to Active Directory; without users why have Active Directory in the first place? There are four main types of users you'll find in an Active Directory network; however, there can be more depending on how a company manages the permissions of its users. The four types of users are: 

 * Domain Admins - This is the big boss: they control the domains and are the only ones with access to the domain controller.
 * Service Accounts (Can be Domain Admins) - These are for the most part never used except for service maintenance, they are required by Windows for services such as SQL to pair a service with a service account
 * Local Administrators - These users can make changes to local machines as an administrator and may even be able to control other normal users, but they cannot access the domain controller
 * Domain Users - These are your everyday users. They can log in on the machines they have the authorization to access and may have local administrator rights to machines depending on the organization.
# Groups Overview
* Groups make it easier to give permissions to users and objects by organizing them into groups with specified permissions. There are two overarching types of Active Directory groups:
	* Security Groups - These groups are used to specify permissions for a large number of users
	* Distribution Groups - These groups are used to specify email distribution lists. As an attacker these groups are less beneficial to us but can still be beneficial in enumeration

### Default Security Groups - 

* There are a lot of default security groups so I won't be going into too much detail of each past a brief description of the permissions that they offer to the assigned group. Here is a brief outline of the security groups:
	* Domain Controllers - All domain controllers in the domain
	* Domain Guests - All domain guests
	* Domain Users - All domain users
	* Domain Computers - All workstations and servers joined to the domain
	* Domain Admins - Designated administrators of the domain
	* Enterprise Admins - Designated administrators of the enterprise
	* Schema Admins - Designated administrators of the schema
	* DNS Admins - DNS Administrators Group
	* DNS Update Proxy - DNS clients who are permitted to perform dynamic updates on behalf of some other clients (such as DHCP servers).
	* Allowed RODC Password Replication Group - Members in this group can have their passwords replicated to all read-only domain controllers in the domain
	* Group Policy Creator Owners - Members in this group can modify group policy for the domain
	* Denied RODC Password Replication Group - Members in this group cannot have their passwords replicated to any read-only domain controllers in the domain
	* Protected Users - Members of this group are afforded additional protections against authentication security threats. See http://go.microsoft.com/fwlink/?LinkId=298939 for more information.
	* Cert Publishers - Members of this group are permitted to publish certificates to the directory
	* Read-Only Domain Controllers - Members of this group are Read-Only Domain Controllers in the domain
	* Enterprise Read-Only Domain Controllers - Members of this group are Read-Only Domain Controllers in the enterprise
	* Key Admins - Members of this group can perform administrative actions on key objects within the domain.
	* Enterprise Key Admins - Members of this group can perform administrative actions on key objects within the forest.
	* Cloneable Domain Controllers - Members of this group that are domain controllers may be cloned.
	* RAS and IAS Servers - Servers in this group can access remote access properties of users
# Trusts and Policies
## What are trusts?
* Trusts and policies go hand in hand to help the domain and trees communicate with each other and maintain "security" inside of the network. They put the rules in place of how the domains inside of a forest can interact with each other, how an external forest can interact with the forest, and the overall domain rules or policies that a domain must follow.

## Domain Trusts Overview

* Trusts are a mechanism in place for users in the network to gain access to other resources in the domain. For the most part, trusts outline the way that the domains inside of a forest communicate to each other, in some environments trusts can be extended out to external domains and even forests in some cases.
* There are two types of trusts that determine how the domains communicate. The two types of trusts below:
	* Directional - The direction of the trust flows from a trusting domain to a trusted domain
	* Transitive - The trust relationship expands beyond just two domains to include other trusted domains
* The type of trusts put in place determines how the domains and trees in a forest are able to communicate and send data to and from each other when attacking an Active Directory environment you can sometimes abuse these trusts in order to move laterally throughout the network. 
## Domain Policies Overview 

* Policies are a very big part of Active Directory, they dictate how the server operates and what rules it will and will not follow. You can think of domain policies like domain groups, except instead of permissions they contain rules, and instead of only applying to a group of users, the policies apply to a domain as a whole. They simply act as a rulebook for Active  Directory that a domain admin can modify and alter as they deem necessary to keep the network running smoothly and securely. Along with the very long list of default domain policies, domain admins can choose to add in their own policies not already on the domain controller, for example: if you wanted to disable windows defender across all machines on the domain you could create a new group policy object to disable Windows Defender. The options for domain policies are almost endless and are a big factor for attackers when enumerating an Active Directory network. I'll outline just a few of the  many policies that are default or you can create in an Active Directory environment: 
	* Disable Windows Defender - Disables windows defender across all machine on the domain
	* Digitally Sign Communication (Always) - Can disable or enable SMB signing on the domain controller

# AD DS and Authentication
## Domain Services Overview
* Domain Services are exactly what they sound like. They are services that the domain controller provides to the rest of the domain or tree. There is a wide range of various services that can be added to a domain controller; however, in this room we'll only be going over the default services that come when you set up a Windows server as a domain controller. Outlined below are the default domain services: 
	* LDAP - Lightweight Directory Access Protocol; provides communication between applications and directory services
	* Certificate Services - allows the domain controller to create, validate, and revoke public key certificates
	* DNS, LLMNR, NBT-NS - Domain Name Services for identifying IP hostnames
## Domain Authentication Overview
* The most important part of Active Directory -- as well as the most vulnerable part of Active Directory -- is the authentication protocols set in place. There are two main types of authentication in place for Active Directory: NTLM and Kerberos.
	* Kerberos - The default authentication service for Active Directory uses ticket-granting tickets and service tickets to authenticate users and give users access to other resources across the domain.
	* NTLM - default Windows authentication protocol uses an encrypted challenge/response protocol
* The Active Directory domain services are the main access point for attackers and contain some of the most vulnerable protocols for Active Directory, this will not be the last time you see them mentioned in terms of Active Directory security.
# AD in the Cloud
## Azure AD Overview
* Azure acts as the middle man between your physical Active Directory and your users' sign on. This allows for a more secure transaction between domains, making a lot of Active Directory attacks ineffective.
## On-prem vs Cloud Comparison
|Windows Server AD|Azure AD|
|:---:|:----:|
|LDAP|Rest APIs|
|NTLM|OAuth/SAML|
|Kerberos|OpenID|
|OU Tree|Flat Structure|
|Domains and Forests|Tenants|
|Trusts|Guests|
* Still need to do more reading on this, but these are the quick basics to get by
