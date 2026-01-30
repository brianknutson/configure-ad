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
- Deploying Active Directory
- Creating Users with PowerShell
- Group Policy and Managing Accounts 

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

Step 5
------
I want to ensure that Client-1 can connect to DC-1. 

To ensure this, I used Remote Desktop to connect to Client-1. Then I opened PowerShell and used the “ping” command to the dc-1 private IP address. 

I know the connection was successful because there isn't a "Destination host unreachable" and all Packets were recevied. 

![image alt](https://github.com/brianknutson/configure-ad/blob/d6f18aff624abdfe4c835ea33b2835a365d9d708/Step4.png)

Step 6
------
Next, I want to ensure that Client-1's DNS settings is set for dc-1. 

To check this, I used PowerShell in Client-1 and used the "ipconfig /all" command. I found the DNS Servers and saw that dc-1's private IP address was used, meaning that Client-1's DNS settigns is set for dc-1. 

![image alt](https://github.com/brianknutson/configure-ad/blob/a24171bca56a2a4239f4c5b15c43551a665eae1e/Step4.1.png)

<h2>Deployment and Configuration Steps</h2>

Step 1
------
Since I'm done with preparing active directory infrastructure in Azure, it's time to deploy active directory. First, I need to install it, so I logined into dc-1 and to install Active Directory Domain Services. 

To install Active Directory Domain Services, I went into the Server Manager and clicked on "Add roles and features". Once I got to "Server Roles", I clicked on "Active Domain Services to install it. 

![image alt](https://github.com/brianknutson/configure-ad/blob/1ed3fd04ed0104ca332cdf8b4d937537daaf56db/Step5.0.png)

I continued to press "Next" until I got to the "Confirmation" page where I again clicked on "Install". 

![image alt](https://github.com/brianknutson/configure-ad/blob/1ed3fd04ed0104ca332cdf8b4d937537daaf56db/Step5.1.png)

Step 2
------
Now that I installed Active Directory Domain Services, it's time to promote dc-1 as a domain controller(DC). 

To achieve this, I clicked on the flag icon then pressed "Promote this server to a domain controller". 

![image alt](https://github.com/brianknutson/configure-ad/blob/48f155992f9319f587951d28e32a87db5effbf8d/Step6.0.png)

Next, I clicked on "Add a new forest". Then named my Root domain name as "mydomain.com". 

![image alt](https://github.com/brianknutson/configure-ad/blob/48f155992f9319f587951d28e32a87db5effbf8d/Step6.1.png)

I unchecked "Create DNS delegation". 

![image alt](https://github.com/brianknutson/configure-ad/blob/48f155992f9319f587951d28e32a87db5effbf8d/Step6.2.png)

Finally, within "Prerequisties Check", I clicked on "Install" to make dc-1 as a domain controller. 

![image alt](https://github.com/brianknutson/configure-ad/blob/48f155992f9319f587951d28e32a87db5effbf8d/Step6.3.png)

Step 3
------
Because dc-1 restarted after the installation. I need to log back in. However, since dc-1 is a domain controller now, I need to specify the context to which I want to login as either as a local user or as a domain account. In this demonstration because I want to work on the domain controller, I will log in as mydomain.com\labuser

![image alt](https://github.com/brianknutson/configure-ad/blob/04f27efb98c0f30f0c74956732151eb61d02298e/Step7.0.png)

Step 4
------
Now I need to create an admin, but before I do that, I will create two organizational units: _EMPLOYEES and _ADMINS. I put an underscore to differentiate them from other groups. 

To do this, I went to “Active Directory Users and Computers”, right-clicked on “mydomain.com”, clicked on " New ", then “Organizational Unit”.

![image alt](https://github.com/brianknutson/configure-ad/blob/4282c5e9c76b5334bef4ee4936ad35db1c368eda/8.0.png)

For the name, I entered _EMPLOYEES. After I went through the same process for the next organizational unit: _ADMINS. 

![image alt](https://github.com/brianknutson/configure-ad/blob/4282c5e9c76b5334bef4ee4936ad35db1c368eda/8.1.png)

For the next part, I will create a new employee named “Jane Doe” within the _ADMINS folder. 

I right-clicked on _ADMINS, hovered over “New”, then finally clicked on “Users”. After I filled in the necessary information. 

![image alt](https://github.com/brianknutson/configure-ad/blob/4282c5e9c76b5334bef4ee4936ad35db1c368eda/8.2.png)

Even though jane_admin has admin in the name, Jane is not an admin yet. I need to add her to the “Domain Admins” Security Group. 

I went to the properties of jane_admin, clicked on “Member Of”, and then “Add”. I typed in “Domain Admins” and then clicked on “Check Names” to ensure I had Domain Admins. Finally, I pressed “Ok”, “Apply”, and “Ok”.

For the remainder of the lab, I will use jane_admin as my admin account.  

![image alt](https://github.com/brianknutson/configure-ad/blob/4282c5e9c76b5334bef4ee4936ad35db1c368eda/8.3.png)
![image alt]()
