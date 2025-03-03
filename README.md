 configure-ad
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This project outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>
<p>
 Create a virtual machine ,which is running as a windows server.This server will act as a Domain Controller.we will have other vm which run as a client.the client will join the dc.the users in the dc can login to the client.the dc and the client have a virtual nic and ip address.usually the dns client is connected to the azure dns server automatically.inorder for the client to join the dc , we need to ask the client to use the domain controller dns server.So dns ip address of the client is customized to dc ip address(set to static)
</p>
<p>
<img src="https://github.com/user-attachments/assets/66b9f35d-ee58-4551-a3f2-47a4e04fc713" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>Step 1:Setup Domain Controller in Azure
Create a Resource Group.Create a Virtual Network and Subnet
Create the Domain Controller VM (Windows Server 2022) named “DC-1”
After VM is created, set Domain Controller’s NIC Private IP address to be static
Log into the VM and disable the Windows Firewall (for testing connectivity)
</p>
<p>
<img src="https://github.com/user-attachments/assets/80a52d2c-a440-419a-9e58-fcb256491505" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>Step 2: Setup Client-1 in Azure
Create the Client VM (Windows 10) named “Client-1”
Attach it to the same region and Virtual Network as DC-1
After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address</p>
<p>
<img src="https://github.com/user-attachments/assets/24f2735b-dbf0-404e-9fef-70d93e4d238e" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 Step3: Log into the VM(server-dc-1) and disable the Windows Firewall (for testing connectivity)

</p>
<p>
<img src="https://github.com/user-attachments/assets/ce042411-f2d4-45ec-845b-140d8490ac78" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 4: Login to Client-1
Attempt to ping DC-1’s private IP address
Ensure the ping succeeded
<p>
<img src="https://github.com/user-attachments/assets/623f3bbb-2799-4861-9068-1095807166e8" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

From Client-1, open PowerShell and run ipconfig /all
The output for the DNS settings should show DC-1’s private IP Address
</p>
<p>
<img src="https://github.com/user-attachments/assets/b7d84a38-be31-4100-ae9a-304202884bcd" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h1>
 <p>Deploying Active Directory</p>
</h1>
<h2>
 Step1 :Login to DC-1 and install Active Directory Domain Services
</h2>
<p>
<img src="https://github.com/user-attachments/assets/53205597-0100-4354-a7d1-b64de6c1269d" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/user-attachments/assets/782b1f61-863a-4a32-adb7-dd24e65fbb5d" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>Promote as a DC: Setup a new forest as mydomain.com
 <p>
<img src="https://github.com/user-attachments/assets/c78e4202-eb43-484a-8042-d15ca5eebce8" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</p>
<h1>
 Create a Domain Admin user within the domain</h1>
<h2><p> In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Create a new OU named “_ADMINS”
Create a new employee named “Remya hk”  with the username of “remyahk”
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Create a new OU named “_ADMINS”
Add remyahk to the “Domain Admins” Security Group
Log out / close the connection to DC-1 and log back in as “mydomain.com\remyahk”
User jane_admin as your admin account.</p></h2>
<p>
<img src="https://github.com/user-attachments/assets/44444b9b-9861-4aa6-9c2c-db36f6b540d7" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/user-attachments/assets/75b793e8-a555-4b4b-92b6-e1658099b5bf" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
<img src="https://github.com/user-attachments/assets/6a86f731-f993-4f30-bf34-035268dcb987" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h1>
 Join Client-1 to your domain (mydomain.com)</h1>
<h2>Login to Client-1 as the original local admin (labuser) and join it to the domain
 <p>
<img src="https://github.com/user-attachments/assets/33daccb3-ae3b-48e9-b706-0433c1c7932a" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
 </p>
</h2>

<h3>
 <p>
  Login to the Domain Controller and verify Client-1 shows up in ADUC
<p>
 <img src="https://github.com/user-attachments/assets/1c819790-2d0b-4b72-a44b-21293f75bf0b" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
 </p>
</h3>
<h1>

 Setup Remote Desktop for non-administrative users on Client-1

</h1>
<h2>
 <p> Login to client -1 and add the domain users on to to remote desktop access</p>
 <p>
 <img src="https://github.com/user-attachments/assets/7d52fedf-7247-4fbe-bb06-7232d23e2fe1" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h2>
 Open poweshell in  DC-1 and run this script  src=" https://github.com/remyahk12/Active-Directory/blob/main/employeecreate "
 <p>Create a bunch of additional users </p>
</h2>
<p>
 <img src="https://github.com/user-attachments/assets/ec0744fc-288d-4012-81bc-aee649a21933" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
 <h2>
  <p>Attempt to log into client-1 with one of the users
</p>
 </h2>
 <p>
 <img src="https://github.com/user-attachments/assets/8dea33cc-d9fb-47be-a3de-d000f4f1677d" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 <img src="https://github.com/user-attachments/assets/8d5c4597-3318-472e-ae54-f4ac62b8c937" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
 
</h2>
<h1>
 <p>
  Configuring Group Policy
 </p>
</h1>
<h2>
 <p>
<h3>Dealing with Account Lockouts</h3>
Get logged into dc-1
Configure Group Policy to Lockout the account after 5 attempts:
 <p>
 <img src="https://github.com/user-attachments/assets/d2888503-2d09-4e11-85e9-40e258bea689" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
Pick a random user account created previously.(user account:gog.kub)


Attempt to log in with it 6 times with a bad password

Observe that the account has been locked out within Active Directory
<p>
 <img src="https://github.com/user-attachments/assets/46eef94f-892b-49f8-8cd8-c22186a63958" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
Unlock the account:Go to dc-1 .Find the account gog.kub .Under account information ,check the unlock account.This will unlock the account and can try login again.
<p>
 <img src="https://github.com/user-attachments/assets/b2664011-43ee-4a00-875e-e7d8774860f5" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h2>
 User can now successfully login with the correct password
<p>
 <img src="https://github.com/user-attachments/assets/a8c48dee-2ea6-4e6e-b2f1-c60d4fbd96c2" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

</h2>
<h1>
 <p>Network File Share and Permission</p>
</h1>
<h2>
Step1 : Create some sample file shares with various permissions
Connect/log into DC-1 as your domain admin account (mydomain.com\remyahk)
Connect/log into Client-1 as a normal user (mydomain\<someuser>)
On DC-1, on the C:\ drive, create 4 folders: “read-access”, “write-access”, “no-access”, “accounting”
Set the following permissions (share the folder)
Folder: “read-access”, Group: “Domain Users”, Permission: “Read”
Folder: “write-access”,  Group: “Domain Users”, Permissions: “Read/Write”
Folder: “no-access”, Group: “Domain Admins”, “Permissions: “Read/Write”
</h2>
<p>
<img src="https://github.com/user-attachments/assets/e7406274-9f18-4f56-8ccb-20b108327e68" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/user-attachments/assets/5aec0c84-889f-49ec-9acc-db69868a4425" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/user-attachments/assets/e64bbb37-8701-42ab-972b-922c2a78ade9" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h2>
 Attempt to access file shares as a normal user
On Client-1, navigate to the shared folder (start, run, \\dc-1)
<h2>Try to access the folders just created. </h2>
 <h3><p>
  Step1:Accessing readaccess folder allows the user to open the file and read  but no write permission
 </p>
<p>
<img src="https://github.com/user-attachments/assets/76c2a677-0630-4ed2-bf5d-5099b67c85e3" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</h3>
 <h3>
 <p>
 step2: Accessing writeaccess folder allows the user to open the file and read and write permission
 </p>
<p>
<img src="https://github.com/user-attachments/assets/c9c734ec-e036-43bf-8603-714895d5deae" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</h3>
<h3>
 <p>
  Accessing noaccess folder deny the user  permission
 </p>
<p>
<img src="https://github.com/user-attachments/assets/08dfb4c1-d355-4e8c-b570-6a680f045303" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</h3>

