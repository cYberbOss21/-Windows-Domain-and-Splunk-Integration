# Windows-Domain-and-Splunk-Integration

This project involves setting up an Active Directory domain environment with example.local, configuring users, groups, and organizational units, and integrating Splunk Enterprise for centralized log monitoring and analysis. The Splunk Universal Forwarder was installed on the domain controller to forward Windows Event Logs, enabling real-time visibility into system and security events. This setup demonstrates the foundational steps required for creating a secure and monitored Windows domain environment, providing the basis for future audits and alerting.

________________________________________
**Setting Up Active Directory Domain Services (AD DS)** 

**Install AD DS Role on Windows Server**
1. Open **Server Manager**.

 ![image](https://github.com/user-attachments/assets/ce75e03f-d6cb-45f7-8e15-eaea843a1cb9)

2.	Click on **Add Roles and Features**.
 
  ![image](https://github.com/user-attachments/assets/efc24b7d-b8ab-4e71-b7d8-f585409bbcbb)
  
![image](https://github.com/user-attachments/assets/c61d69f2-e028-4694-a729-e61fa27e5874)


3.	Select **Role-based or feature-based installation** and click **Next**.
   
 ![image](https://github.com/user-attachments/assets/8a7a5515-a325-4f89-ba4f-720274de2752)


4.	Select the server to install roles on and click **Next**.
5.	Choose **Active Directory Domain Services** and click **Add Features** when prompted.

   
 ![image](https://github.com/user-attachments/assets/2c3e037d-a793-44f5-a9f6-24b575a06b09)


6.	Proceed through the wizard, leaving defaults selected, and click **Install**.
7.	After installation, click on **Promote this server to a domain controller.**
    
 ![image](https://github.com/user-attachments/assets/85e81a6c-2ea6-48cf-a91b-5160114eed15)


Note: You will have to restart the server 
________________________________________

**Promote Server to Domain Controller**
1.	Choose **Add a new forest** and enter example.local as the root domain name.

   
  ![image](https://github.com/user-attachments/assets/0de97d36-4b86-42f7-b37d-3d1fb5374fcb)

 
2.	Set the **Domain Functional Level** and **Forest Functional Level** to Windows Server 2016.
3.	Check **Domain Name System (DNS) server** and set a Directory Services Restore Mode (DSRM) password.

   
 ![image](https://github.com/user-attachments/assets/cabf575a-b41c-4062-9706-fd884da6f76f)


4.	Proceed through the wizard, ensuring NetBIOS domain name is set to EXAMPLE.
5.	Complete the prerequisites check and click **Install**. The server will restart automatically.
________________________________________
**Configuring DNS** 
1.	Open **DNS Manager** from the **Tools** menu in Server Manager.
2.	Verify that a forward lookup zone for example.local exists.
3.	Create a reverse lookup zone:
 - Right-click Reverse Lookup Zones > New Zone.
 - Follow the wizard to create a primary zone for your network (e.g., 192.168.10.x).
________________________________________
**Adding Users and Groups** 
**Create Organizational Units (OUs)** 
1.	Open **Active Directory Users and Computers**.
2.	Right-click the domain (example.local) > **New** > **Organizational Unit.**

   
 ![image](https://github.com/user-attachments/assets/61b66941-a5dc-49c9-8747-8d866a35d562)


3.	Name the OU (e.g., IT, HR, Admins).

   
 ![image](https://github.com/user-attachments/assets/78ea86f3-1e66-4305-9f6b-8271cb0af6bd)
________________________________________


**Create Users** 
1.	Right-click the appropriate OU > **New** > **User**.

 
 ![image](https://github.com/user-attachments/assets/352bfa9c-a529-4089-a42a-267f4ef4f0ca)


![image](https://github.com/user-attachments/assets/030fda47-30ee-4831-b2b9-ac6a2a4a56d3)

 
2.	Fill in user details (e.g., Bob, username bob).
3.	Set a password and configure options such as "User must change password at next logon," or the preference you prefer.
________________________________________

**Create Groups** 
1.	Right-click the appropriate OU > New > Group.
2.	Enter a group name (e.g., IT_Admins) and select the scope (Global) and type (Security).
 
 
 ![image](https://github.com/user-attachments/assets/39dbb13a-ee3a-4304-8187-a8cfc6c3b1c8)


3.	Add users to the group:
- Right-click the group > **Properties** > **Members** > **Add**.

 ![image](https://github.com/user-attachments/assets/0401b991-f270-49f2-9b14-c3e08c830056)

________________________________________
**Configuring Windows 10 to Join the Domain** 

**Rename the Computer**

1.  On the Windows 10 machine, open **System Properties** > **Change Settings** > **Change**.
2.	Set the computer name (e.g., Workstation01) and click **OK**.
3.	Restart the machine when prompted.
**Join the Domain**
1.	Open **System Properties** > **Change Settings** > **Change**.
2.	Select **Domain** and enter example.local.
3.	Provide credentials for a domain admin user when prompted.
4.	Restart the machine.
5.	Log in as the domain user (e.g., example\bob).
   
 ![image](https://github.com/user-attachments/assets/e0b10cb6-fb13-4f0d-b9f8-b1d1026725e9)

 
________________________________________
**Troubleshooting Issues**

_**DNS Delegation Warning**_

Ignored as this was a standalone setup.

_**Cannot Ping Between Machines**_

Ensured all machines were on the same subnet (xxx.xxx.xx.x).
   
   
   ![image](https://github.com/user-attachments/assets/83dd3b7e-457d-4114-a56c-1710fa86388a) 
   
   
   ![image](https://github.com/user-attachments/assets/c2d417b0-0794-421d-906e-018e5dc6fb47)


Note: You may have to manually add the IP of the server in the Windows 10 VM

• Verified NAT Network in VirtualBox was correctly configured.

•Allowed ICMP traffic through Windows Firewall.
________________________________________

_**User Login Issues**_

Confirmed correct domain name and username format (example\username).

Verified that the DNS server IP on the client machine was pointing to the domain controller.


![image](https://github.com/user-attachments/assets/7445fa07-6f6b-45c4-9fdd-5ec690f6bdc0)

________________________________________
**Installing Splunk Enterprise on Kali Linux**

**Download and Install**

1.	Download Splunk Enterprise from Splunk's official website.

  	 - You will have to create a account and download Splunk Enterprise 
           with a 14-day trial
      
2.	On Kali Linux, run:
   
      	sudo dpkg -i splunk-9.3.2-d8bb32809498-linux-2.6-amd64.deb
        sudo /opt/splunk/bin/splunk start --accept-license

   
 ![image](https://github.com/user-attachments/assets/63f953d8-b2df-4744-86da-a458d6eba21a)

You will be prompted to create an admin profile

  
 ![image](https://github.com/user-attachments/assets/db6b90d8-4552-4c4b-80b5-682a4c3a71d8)


3.	Access Splunk Web at http://kali:8000.
________________________________________

**Configure Ports on Kali**

Ensure that the Splunk ports are open on the Kali Linux firewall:

_**Web Interface:** 8000_

_**Deployment Server:** 8089_

_**Forwarding Port:** 9997_


4. To allow these ports:

Example:

        sudo iptables -A INPUT -p tcp --dport 9997 -j ACCEPT
________________________________________
  	
**Create Index for Windows Logs**
1.	In Splunk Web, go to **Settings** > **Indexes** > **New Index**.

   
     ![image](https://github.com/user-attachments/assets/5cb84385-ba82-4563-96ad-107de51b0b3b)


2.	Name the index **windows_audit** and save.

  ![image](https://github.com/user-attachments/assets/c3e79305-a89e-4b49-a9f8-dc51844f2494)
  
      
![image](https://github.com/user-attachments/assets/6a89b77a-6538-47b3-99d8-77c1bd2d7671)


________________________________________
**Installing Splunk Universal Forwarder on Windows Server**

**Download and Install**
1.	Download Splunk Universal Forwarder from Splunk's official website.
   
     ![image](https://github.com/user-attachments/assets/0f9ff9e8-0b0b-4d40-9632-fa6405569654)


2.	Run the installer and choose "On-premises Splunk Enterprise instance."
3.	Set the deployment server to 192.168.10.4:9997 (Splunk Enterprise server IP).
________________________________________

**Configure Forwarder**

_Navigate to:_

	-C:\Program Files\SplunkUniversalForwarder\etc\system\local

_Edit outputs.conf:_

	 [tcpout] defaultGroup = default-autolb-group

	 [tcpout:default-autolb-group] server = 192.168.10.4:9997

_Edit inputs.conf to collect Windows logs:_

	 [WinEventLog://Application]
 	 index = windows_audit
  	 disabled = 0

 	[WinEventLog://Security]
	index = windows_audit
	disabled = 0

	[WinEventLog://System]
	index = windows_audit
	disabled = 0

_Restart the forwarder:_
	
 	.\splunk restart
   
![image](https://github.com/user-attachments/assets/f71f2ff2-9ff5-412d-b301-ce621b2b47df)

**Verifying Data in Splunk**
1.	Log in to Splunk Web on Kali Linux.
2.	Go to **Search & Reporting.**

  	 ![image](https://github.com/user-attachments/assets/219394cd-3459-4f25-a2e5-6ff644c80dc4)

3.	Run the following search to see logs:
   **index=windows_audit**

  	 ![image](https://github.com/user-attachments/assets/0006e758-167a-4930-9c3e-0550bd06f287)

4. If no logs appear, check:
-Forwarder logs on the Windows machine.
-Firewall and connectivity settings.
________________________________________
**Creating Alerts and Dashboards**
**Create an Alert**
1.	In Splunk Web, go to **Search & Reporting.**
2.	Run a query for failed logins:
   	
    	index=windows_audit EventCode=4625 | stats count AS failed_attempts

4.	Click **Save As > Alert.**
5.	Configure alert conditions (e.g., more than 5 failed logins in 15 minutes).
6.	Set actions (e.g., send an email).
 
  ![image](https://github.com/user-attachments/assets/5cbcc9ca-62cd-48df-8493-1610d5c0c5f7)
  
  ![image](https://github.com/user-attachments/assets/b9cd075f-6b5a-489a-b141-968909030720)

![image](https://github.com/user-attachments/assets/5ff6fc1f-7621-40ea-904a-4b2d5fa99e4c)


 
**Create a Dashboard**
1.	In Splunk Web, go to **Dashboards** > **Create New Dashboard.**
2.	Add panels for:

-Login attempts over time.

-Top users by login failures.

 -System errors.
 
3.	Save and customize the dashboard layout.
