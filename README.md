<p align="center">
<img src="https://i.imgur.com/9x1zGwW.png" height="80%" width="80%" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/if9dTzg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To begin, first thing is to create your resources in azure. Creating a virtual machine named "DC-1" will automatically create a resource group and virtual network. Next, we will set domain controllers NIC Private IP address to be static.A second virtual machine called "client-1" will need to be made with the same reource group and virtual network making sure the vnet is the same.
</p>
<br />

<p>
<img src="https://i.imgur.com/9E28t92.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, we must ensure connectivity between the client and the domain controller.To test, we can first login into the domain controller and enable ICMPv4 on the local windows firewall.Go into command prompt and type in ping -t <ip address> and it should perpetually ping on the command prompt.
</p>
<br />

<p>
<img src="https://i.imgur.com/WnwmxVi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Installing active directory is the next step. We login to DC-1 and install, setting up a new forest called www.mydomain.com then restart and log back in as mydomain.com\labuser
</p>
<br />

<p>
<img src="https://i.imgur.com/wrESJtY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We must then create admin and nomral user account in active directory. Create an organizational unit for both, one called _ADMINS & _EMPLOYEES.Create an employee, i used the name jane doe as an example (can be anything) with the username jane_admin. jane_admin will then be added to the "domain admins" security group, we will continue now as jane_admin from now on.
</p>
<br />

<p>
<img src="https://i.imgur.com/HB6HFjh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
From azure, the client-1's dns settings must be set to the DC's private address and then restart client-1.Next is to login into client-1 as the local admin and then join it into the domain.When logging into domain controller and client-1 should pop up in the active directory,once that is verified, a new OU must be created for client-1 named "_CLIENTS".
</p>
<br />

<p>
<img src="https://i.imgur.com/YOQOCqm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To be able to login as a regular user in client-1,Log into Client-1 as mydomain.com\jane_admin and open system properties.Open remote desktop and allow domain users to access remote desktop.
</p>
<br />

<p>
<img src="https://i.imgur.com/KYn4Z8l.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now to create more users in active directory,login as jane_admin and open powershell as an admin.We will create a new file with a new script.We get the script running and see all the accounts be created that u choose to create.
