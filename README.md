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

![image alt](https://github.com/brianknutson/ticket-lifecycle/blob/e2cf5ed9a6604198e07ad530fd47aefb6e2d5d5b/1.2.PNG)

Step 2
------
The next stage of a ticket lifecycle is the assignment and communication phase. In this phase, a help desk agent called John will assign the ticket to the appropriate department, so the ticket can be completed. To assign a ticket, he went to the webpage http://localhost/osTicket/scp/login.php. Then he logged in. 

Next, he went to "Tickets" and then clicked on the recent ticket created by Karen Doe. With some tickets, you may want to communicate with the person who created the ticket to get further insights as to what is going on and if the problem is persisting. In this example, the "SLA Plan" will be set to "Sev-A" because there is a wide impact on online banking, affecting most users. Once the info was filled in, he clicked on "Update Plan". 

Next, he assigned the ticket. He clicked on "— Unassigned —". The "Assignee" was to be "Online Banking" since the online banking system is affected. 

![image alt](https://github.com/brianknutson/ticket-lifecycle/blob/e2cf5ed9a6604198e07ad530fd47aefb6e2d5d5b/2.1.PNG)

![image alt](https://github.com/brianknutson/ticket-lifecycle/blob/e2cf5ed9a6604198e07ad530fd47aefb6e2d5d5b/2.2.PNG)

![image alt](https://github.com/brianknutson/ticket-lifecycle/blob/e2cf5ed9a6604198e07ad530fd47aefb6e2d5d5b/2.3.PNG)

Step 3
------
The next stage of a ticket lifecycle is working on the issue. At this stage, Jane Doe, who is a part of the online banking team, worked through this ticket. Like before, she went to "Tickets" and then clicked on the recent ticket created by Karen Doe. 

Jane decides to work on this ticket herself, so she clicks on the "Online Banking" next to the "Assigned To". Then she assigns the ticket to herself. With osTicket, you can make updates with other agents through "Post a Reply".

Karen believes the issue could stem from the recent updates to the online banking system, so she posted a reply relaying this belief. After working through the issue, Karen determined that the root cause of the issue was the recent update to the online banking system. She rolled back the update, getting the online banking system up and running. Then she notified the vendor. Karen made another post explaining this situation. 

![image alt](https://github.com/brianknutson/ticket-lifecycle/blob/e2cf5ed9a6604198e07ad530fd47aefb6e2d5d5b/3.1.PNG)

![image alt](https://github.com/brianknutson/ticket-lifecycle/blob/e2cf5ed9a6604198e07ad530fd47aefb6e2d5d5b/3.2.PNG)

![image alt](https://github.com/brianknutson/ticket-lifecycle/blob/e2cf5ed9a6604198e07ad530fd47aefb6e2d5d5b/3.3.PNG)
