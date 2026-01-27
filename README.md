<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This demonstration outlines the implementation of on-premises Active Directory within Azure Virtual Machines. Moreover, it will show how group policy is used to manage accounts.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell
- Group Policy

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 11 Pro (25H2) 

<h2>High-Level Deployment and Configuration Steps</h2>

- Preparing AD Infrastructure in Azure 
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

Step 1
------
Microsoft Azure will be used in this demonstration to implement on-premises Active Directory. A resource group and vnet were created called Active-Directory-Lab and Active-Directory-Lab respectively. Two virtual machines (VM) were also created. One was the Windows Server 2022, which is called dc-1, and it's the domain controller. The other system was Windows 11 Pro version 25H2 called client-1.

![image alt](https://github.com/brianknutson/configure-ad/blob/119d2333ade4857384b29fa4686e7646e32336cc/Step1.0.png)

Step 2
------
By default, the VM will get a private and a public remote. The private remote will be dynamic, which means the IP address can change. For example, if this VM gets turned off and another VM is created, the new VM might take over dc-1’s old IP address. This would be typically fine; however, dc-1 will be acting as a DNS server. If dc-1’s IP address changes, the settings for client-1 will no longer be valid. Therefore, I will change dc-1 IP address to be static, so the IP address doesn’t change. 

To make the dc-1 IP address static, I went to the IP configurations of dc-1 and clicked on “Static” for the “Allocation” option. Then I pressed “Save”. 

![image alt](https://github.com/brianknutson/configure-ad/blob/1d35e42683036d8f2de9345fd1ae00be2369b714/Step2.png)

Step 3
------
To make it easier to test connectivity and general testing for this lab, I disabled Windows Firewall for dc-1.

To disable Windows Firewall for dc-1, I used Remote Desktop to connect to dc-1. I went to Windows Defender Firewall then clicked on "Windows Defender Firewall Properties. Under the "Domain Profile", "Private Profile", and "Public Profile", I turned off the Firewall state. I pressed "Apply" then "Ok". 

![image alt](https://github.com/brianknutson/configure-ad/blob/5a5b947bb9a6f221d2d23013ad57d586b2539312/Step2.0.png)

Step 4
------
Client-1’s DNS settings are set to Microsoft Azure; however, for this lab, I want Client-1’s DNS settings to be set to dc-1 for testing purposes.

The first thing I need is dc-1’s private IP address. I'll copy the private IP to paste it later.  

![image alt](https://github.com/brianknutson/configure-ad/blob/a508efae321f08d0de28fed3ecd24ae79b081102/Step3.0.png)

Going to the DNS server settings for Client-1, I pressed "Custom" setting under DNS servers, so I can redirect the DNS server to dc-1 instead of Microsoft Azure. Then I pasted dc-1's private IP address to the DNS server making the connection from Client-1 to dc-1. 

![image alt](https://github.com/brianknutson/configure-ad/blob/905a5ad4313e09cc02e79f36d95dce475c08cfa0/Step3.1.png)
