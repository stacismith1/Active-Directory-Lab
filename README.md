# Active-Directory-Lab

## Purpose
This lab was performed to practice and review the key steps to deploy an Active Directory Environment with users and client machines, as well as hardening guidelines for a typical Installation. Every environment will have different factors to account for which will alter how the network is configured and run, so this lab illustrates the key considerations of a typical system configuration.

## Procedure
- This lab was configured and deployed in Oracle VM VirtualBox Manager on a 12th Gen Intel® Core™ i5-1240P Framework laptop with 16GB of memory and 16 total threads. (Plenty for this excersise)
- Beginning with a Windows Server 2019 VM build, install and configure the Domain Controller Role and DNS Server role. (For larger environments it is key to replicate these roles across multiple servers in your AD domain for fail-over management, 1 will suffice.) 
-> It is also possible to handle routing, NAT and DHCP on the same server as the domain controller, however this is not recommended to avoid Single Points of failure and increasing the attack surface for our critical network services.    

![Diagram of Network layout for lab](https://github.com/stacismith1/Active-Directory-Lab/blob/main/AD%20LAB%20diagram.png)

> Diagram Includes 1 Primary DC on Server2019, 2x WIN10PC clients, 1K auto-generated domain users, a Pfsense Firewall managing NAT, Routing & DHCP. 

- After Installing and configuring the DC, the router is installed and configured to enable the devices on the network to communicate. Suricata for IPS and VLAN segmentation was also added to the Pfsense machine for testing, however for the scope of this basic lab, this is not really necessary.

![Screenshot of Pfsense Router GUI as configured for lab](https://github.com/stacismith1/Active-Directory-Lab/blob/main/VirtualBox_Router.png)
- Next a pair of Windows 10 VMs are installed on the network and joined to the domain.

- To efficiently add users to the the domain its possible to leverage the power of custom scripts to do so in batch. PowerShell Script is used for just that purpose as well as to randomly generate names to be used for this process. The code used generates 1000 users and enters each into the domain, far faster than could be done manually.

## Hardening
** These steps are important for locking down your AD environment but there are hundreds of options to consider when hardening and configuring a network that will always be specific for each individual network. These are a handful of important considerations that are often selected in most deployments.

- Create as many Security groups in AD Group Policy Editor as required by the organization and build and apply rules for each based on Organziations security policy, Required permissions and activities for each user group, etc. 
- Activate the signature on SMB protocol exchanges
- Set Password & Authentication policies at the domain level
- Set backup, patching, powermanagement and File encryption rules
- Set File permissions for Network attached Storage.

> It is possible to also harden the domain and server using scripting such as PowerShell to customize and expedite the process:

- Determine if server is running SMBv1 and disable:
   `Get-SmbServerConfiguration | Select EnableSMB1Protocol`
