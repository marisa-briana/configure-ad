<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
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
<img src="https://user-images.githubusercontent.com/131695869/234705009-cafd6804-148c-49d0-8144-3d841b50f31b.png"/>
</p>
<p>
<h2>Setup Resources in Azure</h2>
Create the Domain Controller VM (Windows Server 2022) named “DC-1”
Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
Set Domain Controller’s NIC Private IP address to be static
Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher
</p>
<br />

<p>
<img src="https://user-images.githubusercontent.com/131695869/234705544-0d3cf226-2a4d-4a04-82f7-90b50ea2573e.png"/>
<img src="https://user-images.githubusercontent.com/131695869/234711616-af2006aa-d6aa-450c-bc99-6836c8c57df9.png"/>
</p>
<p>
<h2>Ensure Connectivity between the client and Domain Controller</h2>
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
Check back at Client-1 to see the ping succeed
</p>
<br />

<p>
<img src="https://i.imgur.com/7g0Byol.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h2>Install Active Directory</h2>
Login to DC-1 and install Active Directory Domain Services
Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
Restart and then log back into DC-1 as user: mydomain.com\labuser
</p>
<br />

<p>
<img src="https://user-images.githubusercontent.com/131695869/234707962-adc793bd-9957-4e77-b387-e66d02f5aaba.png"/>
<img src="https://user-images.githubusercontent.com/131695869/234709013-f6e3df4d-7ce3-40c1-95ca-9b3bd555fb1a.png"/>
</p>
<p>
<h2>Create an Admin and Normal User Account in AD</h2>
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Create a new OU named “_ADMINS”
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
Add jane_admin to the “Domain Admins” Security Group
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
User jane_admin as your admin account from now on
</p>
<br />

<p>
<img src="https://user-images.githubusercontent.com/131695869/234709384-90088cd0-8b52-4a44-9b93-7fb2dfdb59ef.png"/>
<img src="https://user-images.githubusercontent.com/131695869/234709760-c1a2068b-6c69-40a5-a3cd-54977b002bfc.png"/>
</p>
<p>
<h2>Join Client-1 to your domain (mydomain.com)</h2>
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
From the Azure Portal, restart Client-1
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
Create a new OU named “_CLIENTS” and drag Client-1 into there
</p>
<br />

<p>
<img src="https://user-images.githubusercontent.com/131695869/234710463-57adf255-35b1-401e-ad85-6bc7fa361b32.png"/>
</p>
<p>
<h2>Setup Remote Desktop for non-administrative users on Client-1</h2>
Log into Client-1 as mydomain.com\jane_admin and open system properties
Click “Remote Desktop”
Allow “domain users” access to remote desktop
You can now log into Client-1 as a normal, non-administrative user now
Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)
</p>
<br />

<p>
<img src="https://user-images.githubusercontent.com/131695869/234710778-554dced2-b718-4986-b1d4-acb2f818a7ea.png"/>
</p>
<p>
<h2>Create a bunch of additional users and attempt to log into client-1 with one of the users</h2>
Login to DC-1 as jane_admin
Open PowerShell_ise as an administrator
Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
Run the script and observe the accounts being created
When finished, open ADUC and observe the accounts in the appropriate OU
attempt to log into Client-1 with one of the accounts (take note of the password in the script)

Finish.
</p>
<br />

