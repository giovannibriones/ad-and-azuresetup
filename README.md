<p align="center">
<img alt="1" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/13675a50-b000-4889-954c-9acb68e54626">

</p>

<h1>Building the Foundation: Preliminary Setup for Active Directory and Network Traffic Analysis between Azure VMs</h1>


<p>Welcome to the inaugural project in a comprehensive series of tutorials focused on Azure and Active Directory implementation. This initial project serves as the foundational cornerstone and setup for the subsequent  parts of this tutorial series. The primary objective is to lay the groundwork for a simple lab environment on Azure to simulate the environment in which Active Directory is employed within an enterprise setting.
</p>

<h2>Overview </h2>

<p>In this first project, I will configure and interconnect two virtual machines, each assuming distinct roles. The first virtual machine will be designated as the Domain Controller. The second virtual machine will be configured as the Client.</p>

<h2>Key Objectives</h2>
<h3>Virtual Machine Setup</h3>

-  Configure the Domain Controller virtual machine
-  Establish the Client virtual machine

<h3>Remote Connectivity</h3>

- Enable a connection using remote desktop connection

<h3> Traffic Inspection</h3>

- Undertake a basic inspection of the network traffic between the Domain Controller and Client virtual machines.



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)


<h2>Configuration Steps</h2>

<h3>&#9312; Create the Domain Controller</h3>

- Create a virtual machine on Azure.
- Name it DC-01 
- Select Windows Server 2022: Azure Edition - x64 Gen2 as the image


<p>
<img width="776" alt="2" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/79cd61be-f939-4778-ad89-0ddc4420f66a">

</p>
<p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<strong> NOTE: Make sure to select at least 2 vcpus and 16 GiB memory and take note of the vnet that the VM has created.</strong>
</p>
<br />

<p>
<img width="736" alt="3" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/6fa6b849-b25e-4ad2-8f4a-5b44f69b4142">

</p>
<p>
</p>
<br />
<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<h3>&#9313; Set the Domain Controller's Private IP to static </h3>

-  Once the VM has been deployed, proceed to the VM overview page and select "Networking" on the left side. 
<img width="692" alt="4" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/1e30e594-c8f1-4e28-b99a-bfd414cdffae">

<br>
<br>
<br>

-  Select Network Interface Card -> IP configurations -> ipconfig1 and set Private IP address allocation to static.

<br>

<p>
<img width="518" alt="5" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/dbc4ae8d-4955-4160-b5d8-d302ea3470b3">

</p>

<br />
<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<h3>&#9314; Create the client VM </h3>

- Once again create a new VM and we'll name it Client-01. We'll select Windows 10 as the image and make sure to select at least 2 vcpus and 16 GiB memory.
<img width="717" alt="6" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/9ff4e593-7a96-4077-903b-c50a93942854">

<br>
<br>
<br>

<p><strong> NOTE: Make sure to select the same resource group and vnet from the DC-01 VM </strong></p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<img width="731" alt="7" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/f658dc3f-8f9a-4cf4-9638-727266a49a60">\

<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

- Now finalize everything and wait for its deployment.

<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<h3>&#9315; Ensure connectivity between Domain Controller and Client  </h3>

<p>To ensure connectivity between the two VM's, we will ping the domain controller from the client.</p>

- First login to the Client-01 using it's public ip address and remote desktop

<img width="993" alt="8" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/34448518-8b9e-41cc-a201-e24345e7a7c7">



<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<img width="297" alt="9" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/29c0fa87-edef-40e5-b747-9556536f155c">


<br>
<br>
<br>
<br>
<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p><strong> Find DC-01's private ip address in the Azure Portal and copy it. Proceed to Client-01 and open the terminal and type "ping -t (DC-01 private ip address)" </strong></p>


<img width="668" alt="10" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/09cc95ec-0028-47c3-8e98-2b3da2b098e1">



<br>
<br>

<p> <strong> Now notice how the request timed out, this is because ICMP v4 traffic is blocked by default on DC-01's firewall. So we will have to enable inbound ICMP traffic to allow for Client-01's ping.</strong> </p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p><strong> Login to DC-01 using remote desktop and open windows defender firewall and select advanced settings. Sort by protocol and find both ICMP echo requests and enable both these rules by right clicking and selecting enable rule.</strong></p>

<be>

<img alt="11" width="668" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/5959256f-5c62-4650-9f09-e0d4d1df0aba">

<br>

<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p><strong> Now once the traffic has been enabled, you can check back with Client-01 and notice that the ping is now successful.</strong> </p>

<img width="334" alt="12" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/6f64e28f-9e05-420e-8132-f6cdeea09d4a">

<h2> In Conclusion </h2>

<p> We've completed the foundational setup for our Azure and Active Directory project series. By configuring two virtual machines, we've laid the groundwork for implementing the subsequent set of projects. In this project, we focused on establishing a Domain Controller and a Client machine, enabling remote access, and briefly examining network traffic between them. Moving forward, this foundation will help implement more advanced configurations and practical scenarios in Azure and Active Directory. </p>











