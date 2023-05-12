<h1>Active Directory Home Lab</h1>

 ### [YouTube Demonstration] WIP

<h2>Description</h2>
In this lab we're going to be walking through how to create an Active Directory home lab environment using Oracle VM Vitrual Box. Configuring and running this lab helps develop the understanding of how active directory and windows networking works. 
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Oracle VM Virtual Box</b>

<h2>Environments Used </h2>

- <b>Windows 10</b>
- <b>Server 2019</b>

<h2>Program walk-through:</h2>

<p align="center">
Launch Server 2019 on Oracle VM VirtualBox: <br/>
<img src="https://i.imgur.com/jHPptLI.png" height="80%" width="80%" alt="ADLab"/> <br />
  <br />
Open network settings and check network details for assigned IPv4 addresses. <br/>
1 will be your proper home IP address (We will name this one Internet) <br/>
2nd will most likely have the APIPA address assigned to it 169.254.x.x (We will name this one Internal) <br/>
  <br/>
<img src="https://i.imgur.com/qWpujTE.png" height="80%" width="80%" alt="ADLab"/> <br />
  <br/>
<img src="https://i.imgur.com/GAC852M.png" height="80%" width="80%" alt="ADLab"/> <br />
  <br />
We will be replacing the APIPA address from the internal network: <br/>
Select properties, make sure you're in the 'Networking' tab. Locate Internet Protocol Version 4 (TCP/IPv4) and select it and choose "Use the following IP address" <br/>
Assign the following: IP - 172.16.0.1 | Subnet Mask - 255.255.255.0 | Preferred DNS server - 127.0.0.1 <br/>  
<img src="https://i.imgur.com/7bAL1HY.png" height="80%" width="80%" alt="ADLab"/> <br />
  <br />
We are going to change the name of our PC. You can do this by right clicking the windows icon in the bottom right and selecting system. <br/>
Click on rename this PC and for this lab we will just rename the PC to "DC". <br/>
<img src="https://i.imgur.com/YHUllpu.png " height="80%" width="80%" alt="ADLab"/> <br /> 
  <br />
Navigate back to the server manager and select "Add roles and features": <br/>
After selecting our server find and select "Active Directory Domain Services" and click next until prompted to install  <br/>
<img src="https://i.imgur.com/LHVewEy.png" height="80%" width="80%" alt="ADLab"/> <br />
<img src="https://i.imgur.com/kRIL7XM.png" height="80%" width="80%" alt="ADLab"/> <br />
  <br />
Click the flag with a caution sign at the top right, click "Promote this server to a domain controller" <br/>
Select add new forest and name it "mydomain.com" on the next page enter a password (I'll be using Password1) and then proceed to install. <br/>    
<img src="https://i.imgur.com/JifvIQG.png" height="80%" width="80%" alt="ADLab"/> <br />
  <br />
Next we will go to "Active Directory Users and Computers" so we can add our own admin account to the server <br/>
<img src="https://i.imgur.com/nzhqLE2.png" height="80%" width="80%" alt="ADLab"/> <br />
  <br />
Expand "mydomain.com" and right click add new "Organizational Unit" <br/>
Enter your first and last name in the respective boxes. <br/>
For the User logon name we will use (a denotes admin) a- first initial full last name (ex - a-sbarrett) <br/>
We'll use the same password as everything else to keep the lab simple (Password1) <br />
<img src="https://i.imgur.com/3Vq45Ec.png" height="80%" width="80%" alt="ADLab"/> <br />
  <br />
Currently this is a normal user account to make it an admin we will right click user and go to properties <br/>
Navigate to "Member of" and "Domain Admins" and press OK <br />
<img src="https://i.imgur.com/fjBnakX.png" height="80%" width="80%" alt="ADLab"/> <br />
  <br />
You can now logout and login using your new admin account with the credentials <br />
Example login | Username: a-sbarrett Password: Password1 <br />
  <br />
We'll navigate back to the server manager and once again click "Add roles and features" <br />
Select Remote Access(roles) in the menu and when asked in 2 next clicks select "routing" option and then install <br />  
<img src="https://i.imgur.com/LHVewEy.png" height="80%" width="80%" alt="ADLab"/>
  <br />
<img src="https://i.imgur.com/UMF7tvv.png" height="80%" width="80%" alt="ADLab"/> <br />
  <br />
At the top right of the server manager application press tools and select routing and remote access <br />
Right click "DC (local) and choose configure and enable, select "Network address translation (NAT) <br />
Select "Internet" from the choices and finish <br />  
<img src="https://i.imgur.com/80Hl8dt.png" height="80%" width="80%" alt="ADLab"/> <br />
  <br />
<img src="https://i.imgur.com/TCtOxuU.png" height="80%" width="80%" alt="ADLab"/> <br />
  <br />
Navigating back to the server manager we'll again add roles and features and select "DHCP server" and install <br />
<img src="https://i.imgur.com/60W1r0y.png" height="80%" width="80%" alt="ADLab"/> <br />
  <br />  
On the server manager click tools and then DHCP in the dropdown. <br />
Expand dc.mydomain.com, right click IPv4 and add new scope. <br />
We're going to name it our IP range (172.16.0.100-200) <br />
Assign start IP 172.16.0.100 and end IP 172.16.0.200 <br />
Set length to 24 and subnet to 255.255.255.0 <br />
<img src="https://i.imgur.com/i6A3WCG.png" height="80%" width="80%" alt="ADLab"/>  <br />
  <br />
We won't be adding exclusions and we'll keep default lease time. <br />
Enter our domain controller's IP (172.16.0.1) and hit "Add" <br />  
<img src="https://i.imgur.com/eesUkJp.png" height="80%" width="80%" alt="ADLab"/> <br />  
<br />
We don't change or add anything else and proceed to activate the scope. <br />
<img src="https://i.imgur.com/A2k7dRq.png" height="80%" width="80%" alt="ADLab"/> <br />  
<br />
If our IPv4 icon is still red we may need to authourize the domain. <br />
To authourize right click "dc.mydomain.com" and click authourize and then once more and click refresh. <br />
<img src="https://i.imgur.com/nUDXAi8.png" height="80%" width="80%" alt="ADLab"/> <br />
<br />
We will be using a powershell script in Windows PowerShell ISE. <br />
Run "Windows PowerShell ISE" as administartor. <br />
<img src="https://i.imgur.com/Gugma7Q.png" height="80%" width="80%" alt="ADLab"/> <br />
<br />
We'll open our script (1_CREATE_USERS) <br />
<img src="https://i.imgur.com/MGvDaCk.png" height="80%" width="80%" alt="ADLab"/> <br />
<br />
In the command prompt we will change the security settings by entering "Set_ExecutionPolicy Unrestricted" <br />
We are going to change the directory so we can pull from out names.txt file (cd C:\Users\s-barrett\Desktop\AD_PS-master) <br />
<img src="https://i.imgur.com/iiHCBjK.png" height="80%" width="80%" alt="ADLab"/> <br />
<br />
Once we run script accounts will start to be created. <br />
<br />  
<br />
Now we'll make a "Client" machine for our domain. Create a new VM and name it "Client" in the settings before launching this VM set our network to "attached to: NAT" <br />
<br />  
Once our machine is running we will rename this PC. Right click the start menu, system, and this time scroll down and select "rename this PC (advanced)" and change. <br />
The computer name will be "Client1" and we will also add this PC to our domain. Under "Member of" enter "mydomain.com" in the "Domain" box <br />
You can use your admin account to confirm. In my case (Username: a-sbarrett password: Password1) <br />
<img src="https://i.imgur.com/jS7vXIU.png" height="80%" width="80%" alt="ADLab"/> <br />
<br />   
If you navigate back to our DHCP on our VM running the server we can find our new client machine. <br />
Server Manager > Tools > DHCP > Expand the following > dc.mydomain.com > IPv4 > Scope[172.16.0.0] 172.16.0.100-200 > Click "Address Leases" and our client will show up. <br />
<img src="https://i.imgur.com/diC6qxy.png" height="80%" width="80%" alt="ADLab"/> <br />
>br />
If you look under "Active Directory Users and Computers" you can also find our client under "Computers" <br />  
<img src="https://i.imgur.com/cxIQdo0.png" height="80%" width="80%" alt="ADLab"/> <br />
<br />
On that client pc you can now log out and when signing in you can select "Other User" and it will show us signing into "MYDOMAIN" <br />
We can use an account we made through our script to login. Since we added our name to the list I can use (Username: sbarrett Password: Password1) to login. <br />
<br />
<br />
<br />
<br />
Essentially we have created a mini corporate network with this process. <br/ >
An overall view of this would be: We got hired at a company > Our name was added to a batch file. <br />
The next morning the script ran and created all the new accounts (including ours). <br />
Our client 1 we made can be seen as our work laptop <br />
We can login to our work laptop with our credentials since they have already been created and linked to the domain / network.



</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
