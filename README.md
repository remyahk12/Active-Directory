 configure-ad
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
<h1>🚀 Azure Domain Controller & Client Deployment</h1
<p>This guide provides step-by-step instructions to deploy a <b>Windows Server 2022</b> as a Domain Controller (DC) and a <b>Windows 10</b> Client VM in Microsoft Azure.</p>

    
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
 <pre><code>
$nic = (az network nic list --resource-group RG-DomainController --query "[?contains(name,'dc-1')].name" --output tsv)
az network nic ip-config update --resource-group RG-DomainController --nic-name $nic --name ipconfig1 --private-ip-address 10.0.0.4
        </code></pre>
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
