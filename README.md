<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Setting Up Active Directory in Azure</h1>
<p>In this guide, I will create an Active Directory within Azure Virtual Machines. We will have the domain and client before setting up an Active Directory Domain on a Windows Server 2022 machine and join a Windows 10 client to the domain. And I will create user accounts, enabling remote desktop access and other related tasks. It will be a tedious tasks but I have provided the guide to the best of my ability.</p>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- MacBook Air
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server
- Windows 10 Pro

-----

# Step 1: Set Up Resources in Azure</h1>

## Create the Domain Controller VM</h2>

- Name it "DC-1" and use Windows Server 2022.
- Remember the Resource Group and Virtual Network (Vnet) created during this step.

## Set Static IP for Domain Controller

- Make sure DC-1's NIC Private IP address is set to static.

## Create the Client VM

- Name it "Client-1" and use the same Resource Group and Vnet from Step 1.

## Ensure Both VMs are in the Same Vnet

- You can verify this using Network Watcher.

-----

# Step 2: Ensure Connectivity

## Check Connection

- Log in to "Client-1" with Remote Desktop.

- Ping "DC-1" in Powershell by typing in ping -t 'ip address' (make sure you use these - <> before placing word of 'ip address'). Make sure to replace 'ip address' with DC-1's private IP to confirm a successful connection.

## Enable ICMPv4

- On "DC-1," enable ICMPv4 in the Windows Firewall.

## Verify Ping

- Check back on "Client-1" to confirm that the ping to DC-1 succeeds.

-----

# Step 3: Install Active Directory

## Install Active Directory

- Log in to "DC-1" and install Active Directory Domain Services.

## Promote as a Domain Controller

- Set up a new forest (e.g., mydomain.com).

- After installation, restart DC-1.

## Log Back In

- Log back into DC-1 as user "mydomain.com\labuser."

-----

# Step 4: Create Admin and Normal User Accounts

## Create Organizational Units (OUs)

- In Active Directory Users and Computers (ADUC), create an OU called "_EMPLOYEES."

- Create another OU named "_ADMINS."

## Create Admin User

- Create a new employee named "Jane Doe" with the username "jane_admin."

- Add "jane_admin" to the "Domain Admins" Security Group.

## Switch to Admin Account

- Log out/close the Remote Desktop connection to DC-1.

- Log back in as "mydomain.com\jane_admin."

- Use "jane_admin" as your admin account from now on.

-----

# Step 5: Join Client-1 to Your Domain

## Update DNS Settings

- In the Azure Portal, set Client-1's DNS settings to the DC's Private IP address.

## Restart Client-1

- Restart Client-1 from the Azure Portal.

## Join Client-1 to Domain

- Log in to Client-1 (Remote Desktop) as the original local admin (labuser).

- Join it to the domain (Client-1 will restart).

## Verify in ADUC

- Log in to the Domain Controller (Remote Desktop).

- Confirm that Client-1 shows up in Active Directory Users and Computers (ADUC) inside the "Computers" container on the root of the domain.

-----

# Step 6: Set Up Remote Desktop for Non-Admin Users on Client-1

## Log into Client-1 as jane_admin

- Open system properties.

- Click "Remote Desktop."

- Allow "domain users" access to remote desktop.

## Non-Admin Access

- You can now log into Client-1 as a normal, non-administrative user.

# Step 7: Create Additional Users and Test

## Create Additional Users

- Log in to DC-1 as jane_admin.

- Open PowerShell_ise as an administrator.

- Copy and run the script from this [link](https://github.com/anumkhanit/AD_Users/blob/main/Generate-Names-Create-Users.ps1).

- Observe the accounts being created in the appropriate OU.

## Test Additional User Logins

- Attempt to log into Client-1 with one of the newly created accounts (note the password from the script).

-----

# Conclusion
Congratulations! You've successfully set up Active Directory in Azure and created user accounts for your domain. You can now manage your domain and user access efficiently.
