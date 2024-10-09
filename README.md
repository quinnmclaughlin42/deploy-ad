
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Configuring On-premises Active Directory within Azure VMs</h1>
This walkthrough showcases the deployment and configuration of Active Directory within Azure Virtual Machines<br />


<h2>Video Demonstration</h2>

- ### [YouTube: Implementing Active Directory Infrastructure using Microsoft Azure VMs - Quinn McLaughlin](https://youtu.be/vqapby2E5ps)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Network)
- Remote Desktop
- Active Directory

<h2>Operating Systems Used </h2>

- Windows Server 2022 Datacenter
- Windows 10 Pro (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Deploy Active Directory and promote 'DC-1' to a Domain Controller
- Create new Organizational Units and create an admin account within AD Users and Computers
- Link the Client VM into the fresh domain
- Showcase domain connection by observing the changes within AD Users and Computers

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://github.com/user-attachments/assets/ada57f78-8d6e-4513-a2a5-d2a63ce5f4c3" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To begin, we log into Microsoft Azure and create a new Resource Group to house everything we’ll build. Once this is created, we’ll create a Virtual Network server for our consequent Virtual Machines to run within. In this scenario, I decided to have the region of our Virtual Network set to ‘East US 2’, and as the VMs are built, they will also be put into the same region. Once the Virtual Network deploys we can build both of our Virtual Machines, with the key difference between them being the operating system we choose for each. One will be launched with Windows Datacenter (DC-1), and the other will run Windows 10 (Client-1). Before subsequent deployment, we also make sure each VM is set to the Virtual Network we created previously, which can be found within the Networking tab. After this, both VMs can be deployed.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/b0310fbc-c163-4345-8f66-32aa4c998156" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After successful deployment of the Datacenter VM (we don’t have to wait for both to deploy for this step), we navigate to its ‘Networking’ tab within Azure. Here we can click on ‘Network Settings’ in the dropdown menu which allows us to see DC-1’s Virtual NIC. Clicking on this will bring us to a window where we can set the VM’s Private IP Address settings to static, which is necessary for utilizing it as a Domain Controller. In this case, the Private IP Address was locked to ‘10.0.0.4’. We can then go into the VM’s ‘Overview’ tab to copy the Public IP Address and Remote Desktop into the VM, using the credentials we chose while deploying the VM to log in. Once logged in, we’ll open Windows Firewall (wf.msc) and disable it within the Domain, Private, and Public Profile tabs.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/57b29aa8-142d-447c-a2e0-a17593638460" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, we navigate back into Azure and follow the same steps as we did earlier to access our ‘Client’ VM’s Virtual NIC. Once inside, we’ll click ‘DNS Servers’ from the ‘Settings’ drop-down tab, and change the DNS Server to ‘Custom’. This will allow us to input our Domain Controller’s Private IP Address (why it was necessary to make the IP Address static). This lets our VMs communicate with each other, with the Client VM using the DC VM as its DNS Server from now on. After saving, we also choose to restart the Client VM to ensure the changes update. We then copy the Client VM’s Public IP Address, and Remote Desktop in, just as we did with the Domain Controller VM. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/140f8c67-2834-42df-b674-05f23c507f80" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly, to showcase the connection of the two VMs, we open Windows Powershell within the Client VM. Typing ‘ping’ alongside the DC’s Private IP Address (10.0.0.4) will show us that the VMs are able to send and receive data from one another. To dig a bit deeper, we’ll also type ‘ipconfig /all’, which upon scrolling down will show the DC’s Private IP Address listed as ‘DNS Servers’, showing us the infrastructure is now in place to deploy Active Directory (showcased in a separate project).
</p>
<br />
