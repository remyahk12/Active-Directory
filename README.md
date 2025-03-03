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

<h2>📌 Overview</h2>
<ul>
 <li>Create an <b>Azure Resource Group</b></li>
 <li>Set up a <b>Virtual Network and Subnet</b></li>
 <li>Deploy a <b>Windows Server 2022 VM</b> as a Domain Controller</li>
 <li>Deploy a <b>Windows 10 VM</b> as a Client</li>
 <li>Configure DNS settings for Client-1 to use DC-1</li>
 <li>Verify network connectivity between the VMs</li>
 </ul>
 

   
 <h2>🛠 Prerequisites</h2>
 <ul>
 <li>An <b>active Azure subscription</b></li>
 <li><b>Azure CLI</b> or <b>Azure PowerShell</b> installed</li>
 <li><b>RDP client</b> for remote access</li>
 </ul>
  <div class="section">
  <h2>🔧 Deployment Steps</h2>

  <h3>1️⃣ Create a Resource Group</h3>
  <pre><code>az group create --name RG-DomainController --location eastus</code></pre>

  <h3>2️⃣ Create a Virtual Network and Subnet</h3>
  <pre><code>az network vnet create --name VNet-DC --resource-group RG-DomainController --location eastus --address-prefix 10.0.0.0/16 --subnet-name Subnet-DC --subnet-prefix 10.0.0.0/24</code></pre>

 <h3>3️⃣ Deploy the Domain Controller VM</h3>
  <pre><code>az vm create --resource-group RG-DomainController --name DC-1 --image Win2022Datacenter --size Standard_D2s_v3 --admin-username labuser --admin-password "Cyberlab123!" --vnet-name VNet-DC --subnet Subnet-DC --nsg "" --public-ip-address ""</code></pre>

<h3>4️⃣ Assign Static Private IP to DC-1</h3>
<p>
<img src="https://github.com/user-attachments/assets/7c15be03-da43-4dda-9088-59e67058afe9" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

 <h3>5️⃣ Disable Windows Firewall on DC-1 (For Testing)</h3>
 <p>After connecting to the VM via <b>RDP</b>, open <b>PowerShell as Administrator</b> and run:</p>
 <pre><code>Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False</code></pre>
 </div>

<div class="section">
<h2>💻 Deploy Client-1 VM</h2>

 <h3>6️⃣ Deploy Windows 10 VM (Client-1)</h3>
<pre><code>az vm create --resource-group RG-DomainController --name Client-1 --image Win10 --size Standard_B2ms --admin-username labuser --admin-password "Cyberlab123!" --vnet-name VNet-DC --subnet Subnet-DC --nsg "" --public-ip-address ""</code></pre>

 <h3>7️⃣ Configure Client-1 DNS Settings to DC-1</h3>
 <pre><code>
$nicClient = (az network nic list --resource-group RG-DomainController --query "[?contains(name,'client-1')].name" --output tsv)
az network nic update --resource-group RG-DomainController --name $nicClient --dns-servers 10.0.0.4
        </code></pre>

 <h3>8️⃣ Restart Client-1 from Azure Portal</h3>
 <p>After updating DNS settings, restart the VM to apply changes.</p>

 <h3>9️⃣ Verify Network Connectivity</h3>
 <ul>
  <li>Log in to Client-1 via RDP</li>
  <li>Open **Command Prompt** and run:</li>
  </ul>
  <pre><code>ping 10.0.0.4</code></pre>
 <p>✅ <b>Ensure the ping succeeds</b></p>

 <h3>🔟 Verify DNS Configuration on Client-1</h3>
  <ul>
  <li>Open **PowerShell** on Client-1 and run:</li>
        </ul>
        <pre><code>ipconfig /all</code></pre>
        <p>✅ The output should show **DC-1's private IP (10.0.0.4) as the DNS server**</p>
    </div>
 <div class="section">
        <h2>📂 Project Structure</h2>
        <pre>

    </div>

 <div class="section">
 <h2>🚀 Next Steps</h2>
  <ul>
  <li>Install <b>Active Directory Domain Services (AD DS)</b> on DC-1</li>
  <li>Promote DC-1 as a <b>Domain Controller</b></li>
  <li>Join Client-1 to the <b>Active Directory Domain</b></li>
  <li>Configure <b>Group Policies</b> and user authentication</li>
  </ul>
  </div>




<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
