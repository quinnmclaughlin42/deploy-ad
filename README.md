
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
<img src="https://github.com/user-attachments/assets/7d1e70f6-7b8b-46c1-a75d-0d55fd89c9e5" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We begin by logging into DC-1 (all materials used were built in a prior project), and open the Server Manager. Within the Dashboard we click on ‘Add roles and features’ which will allow us to install Active Directory on the VM. Within the installation wizard and in the ‘Server Roles’ tab, we choose ‘Active Directory Domain Services’ and add the features. We then proceed to finish the installation. Once this is complete, an alert shows up inside the Server Manager Dashboard, allowing us to ‘Promote this server to a domain controller’, which we click. This opens another installation wizard where we choose ‘Add a new forest’ within the Deployment Configuration. This also lets us add our root domain name, which in this case is ‘mydomain.com’. Proceeding will let us create a Directory Services Restore Mode password, and in this case we choose ‘Password1’. After this, we continue through the rest of the installation and let the VM restart as needed.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/ca9bf180-630e-4eef-b007-36f1faeeefd7" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once the VM has restarted, we Remote Desktop back in, but this time providing domain context before our username. In this instance, our username is now ‘mydomain.com\labuser42’ with the same password as before. Attempting to log in without proper domain context would result in a failure to log in since we can no longer log into this VM as a local user, only a domain user. Once we’ve logged back in, we can now open the application ‘Active Directory Users and Computers’ through the Start Menu. Inside this application we can right click on the domain tab, and create a ‘New’ ‘Organizational Unit’. We create two of these for now, one titled “_EMPLOYEES” and the other titled “_ADMINS”. Within ‘_ADMINS’ we again right click, and choose to create a new user. In this case I chose my own name as the admin account, with the username of ‘quinn_mcl’ and a random never expiring password (not recommended, but for the purpose of the lab it helps). After creating the user we enter its ‘Properties’ and open the ‘Member Of’ tab. Here we add this fresh user to ‘Domain Admins’ and apply changes, allowing this account to have full administrative functionality. We then sign out of the Domain Controller with the ‘labuser42’ account and sign back in with our fresh admin account, remembering to add context before our username as we log in.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/fcc5bc71-4520-43e5-95e7-3783df7b90d4" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, we open the Client VM and enter the System Settings from the Start Menu. In this instance, we’re still logged in to the Client as our initial local user. Within the settings we click ‘Rename this PC (advanced)’ which opens a window allowing us to change the VM’s domain. Clicking this lets us choose ‘Domain’ under ‘Member OF’ and input our domain name, which in this case is ‘mydomain.com’. Since we are still logged in as a random user currently, making these changes causes a security pop-up window, where we enter the credentials of our newly created admin account, along with the domain context in the username. This finalizes the changes and asks for the Client VM to restart, which we allow. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/88cc0277-750d-4c49-8ea5-42308dd7eb94" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly, we go back to the Domain Controller VM and reopen Active Directory Users and Computers. Within the domain dropdown we can open the ‘Computers’ folder, and within we see our recently added ‘Client-1’. For the sake of organization, we create one final organizational unit titled “_CLIENTS” and drag the Client-1 computer into it. Now that our domain is officially running we can experiment with its use cases (showcased in the subsequent project).
</p>
<br />
