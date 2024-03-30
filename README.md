<p align="center">
<img alt="1" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/13675a50-b000-4889-954c-9acb68e54626">

</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure) (Part 1)</h1>


<p>The tutorials, which this section is the first of, outline the implementation of on-premises Active Directory within Azure Virtual Machines.
</p>

<h2>Overview </h2>

<p>To start this first part of the tutorial, two virtual machines will be interconnected and configured, each taking different roles. The first virtual machine will be purposed as the Domain Controller. The second virtual machine will be purposed as the Client.</p>

<h2>Main Objectives</h2>
<h3>Virtual Machine Setup</h3>

-  Setup the Domain Controller virtual machine
-  Ground the Client virtual machine

<h3>Remote Connectivity</h3>

- Create a connection using the Remote Desktop connection service

<h3> Traffic Inspection</h3>

- Do a basic inspection of the network traffic between the Domain Controller and Client VMs.



<h2>Environments and Technologies Utilized</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Utilized </h2>

- Windows Server 2022
- Windows 10 (22H2)


<h2>Steps for Configuration</h2>

<h3>&#9312; Create the Domain Controller</h3>

- Create a virtual machine on Azure.
- Name it DC-01 
- Choose Windows Server 2022: Azure Edition - x64 Gen2 as the operating system image


<p>
<img width="776" alt="2" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/79cd61be-f939-4778-ad89-0ddc4420f66a">

</p>
<p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<strong> NOTE: Make sure to choose at least 2 vcpus and 16 GiB memory and notice the vnet that the VM has created.</strong>
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

<h3>&#9313; Configure the Domain Controller's Private IP to static </h3>

-  Once the VM has been deployed, continue to the VM overview page and select "Networking" on the left side. 
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

<h3>&#9314; Produce the client VM </h3>

- As done before, produce a new VM and we'll name it Client-01. We'll choose Windows 10 as the operating system image and be certain to choose at least 2 vcpus and 16 GiB memory.
<img width="717" alt="6" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/9ff4e593-7a96-4077-903b-c50a93942854">

<br>
<br>
<br>

<p><strong> NOTE: Be certain to choose the same resource group and vnet from the DC-01 VM </strong></p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>

<img width="731" alt="7" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/f658dc3f-8f9a-4cf4-9638-727266a49a60">\

<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

- Now complete everything and wait for its deployment.

<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<h3>&#9315; Making sure the connectivity between Domain Controller and Client is successful  </h3>

<p>To make sure connectivity between the two VMs is successful, ping the domain controller from the client.</p>

- First login to the Client-01 utilizing it's public ip address and creating a Remote Desktop connection

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

<p><strong> Locate DC-01's private ip address in the Azure Portal and copy it. Continue to Client-01 and open the terminal and type "ping -t (DC-01 private ip address)" </strong></p>


<img width="668" alt="10" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/09cc95ec-0028-47c3-8e98-2b3da2b098e1">



<br>
<br>

<p> <strong> Now notice how the request timed out, this is because ICMP v4 traffic is blocked by default on DC-01's firewall. So enable inbound ICMP traffic to allow for Client-01's ping.</strong> </p>

<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p><strong> Login to DC-01 using the Remote Desktop connection service and open Windows Defender Firewall and choose advanced settings. Sort by protocol and locate both ICMP echo requests and enable both these rules by right-clicking and choosing enable rule.</strong></p>

<be>

<img alt="11" width="668" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/5959256f-5c62-4650-9f09-e0d4d1df0aba">

<br>

<p><strong>.</strong></p>
<p><strong>.</strong></p>
<p><strong>.</strong></p>

<p><strong> After the traffic has been enabled, check back with Client-01 and notice that the ping is now successful.</strong> </p>

<img width="334" alt="12" src="https://github.com/giovannibriones/ad-and-azuresetup/assets/163789590/6f64e28f-9e05-420e-8132-f6cdeea09d4a">

<h2> In Conclusion </h2>

<p> We've completed the first part of the outlining of the implementation of on-premises Active Directory within Azure Virtual Machines tutorial. By setting up two virtual machines, the groundwork has been laid for next parts of the tutorial. In this part, the focus was on creating a Domain Controller and a Client machine, enabling remote access, and briefly explore network traffic between them. In the next parts of the tutorial, this foundation will help create more advanced configurations and practical situations in Azure and Active Directory. </p>











