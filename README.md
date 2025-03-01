<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Deploying Active Directory in Azure</h1>

This project focuses on deploying and managing an Active Directory (AD) domain in Microsoft Azure, simulating real-world IT administration.
1. Active Directory Setup & Domain Configuration
    - Install Active Directory Domain Services on DC-1 and promote it to a Domain Controller with a new Active Directory forest.
    - Create Organizational Units (OUs) in Active Directory Users and Computers (ADUC) to organize users and devices.
    - Set up a Domain Admin account and use it for further administration.
    - Join Client-1 to the domain and verify its presence in ADUC.

2. Remote Desktop & User Management
    - Enable Remote Desktop for domain users on Client-1.
    - Create multiple user accounts using a PowerShell script, organizing them in ADUC under the appropriate Organizational Units.
    - Test user authentication by logging into Client-1 with a newly created user.

This lab provides hands-on experience with Active Directory setup, domain management, user authentication, remote access configuration, and automation using PowerShell in a cloud-based environment.

<h2>Environments and Technologies Used</h2>

  - Microsoft Azure (Virtual Machines/Compute)
  - Active Directory
  - Domain Name System
  - PowerShell


<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>

After remoting into DC, we will open Server Manager and navigate to "Add Roles and Features."
![Step10](https://github.com/user-attachments/assets/ba5f0134-0ab9-46a6-bf68-0f7668bde8df)

Keep all settings at their default values until reaching the "Select Server Roles" screen.

  - Check the box for Active Directory Domain Services (AD DS) to install the necessary components.
  - On the confirmation screen, also check “Restart the destination server automatically if required” to streamline the installation process.

Why is this important?

Installing AD DS is essential for setting up DC as a Domain Controller, enabling centralized user authentication, group policy management, and resource control within the domain. Allowing automatic restarts ensures the installation completes without manual intervention.
![Step11](https://github.com/user-attachments/assets/6fb1f0b9-e349-4bd1-9ee1-e8985a6a1999)
![Step12](https://github.com/user-attachments/assets/4599112a-3c23-498d-83fb-c2aa1e3ccbef)
![Step13](https://github.com/user-attachments/assets/b7a9982e-23f9-4c2c-a438-1e6c7afaa154)
![Step14](https://github.com/user-attachments/assets/a24f7afa-b094-4bfe-aa2a-4450f7f70822)

Once the AD DS installation is complete, we need to perform post-deployment configuration to fully set up DC as a Domain Controller.

  - Click the flag notification in Server Manager.
  - Select “Promote this server to a domain controller.”
![Step15](https://github.com/user-attachments/assets/139d179d-a2cb-49f2-8802-7f892fbc4a1a)

Next, we will select “Add a new forest” and name it mydomain.com for the lab setup.

  - Keep all settings at their default values until prompted to create another login.
  - For the sake of the lab, use root/root as the credentials and proceed with the configuration.
![Step16](https://github.com/user-attachments/assets/3f282324-ea14-465d-a72a-67524e222736)

Once the installation is complete, DC will automatically restart.

  - From this point forward, you must log in using the domain credentials you created.
  - The correct format for logging in is:
    mydomain.com\labuser

Why is this important?

Now that DC is a Domain Controller, authentication is handled by Active Directory. This means all users must log in using their domain credentials rather than local machine accounts. This ensures centralized user management, security policies, and access control across the domain.

![Step17](https://github.com/user-attachments/assets/418212a3-e8b9-4b05-afb3-101711366b7a)

Now that we are logged back into DC, Active Directory should be fully configured and ready to use.

   - Click on the search bar in the taskbar.
   - Type "Active Directory Users and Computers" and select it from the results.
![Step18](https://github.com/user-attachments/assets/2a937782-35a7-4206-ab20-e6e65e0f0ddb)
![Step19](https://github.com/user-attachments/assets/d3f99034-2a4d-4d9b-b491-03b70203d2b7)


Next, we will create three Organizational Units (OUs) in Active Directory Users and Computers (ADUC):

  - _ADMINS – For administrative accounts.
  - _EMPLOYEES – For standard user accounts.
  - _CLIENTS – For domain-joined computers.

Why is this important?

Organizational Units (OUs) help structure the domain by grouping users and computers logically. This makes it easier to apply Group Policies, manage permissions, and maintain security best practices within the Active Directory environment.
![Screenshot 2025-03-01 124634](https://github.com/user-attachments/assets/bac87bcc-e469-4f7e-a982-aca6fe44a76b)

Next, we will create a user account in Active Directory Users and Computers (ADUC) within the _ADMINS Organizational Unit (OU):

  - Navigate to the _ADMINS OU in ADUC.
  - Create a new user named Jane within this OU.
  - Set her user logon as jane_admin.
  - After creating the account, add jane_admin to the "Domain Admins" security group to grant administrative privileges.

Why is this important?

Placing jane_admin in the _ADMINS OU helps keep administrative accounts organized and separate from standard users. Assigning her to the Domain Admins group ensures she has the necessary privileges to manage users, security policies, and domain settings.
![Screenshot 2025-03-01 130054](https://github.com/user-attachments/assets/d9ec6112-a249-4ca4-8fc6-0e86eb6d7434)
![Step20](https://github.com/user-attachments/assets/3d5526d4-1ae0-46cc-8795-7de8b14f5f54)
![Step21](https://github.com/user-attachments/assets/a9a6b10f-f4e1-4525-986c-2fe10284f534)
![Step22](https://github.com/user-attachments/assets/7ea7e1bc-9632-4180-8829-ee09b827517d)
![Step23](https://github.com/user-attachments/assets/5070d057-e3c9-4830-a3bf-0e9ed1b08b89)

Next, we will Remote Desktop into the Client machine to join it to the domain using our newly created admin account (jane_admin).
Steps:

  - Use Remote Desktop to connect to Client using its public IP address and the credentials for mydomain.com\jane_admin.
  - Open System Properties and navigate to "Change settings" under Computer Name, Domain, and Workgroup settings.
  - Click "Change", select "Domain", and enter mydomain.com.
  - When prompted, enter jane_admin’s credentials to authorize the domain join.
  - Restart Client to apply the changes.

Why is this important?

Joining Client to the domain ensures it is centrally managed within Active Directory, allowing group policies, security settings, and administrative controls to be applied consistently across the network.

![Step25](https://github.com/user-attachments/assets/285d28d7-28fe-4dda-873f-761dd04fd3c0)
![Step26](https://github.com/user-attachments/assets/1730c90f-7aed-4847-a8d7-b291cf39be99)
![Step27](https://github.com/user-attachments/assets/07d97fad-f588-4269-872b-064a2dadf271)
![Step28](https://github.com/user-attachments/assets/3c13e064-794d-4566-a364-1696327ab0bb)

Back in DC, open Active Directory Users and Computers (ADUC) and navigate to the Computers container.
Steps:

  - Locate Client in the Computers folder.
  - Click and drag Client into the _CLIENTS Organizational Unit (OU).
![Step29](https://github.com/user-attachments/assets/7f35d334-dc98-4630-b9a5-0e88f74b6255)
