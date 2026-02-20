# Active-Directory
# Windows Server 2016 AD, DNS, DHCP LAB with Windows 10 Client
Welcome to my GitHub repository, where I document step-by-step setup of Active Directory on Windows Server 2016 and Windows 10.
# Project Overview
This project shows a full lab setup of a Windows Server 2016 domain controller with Windows 10 client, including:
- Installing Windows Server 2016 and Windows 10
- Renaming server
- Setting up static IP address
- Installing Active Directory Domain Services (AD DS), DNS, DHCP
- Promoting server to Domain Controller
- Configuring DNS reverse lookup zone
- Configuring DHCP scope
- Joining Windows 10 to the domain
- Creating Organizational Units, Users, and Security Groups
 # Step 1: Download & Mount ISOs
- Download Windows Server 2016 ISO and Windows 10 ISO.
- Mount the ISO in VirtualBox (or your virtualization platform)

# Step 2: Installing Windows Server 2016 & Windows 10
Installing Server
- Start the Server 2016 virtual machine.
- Select language to install, time & currency format, keyboard input → Click Next.
- Click Install Now to start installation.
- Select Windows Server 2016 Standard Evaluation – Desktop Experience.
- Accept license terms → Click Next.
- Select Custom Install.
- Set up drive → Click Next.
- Type password twice → Click Finish.
- Log in as Administrator.

Installing Windows 10
- Start the Windows 10 virtual machine.
- Follow the same process as the server.
- Select Windows 10 Pro when prompted.
- Complete installation and initial setup.
  
# Step 3: Renaming the Server
	- In Server Manager → Click Local Server.
	- By default, the server is named WIN + random letters/numbers.
	- Click Change → Specify the name you want → Restart to apply changes.

# Step 4: Setting Up Static IP Address

Note: Required if this server will be a Domain Controller.

	1.	Log in as Administrator.
	2.	Click Start → Control Panel → Network and Internet or open command prompt and type ncpa.cpl
	3.	Right-click Ethernet → Properties → Select Internet Protocol Version 4 (TCP/IPv4) → Properties.
	4.	Change from automatic (DHCP) to Use the following IP address:
	•	IP Address: 192.168.20.2
	•	Subnet Mask: 255.255.255.0
	•	Default Gateway: 192.168.20.1
	•	Preferred DNS: 8.8.8.8
	6.	Click OK to apply.
	7.	Verify via Command Prompt: ipconfig

# Step 5: Installing Active Directory, DNS, and DHCP Roles
	- Open Server Manager → Click Manage → Add Roles and Features.
	- On Before you begin screen → Click Next.
	- Select Role-based or feature-based installation → Next.
	- Select server name → Next.
	- On Select Server Roles:
	•	Check Active Directory Domain Services → Click Add Features.
	•	Check DNS Server → Add Features.
	•	Check DHCP Server → Add Features.
	- Click Next until the Install button appears → Click Install.
	- Wait for installation to complete.
You will now see notifications to promote this server to a domain controller and complete DHCP configuration.

# Step 6: Promoting Server to Domain Controller
	1.	Click Promote this server to a domain controller.
	2.	Select Add a new forest → Root domain name: myproject.local.
	3.	Next → Leave default forest & domain functional levels.
	4.	Domain controller capabilities: DNS Server and Global Catalog selected by default → Next.
	5.	Set Directory Services Restore Mode (DSRM) password → Next.
	6.	Ignore DNS delegation warning → Next.
	7.	Verify NetBIOS name → Next.
	8.	Click Next repeatedly until Install.
	9.	Server will reboot automatically.
	10.	Log in as Domain Administrator.

Verification:
	•	Tools → Active Directory Users and Computers → Expand domain → Domain Controllers → Confirm your server is listed.

 # Step 7: DNS Configuration – Reverse Lookup Zone
	1.	Tools → DNS → Expand server → Right-click Reverse Lookup Zones → New Zone.
	2.	Click Next → Select Primary Zone → Next.
	3.	Store in Active Directory (integrated zone) → Next.
	4.	Configure replication scope → Next.
	5.	IP version → IPv4 → Next.
	6.	Network ID → 192.168.20 → Next.
	7.	Dynamic updates → Allow only secure updates → Next → Finish.
	8.	Create PTR record:
	•	Right-click free space → New Pointer → Browse → Select server from Forward Lookup Zone → OK.
	9.	Update preferred DNS server on Ethernet adapter → Use server IP (192.168.20.2).
   
  # Step 8: DHCP Configuration
	1.	Click Complete DHCP Post-Install Configuration → Next → Use Administrator credentials → Commit → Close.
	2.	Create a new DHCP scope:
  - Click on tool → click on DHCP 
  - Expand your server name 
	- Right-click IPv4 → New Scope → Next
  - Name: Scope1 → Description → Next
	- IP range & subnet → Next
	- Exclusions → Next
	- Lease duration → Next
	- Configure options now → Next
	- Default gateway (router IP) → Add → Next
	- WINS server (if any) → Next
	- Activate scope → Next → Finish
	3.	Verify from Windows 10 client:
	•	Disable/enable Ethernet adapter
	•	Check IP using ipconfig /all

NB: Ensure Server and Windows 10 client are on same internal network.

# Step 9: Join Windows 10 to Domain
	1.	Windows 10 → Start → Settings → System → About → Join a domain
	2.	Enter domain: myproject.local → Next
	3.	Enter domain credentials → OK
	4.	Assign user name → Skip optional steps → Restart.

Verification:
	•	Server → Active Directory Users and Computers → Computers → Confirm client exists.

  # Step: 10 Creating and Logging in with a New User Account in Active Directory
	- On the server, navigate to Tools → Active Directory Users and Computers → expand your server
	2.	Create a New User
	•	In the left-panel, select Users.
	•	Right-click and choose New → User.
	•	Enter the First Name, Last Name, and User Logon Name.
	•	Click Next.
	3.	Set Password
	•	Enter a secure password for the user account.
	•	Check the option User must change password at next logon.
	•	Click Next, then Finish to create the account.
	4.	Login on Client PC
	•	On the Windows 10 client machine, log in using the newly created user credentials.
	•	The user will be prompted to change the password upon first login, as configured.

  # Step 11: Managing Users, OUs, and Security Groups

1.	Open Server Manager
2.	Go to Tools → Active Directory Users and Computers
3.	Right-click your domain name
4.	Click New → Organizational Unit
5.	Name it:
	•	HR
	•	IT
	•	Finance
	•	Clients

Now you can move users into those OUs.

Create Security Groups 
1.	Right-click an OU
2.	Click New → Group
3.  Name it: IT-Admins or HR-Staff
4.  Group scope: Global
5.	Group type: Security
6.	Click OK
7.	Open the group → Add members

Now you can give permissions to the group instead of users.

# Additional Server Tasks
Once the Active Directory Domain Service, DNS Server and DCHP Server have been installed and configured, you can perform a variety of administrative and management tasks to maintain, monitor and optimize your server enviroment. Examples are:

Active Directory (AD) Tasks
	
  •	Manage existing user accounts:
	
  •	Reset passwords
	
  •	Lock and Unlock accounts
	
  •	Modify user properties
	
  •	Delegate administrative permissions
	
  •	Configure and apply Group Policy Objects (GPOs)
	
  •	Monitor AD replication and server health
	
  •	Manage computer accounts:
	
  •	Join/remove computers to/from the domain

DNS Server Tasks
	
  •	Create and manage forward and reverse lookup zones
	
  •	Add, modify, or remove DNS records (A, CNAME, MX, PTR)
	
  •	Configure DNS forwarding and conditional forwarding
	
  •	Monitor DNS server performance and logs
	
  •	Configure dynamic updates for client computers


DHCP Server Tasks
	
  •	Create and manage scopes and IP address ranges
	
  •	Configure DHCP reservations and exclusions
	
  •	Set lease durations and DHCP options (gateway, DNS, WINS)
	
  •	Monitor active leases and client assignments
	
  •	Configure DHCP failover and high availability

# Conclusion
This Project demonstrates the setup and configuration of a windows server enviroment with Active Directory Domain Server, DNS and DHCP, along with basic administrative tasks. It demonstrates practical skills in server administration, network configuration and user and computer management with a domain. The project reflects hand-on experience in maintaining a sercure and functional enviroment, which is directly relevant to IT and Cybersecurity roles.


