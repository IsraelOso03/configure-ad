<p align="center">
<img src="https://i.imgur.com/9x1zGwW.png" height="80%" width="80%" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
I set up a Domain Controller and Client VM in Azure, ensuring proper network configuration and static IP. I installed Active Directory, created Organizational Units, and established an admin account. I configured DNS on the Client VM, joined it to the domain, and enabled remote desktop access for domain users. Finally, I used PowerShell to automate user account creation, verified the accounts in ADUC, and tested logging in with the new accounts, showcasing my skills in domain management and user administration.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)
  
<h2>Deployment and Configuration Steps</h2>
<br>

<p>
To set up resources in Azure, begin by creating a Domain Controller virtual machine (VM) using Windows Server 2022, and name it “DC-1”. During this process, take note of the Resource Group and Virtual Network (VNet) that are created. Next, set the Domain Controller’s Network Interface Card (NIC) private IP address to static. Afterward, create a client VM using Windows 10, named “Client-1”, within the same Resource Group and VNet as "DC-1". Ensure both VMs are on the same VNet, which can be verified by using the Network Watcher topology feature.
</p>
<p>
<img width="310" alt="AD_Lab_VM_creation" src="https://github.com/user-attachments/assets/c25c0d17-ed14-4b4a-acb2-7eeb34642d91">
<img width="310" alt="AD_Lab_Client_Creation" src="https://github.com/user-attachments/assets/1907a1fc-0ebe-4a51-909e-3e2a40dd80f4">
<img width="310" alt="DC_DynamicToStatic" src="https://github.com/user-attachments/assets/1e01e3ee-3af9-4ee0-bb1a-cfd257000794">
</p>
<br>

<p>
To ensure connectivity between the client and Domain Controller, start by logging into "Client-1" using Remote Desktop and perform a continuous ping to "DC-1's" private IP address using the command `ping -t <ip address>'. Next, log into the Domain Controller and enable ICMPv4 inbound traffic on the local Windows Firewall. After that, return to "Client-1" to verify that the ping is successful, confirming connectivity between the two machines.
</p>
<p>
<img width="400" src="https://github.com/user-attachments/assets/f21637ae-1a69-46b5-9c47-5ec0c8d83f9b">
<img width="400" src="https://github.com/user-attachments/assets/befc01f9-8eb4-43c5-a195-ebd89d7da65e">
</p>
<br>
  
<p>
To install Active Directory, log in to "DC-1" and install the "Active Directory Domain Services" feature. Once installed, promote the server as a Domain Controller by setting up a new forest with the domain name <b>mydomain.com</b>. After the process is complete, restart the server, and log back into "DC-1" using the credentials <b>mydomain.com\labuser</b>.
</p>
<p>
<img width="310" src="https://github.com/user-attachments/assets/d689b3ed-ca09-4762-87e1-912a1d83fb45">
<img width="310" src="https://github.com/user-attachments/assets/4c1bc644-7911-4603-b010-6ec245661b4a">
<img width="310" alt="image" src="https://github.com/user-attachments/assets/96a429c0-34ab-4bae-b61c-1d93c88890da">
</p>
<br>

<p>
In "Active Directory Users and Computers (ADUC)", begin by creating an "Organizational Unit (OU)" named <b>_EMPLOYEES</b>. Afterward, create another new OU called <b>_ADMINS</b>. These OUs will help in organizing and managing users and groups within your domain effectively.
</p>
<p>
<img width="400" alt="image" src="https://github.com/user-attachments/assets/f9b65807-fb5f-4db8-9045-faed00b2df62">
<img width="400" alt="image" src="https://github.com/user-attachments/assets/78c7625b-0f7a-4ef1-a8c9-45cc21c0436c">
</p>
<br>

<p>
In Active Directory Users and Computers (ADUC), create a new employee with the name First Last Name, using the same password for both fields, and assign the username firstname_admin. Next, add firstname_admin to the Domain Admins security group. Once this is done, log out and close the Remote Desktop connection to "DC-1". Then, log back into "DC-1" as mydomain.com\firstname_admin. From this point forward, use firstname_admin as your admin account.
</p>
<p>
<img width="310" alt="image" src="https://github.com/user-attachments/assets/ce6e2cc3-e093-46cf-acfc-83b846d983ca">
<img width="310" alt="image" src="https://github.com/user-attachments/assets/a63c989a-460a-40b3-8b9e-c0cbe8cbe386">
<img width="310" alt="image" src="https://github.com/user-attachments/assets/4f64ac96-0ec7-4641-a9f4-3f2a30b227c1">
</p>
<br>

<p>
From the Azure Portal, update Client-1's DNS settings by configuring them to use the DC's Private IP address. Afterward, restart Client-1 via the Azure Portal. Once restarted, log into Client-1 using Remote Desktop with the original local admin account (labuser). Proceed to join Client-1 to the domain, which will trigger another restart of the computer. Finally, log back into the Domain Controller (via Remote Desktop) and verify that Client-1 appears in Active Directory Users and Computers (ADUC) under the Computers container at the root of the domain.
</p>
<p>
<img width="400" alt="image" src="https://github.com/user-attachments/assets/b8f9259d-b721-4211-8392-6ad87a9e593a">
<img width="400" alt="image" src="https://github.com/user-attachments/assets/2136d9b7-a3aa-4fc7-9f0d-7c74478e80a8">
</p>
<br>

<p>
Log into Client-1 as mydomain.com\firstname_admin and open the System Properties. Navigate to the Remote Desktop tab and enable remote desktop access. Then, add the Domain Users group to allow domain users remote access to the system. Once this is configured, you will be able to log into Client-1 as a normal, non-administrative user from now on.
</p>
<p>
<img width="400" alt="image" src="https://github.com/user-attachments/assets/b8a26e5f-437c-4d92-867e-f74d7576e101">
</p>
<br>

<p>
Log into DC-1 as firstname_admin and open PowerShell ISE with administrative privileges. Create a new file within PowerShell ISE and paste the contents of the script into it. Run the script and monitor the process as the accounts are being created. Once the script has completed, open Active Directory Users and Computers (ADUC) and verify that the new accounts have been created and placed in the appropriate Organizational Units (OUs) as defined by the script.
</p>
<p>
<img width="400" alt="image" src="https://github.com/user-attachments/assets/119ab4c3-c3cb-4059-a497-b5cb47b60222">
<img width="400" alt="image" src="https://github.com/user-attachments/assets/7d7f96aa-81c0-4560-adc5-12a2340dc9bc">
</p>
<br>

<p>
After the accounts have been created, attempt to log into Client-1 using one of the newly created accounts. Use the domain format mydomain.com\newaccountusername and the corresponding password. This will confirm that the account was successfully created and is able to authenticate within the domain.
</p>
<p>
<img width="310" alt="image" src="https://github.com/user-attachments/assets/7e564f5a-5677-467d-8d04-e22aabe822d2">
</p>
