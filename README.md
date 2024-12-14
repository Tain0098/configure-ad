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
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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











