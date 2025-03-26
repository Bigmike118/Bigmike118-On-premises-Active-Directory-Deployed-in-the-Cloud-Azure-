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

- Setting up a New Forest via promoting a Domain Client
- Creating a Domain Admin user within the domain
- Joining a Client to the Domain
- RDP Setup for Non-Admin users on a Client

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/Z7DAUVo.png"/>
</p>
<p>
The idea of this lab is to create separate Virtual Machines (VMs) for a Domain Controller (DC-1) and a Client (Client-1). While doing this, the lab will also set DC-1's private IP as Client-1's DNS, essentially creating a Virtual Net DNS Server.
</p>
<br />

<p>
<img src="https://i.imgur.com/iFofunL.png"/>
</p>
<p>
Since the VM for DC-1 was configured as a Windows Server, it has access to the Windows Server Manager.

With this, DC-1 will act as the Domain Client for this lab, while Client-1 will be a client desktop on the Virtual Network.

Start -> Server Manager -> 2. Add roles and features -> press "Next" until Server Selection -> Highlight DC-1 (only option) -> checkmark "Active Directory Domain Services" -> press "Next" all until features can be installed, then close popup window.
</p>
<br />

<p>
<img src="https://i.imgur.com/zR6BBot.png"/>
</p>
<p>
  
<p>
<img src="https://i.imgur.com/mr6BGGq.png"/>
</p>
<p>
  

First, press the flag icon in the top right corner and press "Promote this server to a domain controller". Then "Add a new forest" and make the root domain name: mydomain.com. Create a password and install.

After auto-restart, log back into the DC-1 VM. However, instead of the normal RDP log in, add "mydomain.com\<username>"

</p>
<br />

<p>
<img src="https://i.imgur.com/cX6d4ag.png"/>
</p>
<p>

Create a Domain Admin user within the Domain

Start -> Active Directory Users and Computers (ADUC) -> right click "mydomain.com" -> New -> Organizational Unit

Name the new Organizational Unit (OU): "_EMPLOYEES"

Follow the same process and create another OU named: “_ADMINS”

</p>
<br />

<p>
<img src="https://i.imgur.com/A5fK8tJ.png"/>
</p>
<p>


Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” / Cyberlab123!

Add jane_admin to the “Domain Admins” Security Group

Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”

User jane_admin as your admin account from now on

</p>
<br />

<p>

</p>
<p>
Join Client-1 to your domain (mydomain.com)

Login to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart)

Login to the Domain Controller and verify Client-1 shows up in ADUC

Create a new OU named “_CLIENTS” and drag Client-1 into there

</p>
<br />

<p>
<img src="https://i.imgur.com/7MQ9fTd.png"/>
</p>
<p>
  
Setup Remote Desktop for non-administrative users on Client-1

Log into Client-1 as mydomain.com\jane_admin

Open system properties -> Click “Remote Desktop” -> Click "Select users that can remotely access this PC" -> Allow “domain users” access to remote desktop

You can now log into Client-1 as a normal, non-administrative user now

Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)


</p>
<br />

<p>
<img src="https://i.imgur.com/DmbxAtK.png"/>
</p>
<p>
Creating additional users and attempt to long into client-1 with one of the users

Login to DC-1 as jane_admin

Open PowerShell_ise as an administrator

Create a new File and paste the contents of the script into it: https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1

Run the script and observe the accounts being created


</p>
<br />

<p>
<img src="https://i.imgur.com/krlsGUZ.png"/>
</p>
<p>

When finished, open ADUC and observe the accounts in the appropriate OU　(_EMPLOYEES)

attempt to log into Client-1 with one of the accounts (take note of the password in the script)

