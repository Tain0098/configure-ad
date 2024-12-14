<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [Canva: How to Deploy on-premises Active Directory within Azure Compute](https://www.canva.com/design/DAGY2HjfWW0/vb5tTczdVwc06kZmpROnzQ/edit?utm_content=DAGY2HjfWW0&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1 Setup Domain Controller in Azure
- Step 2 Setup Client-1 in Azure
- Step 3 Install Active Directory
- Step 4 Create a Domain Admin user within the domain

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/aII7sLA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
Create a Resource Group in Azure (ex..Active Directory)
  
Create a Virtual Network and Subnet

Search Virtual Network in Azure

Click Create and put the resource group above

Name Vnet something easy and you can remember, like(AD-Vnet)

The region has to be all the same(Important)

Click Create

*

<p>
<img src="https://i.imgur.com/JGTMiPb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create the Domain Controller VM (Windows Server 2022) named “DC-1”

Follow the steps above till you get to "Image and Size"

Image = Windows Server 2022

Size = 2 or 4 vcpus

Region = same as above

Note down the Username/Password

Check the "licensing" box

Skip on Disk to Networking

On Networking, in the Virtual Network section -> pick the Virtual Network that you created

Sometimes the VN name doesn't show up, in that case, restart the page and the filling process again

You can skip management to Create

This Virtual Machine will serve as the Domain Controller or the Head Computer 

*

<p>
<img src="https://i.imgur.com/MUjui37.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create the Client VM (Windows 10) named “Client-1”

Image = Windows 10 pro (don't mistake it)

Size = 2 or 4 vcpus

Note down the Username/Password

Region = same as above

Skip on Disk to Networking

On Networking, in the Virtual Network section -> pick the Virtual Network that you created

Sometimes the VN name doesn't show up, in that case, restart the page and the filling process again

This will serve as a regular computer (like a radome computer you find in school or library that you can log in)

*

<p>
<img src="https://i.imgur.com/xz8sUZr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Set Domain Controller’s NIC Private IP address to be static

Steps to Set NIC Private IP Address to Static in Azure:

Login to the Azure Portal:

Open the Azure Portal.
Navigate to the Virtual Machines section.
Locate the Domain Controller VM:

In the Azure portal, go to Virtual Machines.
Search for and select the VM that is running your Domain Controller.
Navigate to the Networking Settings:

Once inside the VM's management page, scroll down and select Networking under the "Settings" section.
Here you will see the list of network interfaces (NICs) associated with the VM.
Select the Network Interface:

Click on the NIC attached to the Domain Controller.
This will take you to the NIC's configuration page.
Configure the Private IP Address:

In the NIC settings, under the IP configurations section, select the IP configuration that is currently assigned to the VM (usually named something like "ipconfig1").

Set the IP Address to Static:

Under the Private IP address settings, you will see an option for Assignment.

Change this setting from Dynamic to Static.

Select "Static"

After entering the static IP address, click Save at the top of the page to apply the changes.

*
<p>
<img src="https://i.imgur.com/t3Bh9QK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Steps to Set DNS to Domain Controller’s Private IP Address in Azure

Go to the Azure Portal.

Locate the Virtual Machine

In the Azure portal, go to Virtual Machines.

Find and select the Windows 10 Pro VM for which you want to set the DNS to the Domain Controller’s IP.

Identify the Domain Controller’s Private IP

Next, locate the Domain Controller’s private IP address (if you don't know it already).

Go to Virtual Machines in the Azure portal.

Select the Domain Controller VM.

Under Networking, you will see the Private IP Address of the Domain Controller's NIC.

Modify DNS Settings on the Windows 10 VM's NIC

Go to the Networking section for the Windows 10 Pro VM.

Under the Network Interface section, click on the Network Interface name associated with the Windows 10 Pro VM.

This takes you to the NIC's settings for that VM.

Configure DNS Settings for the NIC

In the Network Interface pane, under the IP configurations section, click on the IP configuration (usually named ipconfig1).

In the IP configuration settings, scroll down to the DNS servers section.

Set the DNS servers:

Choose Custom for the DNS server settings.

In the DNS servers section, enter the private IP address of the Domain Controller (which you identified in step 3).

Example:
DNS servers: 10.0.0.4 (Replace this with your Domain Controller's private IP address)

Click Save

From Client-1, open PowerShell and run ipconfig /all

The output for the DNS settings should show DC-1’s private IP Address

*
<p>
<img src="https://i.imgur.com/gIigyoc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Install Active Directory

Login to DC-1

Press Windows + R, type ServerManager, and press Enter to open the Server Manager.

Click on Add Roles and Features.

In the Add Roles and Features Wizard, select the following options:

Role-based or feature-based installation.

Choose the local server

<p>
<img src="https://i.imgur.com/9pfcjxr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Under Roles, select Active Directory Domain Services.

Under Features, select AD DS and AD LDS Tools.

Click Next and then Install.

Wait for Installation to Complete:

The installation process should take a few minutes. Once done, it will show you a message saying that the role has been installed.

Promote the Windows 10 Pro VM to a Domain Controller

After installing the Active Directory Domain Services role, you need to promote the machine to a Domain Controller.

Open Active Directory Domain Services Configuration Wizard:

Open Server Manager (or press Windows + R and type dcpromo to run the Active Directory Domain Services Configuration Wizard).

A wizard will guide you through the domain controller promotion.

Choose to Add a New Forest (if this is the first DC):

Since you are likely setting this up as the first Domain Controller, select Add a new forest.

Enter the Root domain name for your Active Directory domain.

Setup a new forest as mydomain.com (can be anything, just remember what it is)


Set Domain and Directory Services Restore Mode (DSRM) Password:

Set a password for Directory Services Restore Mode (DSRM). This password is used when you need to restore Active Directory or perform other recovery tasks.

Complete the Domain Controller Configuration:

Continue through the wizard, accepting default settings unless you have specific requirements.

The wizard will verify DNS configuration, NetBIOS name, and other settings. Ensure the DNS settings point to the local private IP of the Domain Controller.

Restart the VM:

Once the wizard completes, you will be prompted to restart the system. Restart the VM to apply the changes.

Log back into DC-1 as user: mydomain.com\DC-1 username

The VM should now be a Domain Controller for your new Active Directory domain.

Verify AD DS Services:

Open Active Directory Users and Computers (you can find it in the Start Menu).

<p>
<img src="https://i.imgur.com/R08MVZd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Good Job! 
You successfully setup Active Directory on this VM

If you do it right, you should have the same tap as the picture above.













