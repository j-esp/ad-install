![image](https://github.com/user-attachments/assets/c6575b2d-c357-4886-bae2-08ea505b3eed)![image](https://github.com/user-attachments/assets/2ca770bf-8a01-4af1-b09a-81fc89196c1b)

  <h1>Active Directory Installation in Azure</h1>
This document outlines the steps taken to install Active Directory in Azure. The setup involves two VMs within the same virtual network: one VM will be configured as the domain controller with Active Directory, while the other will serve as the client for the next activity. <br />

<h2>Technologies and Platforms</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Connection
- Active Directory Domain Services

<h2>Operating Systems</h2>

- Windows 10 Pro (22H2)
- Windows Server 2022

<h2>Installation Steps</h2>

![image](https://github.com/user-attachments/assets/134f6ad2-51a2-4d95-bd08-f48bcdb0ec68)
<p>
Before connecting to the VMs, it is crucial to assign a static IP address to the domain controller VM. By default, VMs with dynamic IPs cannot communicate with each other, even if they are on the same virtual network. Failure to set a static IP could prevent the client from joining the domain that will be created.

In the Azure portal, go to the domain controller VM's Networking tab. Click on the Network Interface and open IP configurations. Change the Allocation setting from dynamic to static and save the changes. This ensures that the domain controller's IP address remains the same.
</p>

![image](https://github.com/user-attachments/assets/33d7905f-a266-4587-9bf0-c0a397a22f8d)

![image](https://github.com/user-attachments/assets/485cd632-3f67-4f66-b983-d66af23ef10e)

![image](https://github.com/user-attachments/assets/b2986cc3-ee7e-4b32-a63a-6f28c181df94)
<p>
After configuring the static IP, log in to the client VM and test connectivity to the domain controller using the command ping <domain controller IP address> -t. Since the connection times out, it is imperative we enable ICMPv4 on the domain controller VM's firewall. Open Windows Defender Firewall by typing wf.msc in the search bar, navigate to Inbound Rules, and enable the Core Networking Diagnostics - ICMP Echo Request rules. Once these settings are applied, we can confirm that the ping command resolves without errors back on the client VM. 
</p>
  
![image](https://github.com/user-attachments/assets/185c9667-1d84-4e72-a50a-2d2b57d66a03)
![image](https://github.com/user-attachments/assets/25a9f828-c167-4af8-ae16-711f93b59c38)

To configure the server, begin by opening Server Manager and selecting "Add Roles and Features," then click "Next." Ensure you verify the private IP address of the domain controller's VM. Proceed to the "Server Roles" tab, choose "Active Directory Domain Services," click "Next," and complete the installation. Once the installation is finished, you need to promote the server to a domain controller. In Server Manager, click the warning icon under the flag to start the promotion process. Select "Add a new forest" and specify your domain name, which in my case is conjuring.com. Following these steps will set up and promote the server as a domain controller. 
</p>
<br />
<h2>Critical Note </h2>

When logging into the domain controller's VM, ensure you use the domain context. Enter the domain path followed by the user name, for example, conjuring.com\labuser. The next step will involve configuring settings to allow the client VM to join the newly created domain. 
