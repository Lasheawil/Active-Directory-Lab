<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in Azure</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 11

<h2>High-Level Deployment and Configuration Steps</h2>

- Create 2 virtual machines (Domain controller and Client-1)
- Install active directory on Domain controller and join domain from client-1 to domain controller
- Create an Admin and Normal User Account in AD
- Setup Remote Desktop for non-administrative users on Client-1 and Created a bunch of additional users and attempt to log into client-1 with one of the users


<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://github.com/Lasheawil/Active-Directory-Lab/blob/main/ADLab/CreateVMs.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/Lasheawil/Active-Directory-Lab/blob/main/ADLab/dc-1%20NIC%20IP%20static.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/Lasheawil/Active-Directory-Lab/blob/main/ADLab/client-1%20NIC%20DNS%20point%20to%20dc-1.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>
<p>
Created the Domain Controller VM (Windows Server 2022) named “DC-1” and created the Client VM (Windows 10) named “Client-1” in Microsoft Azure. I then set the Domain Controller’s NIC Private IP address to be static. I then pointed Client-1 NIC DNS settings to DC-1's Private IP address. This way, Client-1 will look towards DC-1 for all resources.


</p>
<br />

<p>
<img src="https://github.com/Lasheawil/Active-Directory-Lab/blob/main/AD%20Lab%202/ActivedirectoryDomainServices.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/Lasheawil/Active-Directory-Lab/blob/main/AD%20Lab%202/promote.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/Lasheawil/Active-Directory-Lab/blob/main/AD%20Lab%202/add%20new%20forest.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>

  </p>
<p>
Login to DC-1 and install Active Directory Domain Services. Promote as a DC: Setup a new forest as mydomain.com. Restart and then log back into DC-1 as user: mydomain.com\labuser  

</p>
<br />

<p>
<img src="https://github.com/Lasheawil/Active-Directory-Lab/blob/main/ADLab/client-1%20NIC%20DNS%20point%20to%20dc-1.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/Lasheawil/Active-Directory-Lab/blob/main/AD%20Lab%202/change%20client%201%20name%20to%20mydomain.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/Lasheawil/Active-Directory-Lab/blob/main/AD%20Lab%202/verify%20client%201%20is%20in%20ad.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>

</p>
<p>
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address. From the Azure Portal, restart Client-1. Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart). Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain. Created a new OU named “_CLIENTS” and drag Client-1 into there. 

</p>
<br />

<p>
<img src="https://github.com/Lasheawil/Active-Directory-Lab/blob/main/AD%20Lab%202/OU%20empl%20and%20admin.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/Lasheawil/Active-Directory-Lab/blob/main/AD%20Lab%202/new%20user%20jane%20admin.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/Lasheawil/Active-Directory-Lab/blob/main/AD%20Lab%202/jane%20in%20folder.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>

</p>
<p>
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Created a new OU named “_ADMINS”. Created a new employee named “Jane Doe” with the username of “jane_admin”
Add jane_admin to the “Domain Admins” Security Group. Logged out of the Remote Desktop connection to DC-1 and logged back in as “mydomain.com\jane_admin”
User jane_admin as my admin account. 
</p>
<br />

<p>
<img src="https://github.com/Lasheawil/Active-Directory-Lab/blob/main/AD%20Lab%202/usurs%20who%20can%20remote%20access%20this%20pc.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/Lasheawil/Active-Directory-Lab/blob/main/AD%20Lab%202/creating%20users.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
</p>
<p>
Logged into Client-1 as mydomain.com\jane_admin and open system properties. Clicked “Remote Desktop”. Allowed “domain users” access to remote desktop
Logged in to DC-1 as jane_admin. Opened PowerShell_ise as an administrator. Created a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
Run the script and observed the accounts being created. When finished, open ADUC and observe the accounts in the appropriate OU
attempted to log into Client-1 with one of the accounts.

</p>
<br />
