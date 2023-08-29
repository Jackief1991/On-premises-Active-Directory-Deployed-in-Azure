<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create (2)Virtual Machines in Azure (Domian Control-1(DC-1), Client-1)
- Install Active Directory on (DC-1)
- Join Client-1 to Main Domain
- Create a list of User Accounts 

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/1fFUONz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Create your first VM. Name it DC-1 for Domain Controler and use windows server 2022 (use 2 VCPUs at least) Click Create.
- Create another vm Naming it Client-1 making sure it is with in the same network as your first VM using Windows 10. Click Create. 
- Set the Domain Controler(DC-1)VM IP address to static. Go to Networking -> Go to Network Interface  -> IP Configurations -> from dynamic to static
- Ensure both VMs are connected.
- Go to Client-1 in remote desktop and log in. Run command prompt and Ping DC-1 private ip address witih -t
- Log in to DC-1 with remote desktop. CLick start and search firewall. Open  Windows firewall with advanced settings
- -> inbound rules. Sort protocol Section -> look for icmpv4 and allow them by clicking enable rule
- ping should work on the Client-1 VM.

<p>
<img src="https://i.imgur.com/R32Xku7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Installing Active Directory on DC-1.
 - Open DC-1 VM remote Desktop. Open Service Manager -> go to add roles and features -> Click next -> Click next -> next -> click Active Directory Domain Services-> click next all the way to install then click install.
 - in the service manager, Click the yellow flag at the top of the page so you can set as domain controler and finish installing acitive directory. 
 - name the domain server mydomain.com -> set password and remember it->click next all the way through.
 -computer will shut down and you will have to reconnect to the DC-1 VM. log in with the domain controler name. mydomain.com/"username" if you cant log in the normal way
 - Click start search active directory. Right click mydomain.com -> new-> organizational unit call it _EMPLOYEES press ok. Do samething making a folder for _ADMIN. Refresh the directory.
- go to _ADMIN. create new user title it an admin account make a password uncheck the change password setting. click ok.
- right click the user account ust made and add  domain click check names and click domain admins group and apply and ok it.
 - log out of DC_1 and log back in under the admin account that you just made.
  
</p>
<br />

<p>
<img src="https://i.imgur.com/CByVR00.png" alt="Disk Sanitization Steps"/>
</p>

- In the Azure portal go to virtual machines and click DC-1 -> networking -> you will see NIC Private IP copy it.
- Still in the Azure portal go back to you VM's and click Client-1 -> networking -> Click the network interface ID. -> DNS Servers on the side panel -> Custom and paste the copied private ip address from DC-1 into the box. Click save.
- Go back to the vm's page in Azure and click Client-1 -> Restart
- Log into Client-1 for remote desktop as your local admin account(the one before you mad the new admin account)
- Open Command Prompt onCLient-1 remote desktop-> type in: ipconfig/all should see
DC-1 ip address
- On Client-1 VM ->Right click Start menu -> system -> rename this pc advanced-> click change -> click domain and type in domain.com.-> click ok, a new window should pop up for Computer Name/ Domain Changes. Use the domain admin log in you created the second time (not your local admin account).
- Computer will need to restart to join the domain.

</p>
<br 
<p>
<img src="https://www.top-password.com/blog/wp-content/uploads/2019/02/remote-desktop-settings.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

- You should be able to log in on the Client-1 computer as the Admin account you created for domains (not local admin account from first step). 
- Right click start menu-> system-> remote desktop-> click select users that can remotely access this PC. ->Click add type in domain users->check names-> click ok
- on DC-1 Start menu type in Active Directory click users and computers one.
- click user folder and click domain users anyone in that folder should be able to log into either computer.
- Create a bunch of additional users and attempt to log into client-1 with one of the users
Login to DC-1 as the Domain Controler Admin account
Open PowerShell_ise as an administrator
Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
Run the script and observe the accounts being created
When finished, open ADUC and observe the accounts in the appropriate OU
attempt to log into Client-1 with one of the accounts (take note of the password in the script)


<br />
