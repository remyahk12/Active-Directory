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
