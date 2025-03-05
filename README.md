<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" height="80%" width="80%" alt="Ubuntu VM" alt="Traffic Examination"/>
</p>

<h1>Configuring Active Directory Within Azure</h1>
This project demonstrates the implementation of an on-premises Active Directory within Azure Virtual Machines.

<h2>Technologies and Environments Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell
- Operating Systems: Windows 10 (21H2), Windows Server 2022

<h2>Steps & Observations</h2>

<h3>Setup Resources in Azure</h3>

Created the Domain Controller VM (Windows Server 2022) named "DC-1" within a resource group and virtual network.
<p>
  <img src="https://i.imgur.com/hubTfey.png" height="75%" width="100%" alt="vm ms server"/>
</p>

Created the Client VM (Windows 10) named "Client-1" using the same resource group and virtual network.
<p>
  <img src="https://i.imgur.com/XyEmv8f.png" height="75%" width="100%" alt="vm windows"/>
</p>

Set the Domain Controller’s NIC Private IP address to static.
<p>
  <img src="https://i.imgur.com/KHU9kC4.png" height="75%" width="100%" alt="static ip"/>
</p>

<h3>Ensure Connectivity between Client and Domain Controller</h3>

Logged in to Client-1 via Remote Desktop and pinged DC-1's private IP using ping.
<p>
  <img src="https://imgur.com/ZCKwSbW.png" height="75%" width="100%" alt="static ip"/>
</p>

<h3>Install Active Directory</h3>

Logged in to DC-1 and installed Active Directory Domain Services.
<p>
  <img src="https://i.imgur.com/A1V9XJ5.png" height="75%" width="100%" alt="active directory install"/>
</p>

Promoted the server to a Domain Controller.
<p>
  <img src="https://i.imgur.com/zi15fw4.png" height="75%" width="100%" alt="domain controller promotion"/>
</p>

Set up a new forest as myactivedirectory.com (I chose to make it mydomain.com later).
<p>
  <img src="https://i.imgur.com/DCFUVrM.png" height="75%" width="100%" alt="set new forest"/>
</p>

Restarted and logged back into DC-1 as mydomain.com\labuser.
<p>
  <img src="https://imgur.com/rDBU15C.png" height="75%" width="100%" alt="set new forest"/>
</p>

<h3>Create an Admin and Normal User Account in AD</h3>

In Active Directory Users and Computers (ADUC), created two Organizational Units (OU): _EMPLOYEES and _ADMINS.
<p>
  <img src="https://i.imgur.com/cYmv0r7.png" height="75%" width="100%" alt="organizational unit"/>
  <img src="https://i.imgur.com/v02CBPI.png" height="75%" width="100%" alt="organizational unit"/>
</p>

Created a new user Jane Doe with the username jane_admin.
<p>
  <img src="https://i.imgur.com/h546E6L.png" height="75%" width="100%" alt="admin creation"/>
</p>

Added jane_admin to the Domain Admins Security Group.
<p>
  <img src="https://i.imgur.com/mnLwTgq.png" height="75%" width="100%" alt="security group"/>
</p>

Logged out and logged back into DC-1 as mydomain.com\jane_admin. I Used this account for administrative actions going forward.
<p>
  <img src="https://imgur.com/eTLlavg.png" height="75%" width="100%" alt="security group"/>
</p>

<h3>Join Client-1 to the Domain (mydomain.com)</h3>

Set Client-1's DNS settings to DC-1’s Private IP address in the Azure Portal.
<p>
  <img src="https://i.imgur.com/1KRsjI6.png" height="75%" width="100%" alt="client dns settings"/>
</p>

Restarted Client-1.

Logged in to Client-1 as the original local admin and joined it to the mydomain.com domain.
<p>
  <img src="https://i.imgur.com/50wszcP.png" height="75%" width="100%" alt="domain joining"/>
</p>

Restarted the computer and verified that Client-1 appeared in ADUC inside the Computers container.

Created an **OU named **_CLIENTS and moved Client-1 into it.
<p>
  <img src="https://i.imgur.com/vB1n9m0.png" height="75%" width="100%" alt="active directory client verification"/>
</p>

<h3>Setup Remote Desktop for Non-Administrative Users on Client-1</h3>

Logged into Client-1 as mydomain.com\jane_admin.

Opened System Properties and enabled Remote Desktop.

Allowed domain users access to Remote Desktop.

Verified that non-administrative users could log in.
<p>
  <img src="https://i.imgur.com/8BfpT3s.png" height="75%" width="100%" alt="remote desktop setup"/>
</p>

<h3>Create Additional Users and Attempt Login</h3>

Logged into DC-1 as jane_admin.

Opened PowerShell ISE as an administrator.

Created a new script file and pasted the contents of this script (https://github.com/iansherer/configure-useradd-AD/blob/main/createusers.ps1)
<p>
  <img src="https://i.imgur.com/0i8uApf.png" height="75%" width="100%" alt="create users script"/>
</p>

Ran the script and observed the user accounts being created.
<p>
  <img src="https://imgur.com/1mz77Ye.png" height="75%" width="100%" alt="create users script"/>
</p>

Opened ADUC to verify the newly created accounts and attempted to login to one of them.
<p>
  <img src="https://imgur.com/mXLZA1j.png" height="75%" width="100%" alt="create users script"/>
  <img src="https://imgur.com/cLKr2oX.png" height="75%" width="100%" alt="create users script"/>
  <img src="https://imgur.com/PS5fR8R.png" height="75%" width="100%" alt="create users script"/>
</p>

Conclusion 

This project provided insights into setting up and configuring an on-premises Active Directory within Azure Virtual Machines. I successfully created a domain, configured users, and ensured secure access via Remote Desktop. These skills are essential for managing enterprise-level IT environments and maintaining secure network infrastructures
