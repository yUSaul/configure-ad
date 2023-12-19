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

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://github.com/yUSaul/configure-ad/assets/140694677/8c4a8534-209d-43ba-a826-0a385c03a637"/>
</p>
<p>
Create the Domain Controller VM (Windows Server 2022) named “DC-1”.  Take note of the Resource Group and Virtual Network (Vnet) that get created at this time.  Set Domain Controller’s NIC Private IP address to be static.  Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was previously created.  Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher).
</p>
<br />

<p>
<img src="https://github.com/yUSaul/configure-ad/assets/140694677/608fa528-3dc5-45fe-9f4f-98654869d5cc"
"/>
</p>
<p>
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping).  Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall.  Check back at Client-1 to see the ping succeed.
</p>
<br />

<p>
<img src="https://github.com/yUSaul/configure-ad/assets/140694677/a087b4ff-e24a-4b9c-9671-be1fb31ddb86"/>
</p>
<p>
Login to DC-1 and install Active Directory Domain Services.  Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is).  Restart and then log back into DC-1 as user: mydomain.com\labuser.
</p>
<br />

<p>
<img src="https://github.com/yUSaul/configure-ad/assets/140694677/4bf3dca2-2edb-4446-bb05-b7ed6f7c1bc9"/>
</p>
<p>
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”.  Create a new OU named “_ADMINS”.  Create a new employee named “Jane Doe” (same password) with the username of “a-janed”.  Add a-janed to the “Domain Admins” Security Group.  Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\a-janed”.  Use a-janed as your admin account from now on.
</p>
<br />

<p>
<img src="https://github.com/yUSaul/configure-ad/assets/140694677/fddcb648-9d1c-4cba-8830-986f517694e6"/>
</p>
<p>
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address.  From the Azure Portal, restart Client-1.  Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart).  Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.
</p>
<br />

<p>
<img src="https://github.com/yUSaul/configure-ad/assets/140694677/7cf07e99-3f1c-45d7-9a71-29ceec0cc60a"/>
</p>
<p>
Log into Client-1 as mydomain.com\jane_admin and open system properties.  Click “Remote Desktop”.  Allow “domain users” access to remote desktop.  You can now log into Client-1 as a normal, non-administrative user now.
</p>
<br />
