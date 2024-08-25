<p align="center">
<img src="https://i.imgur.com/Ucqw15T.jpeg" alt="Active Directory" width=500 height=300/> 
</p>

<h1>Setting Up Active Directory on Azure with Powershell ISE</h1>
<p>In this tutorial, you'll learn how to set up a Domain Controller and a Client VM in Azure, configure network settings, install Active Directory, create and manage user accounts, and ensure proper connectivity and remote access. Follow these steps to establish a basic Active Directory environment for managing network resources and users.</p>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Microsoft RD Client (Remote Desktop)
- Windows Powershell ISE

<h2>Operating Systems Used </h2>

- macOS Sonoma ***(if you own Macbook Air M1 or M2; it does not matter what type of macOS you own)***
- Windows 10 or Windows 11 Home or Pro ***(if you own either of these OS)***

-----

***Note: Create a Virtual Network manually (before following this guide) at Microsoft Azure since you have to make sure the network ID and subnet has to be identical when deploying Windows Active Directory.***

-----

### Setting Up Resources in Azure

1. **Create Domain Controller VM**
   - Name: `DC-1`
   - OS: Windows Server 2022
   - Note the Resource Group and Virtual Network (VNet) created.

2. **Configure Network Settings**
   - Set `DC-1`’s NIC Private IP address to static.

3. **Create Client VM**
   - Name: `Client-1`
   - OS: Windows 10
   - Use the same Resource Group and VNet as `DC-1`.
   - Confirm both VMs are in the same VNet (check with Network Watcher).

### Verify Connectivity

1. **Test Network Connection**
   - Log into `Client-1` using Remote Desktop.
   - Open Command Prompt and run: `ping -t <DC-1’s private IP address>` to continuously ping `DC-1`.
   - Log into `DC-1` and enable ICMPv4 on the Windows Firewall. ***(Type wf.msc from "Search" menu below left)***
   - Check if the ping from `Client-1` succeeds.

### Install Active Directory

1. **Setup Active Directory**
   - Log into `DC-1`.
   - Install Active Directory Domain Services.
   - Promote `DC-1` to Domain Controller:
     - Create a new forest with domain name `mydomain.com` (or any name you choose).
   - Restart `DC-1` and log back in as: `mydomain.com\labuser`.

### Create User Accounts in AD

1. **Setup Organizational Units (OUs)**
   - In Active Directory Users and Computers (ADUC):
     - Create an OU named `_EMPLOYEES`.
     - Create another OU named `_ADMINS`.

2. **Create User Accounts**
   - Create user `Jane Doe` with username `jane_admin` and the same password.
   - Add `jane_admin` to the “Domain Admins” Security Group.
   - Log out and log back in as `mydomain.com\jane_admin`.

### Join Client-1 to Domain

1. **Update DNS and Restart**
   - In the Azure Portal, set `Client-1`’s DNS settings to `DC-1`’s private IP.
   - Restart `Client-1` from the Azure Portal.

2. **Join Domain**
   - Log into `Client-1` (Remote Desktop) as the local admin (`labuser`).
   - Join `Client-1` to the domain (`mydomain.com`).
   - Restart `Client-1` after joining the domain.

3. **Verify Domain Membership**
   - Log into `DC-1` (Remote Desktop).
   - Check `Client-1` in ADUC under the “Computers” container.
   - Optionally, create an OU named `_CLIENTS` and move `Client-1` there for organization.

### Configure Remote Desktop Access

1. **Allow Remote Desktop for Non-Admin Users**
   - Log into `Client-1` as `mydomain.com\jane_admin`.
   - Open System Properties and click “Remote Desktop”.
   - Allow “domain users” to access Remote Desktop.
   - You can now log into `Client-1` as a regular user.

### Create Additional Users

1. **Generate User Accounts**
   - Log into `DC-1` as `jane_admin`.
   - Open PowerShell ISE as an administrator.
   - Copy and paste the script from [Generate-Names-Create-Users.ps1](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into a new file.
   - Run the script to create user accounts.
   - Check ADUC for newly created accounts in the appropriate OU.

2. **Test User Login**
   - Attempt to log into `Client-1` with one of the newly created user accounts (use the password from the script).

-----

Congratulations! You've successfully set up a Domain Controller and Client VM in Azure, configured network settings, and installed Active Directory. You also created and managed user accounts and ensured connectivity and remote access. This is the fundamental environment that you're able to manage network resources, users, and configurations that you'll continue to develop and maintain your IT infrastructure.
