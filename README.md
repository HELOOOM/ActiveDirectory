# ActiveDirectory

## Project description:

The main objective of Active Directory is to provide centralized identification and authentication services to a network of computers using the Windows system. It also enables the assignment and enforcement of policies, the distribution of software, and the installation of critical updates by administrators.
Active Directory lists items in a managed network such as user accounts, servers, workstations, shared folders, printers, and more. It aims at making it easy for a user to find shared resources, and allows administrators to control their usage through distribution, duplication, partitioning, and secure access to listed resources.

## Part I: Active Directory configuration

1. We installed Windows Server 2016 Standard in a VM.

![image](https://user-images.githubusercontent.com/56129562/149035634-199a6e52-5888-4421-b71c-818feefd13e2.png)

2. Now let's install Active Directory, we just need to follow these steps:

![Configure this local server](https://user-images.githubusercontent.com/56129562/149035682-63a03d39-25a8-401d-babf-360650b6086e.png)

![WELCOME TO SERVER MANAGER](https://user-images.githubusercontent.com/56129562/149035694-a0057d0f-f755-4815-b188-e9962eaba813.png)

![WELCOME TO SERVER MANAGER](https://user-images.githubusercontent.com/56129562/149035718-6039e775-d952-44bc-a8bd-4dbfa6ec9d66.png)

![WELCOME TO SERVER MANAGER](https://user-images.githubusercontent.com/56129562/149035736-d19b0443-a014-4b84-ba8f-49c65bfad35d.png)

![WELCOME TO SERVER MANAGER](https://user-images.githubusercontent.com/56129562/149035767-9feb640d-a7bd-41c4-82b0-478a7ff51f66.png)


![Confirm installation selections](https://user-images.githubusercontent.com/56129562/149035780-f120d7b0-e451-47df-a094-8fd330c878a0.png)

After the installation of AD, we need to configure it for the first time.

First, we need to add a new forest , in which we are going to mention our domain `medmac.me`.
![Deployment Configuration](https://user-images.githubusercontent.com/56129562/149035847-863974f2-3b93-4e83-9987-cafc53b4937d.png)

We need to assign a password to our default user.
![Domain Controller Options](https://user-images.githubusercontent.com/56129562/149035885-57cafbc3-46c9-43ca-91cb-a77f2d9fa180.png)

**Note**: We can create a `.bat` script to do all this stuff for us.

```bash
Import-Module ADDSDeployment
Install-ADDSForest `
-CreateDnsDelegation:$false `
-DatabasePath "C:\Windows\NTDS" `
-DomainMode "WinThreshold" `
-DomainNetbiosName "MEDMAC" `
-ForestMode "WinThreshold" `
-InstallDns:$true `
-LogPath "C:\Windows\NTDS" `
-NoRebootOnCompletion:$false `
-SysvolPath "C:\Windows\SYSVOL" `
-Force:$true
```
![Prerequisites Check](https://user-images.githubusercontent.com/56129562/149036334-a5f9d172-f3f8-488d-9641-b5d3a7d756df.png)
After the installation we restart the server.


## Adding a user to the domain
We create a new user `ouhidaoui@medmac.me`. And add it to the domain.

![New Object - User](https://user-images.githubusercontent.com/56129562/149036525-d425c31d-fe29-41fc-84ee-4eced562dc74.png)

![New Object - User](https://user-images.githubusercontent.com/56129562/149036540-3d286e15-0cfb-47ef-a20d-d6230351fe16.png)

**Note**: In order to avoid any login problems, we need to explicitely allow the user to log in into his account. To do so, we need to add it to the `Allow log on locally` policy.

![Allow log on locally Properties](https://user-images.githubusercontent.com/56129562/149036697-0ffcb40d-2266-4025-a87d-f4218e9c1a53.png)

For the first time, we must change the password to log in.
![Other user](https://user-images.githubusercontent.com/56129562/149036734-a3af3218-6b31-4c49-b56a-25afbefa132a.png)

## Roaming Profiles

A profile is a folder that contains all the settings pertaining to a user’s working environment. By default, the profile is stored in the C:\Users directory.
A roaming profile, on the other hand, is stored on a network instead on the local drive of the machine where you are logged. A Roaming profile is cached locally by default. The advantage of a roaming profile is that a user can log into any machine in the domain and have a consistent working environment.

First, we need to create a Roaming folder and share it across the network. 
![Pasted Graphic 18](https://user-images.githubusercontent.com/56129562/149036941-a852cc63-0c76-487b-85ce-b0709634224e.png)

![Pasted Graphic 20](https://user-images.githubusercontent.com/56129562/149036947-a5bb72ab-2209-40ba-814e-457d70b3fd22.png)

![Oussama Hidaoui Properties](https://user-images.githubusercontent.com/56129562/149037022-ec4a65e7-8df4-4ba4-b1dd-65a40fa5296e.png)

**Final Results**:
![Desktop](https://user-images.githubusercontent.com/56129562/149037087-0fa8750f-001d-47c9-a1c3-013ec92b9d43.png)

We can map the folders as a network drive
![v Devices and drives (2)](https://user-images.githubusercontent.com/56129562/149037121-150f3bd1-ee8b-4343-90c9-a4560052ff3e.png)

## Mandatory profiles

A mandatory user profile is a roaming user profile that has been pre-configured by an administrator to specify settings for users. ... Configuration changes made during a user's session that are normally saved to a roaming user profile are not saved when a mandatory user profile is assigned.

![New Object - User](https://user-images.githubusercontent.com/56129562/149037203-5c37dafd-b17a-49eb-93f5-61cae2e4646c.png)

We create a new mandatory user, which is a basic user.

After the first login, we get the following message;
![We can't sign into your account](https://user-images.githubusercontent.com/56129562/149037294-369ebd29-5a19-43da-8a85-2286d8376164.png)

![You've been signed in with temporary](https://user-images.githubusercontent.com/56129562/149037303-12a4a745-fbd2-4964-b770-dcc1ad56a0b1.png)

## Part II: Active Directory Groups

Groups in Active Directory allow to assemble any type of object (user account, computer account, group) into a single entity in order to simplify administrative work in an Active Directory forest. In addition, the groups are characterized by their scope, which will limit their application in the forest. But also by their type, they can be assigned security authorizations if their type allows it.
1. We Create three organizational units that will allow you to manage and organize users and user groups. We name them respectively `Casablanca` , `Fez` and `Rabat`.

![A Forest medmac me](https://user-images.githubusercontent.com/56129562/149037493-54f293c8-7207-4a58-a03b-8b55f51d204d.png)

We create 3 new users for each organizational unit. The result will look like this.
![Active Directory Users and Computers](https://user-images.githubusercontent.com/56129562/149037540-626853ac-3f1f-422c-8705-49f3df1dad03.png)

![Active Directory Users and Computers](https://user-images.githubusercontent.com/56129562/149037553-d379eef7-b203-44dd-b4d4-666b4497e6fd.png)

![Active Directory Users and Computers](https://user-images.githubusercontent.com/56129562/149037568-c46f6a3f-b42e-4d8c-b5f0-f988dc0633fa.png)


## Part III: Group Policy Object

Group Policies (or GPO for Group Policy Object) are centralized management functions of the Microsoft Windows family. They allow the management of computers and users in an Active Directory environment. Group Policies are part of the IntelliMirror family of technologies, which include managing disconnected computers, managing roaming users or managing folder redirection, and managing files in offline mode.

1. Apply a security policy for the user `Sales-dir-casablanca`, which will prevent the user from having the “control panel configuration” in the start menu.

First we need to create a new GPO (Group Policy Object):
![omain Policy](https://user-images.githubusercontent.com/56129562/149037663-df03b1d7-3767-407f-b722-382904601be4.png)

Then we need to add the user to this GPO:
![Select User, Computer, or Group](https://user-images.githubusercontent.com/56129562/149037697-81897c56-608c-4402-a859-4bf36e48bc74.png)

Now, let's block the access to control panel, On the policy editor window, the setting that we’re looking for is under User Configuration > Adminsitrative Templates > Control Panel , find one setting named `Prohibit access to Control Panel and PC settings`.

![Pasted Graphic 37](https://user-images.githubusercontent.com/56129562/149037811-2cdb7217-cfcf-4a03-a34f-96f83938411e.png)

We make sure it is set to `Enabled`.
![Prohibit access to Control Panel and PC settings](https://user-images.githubusercontent.com/56129562/149037853-9e255ff1-0eb0-4a51-84cf-7af6d4c15655.png)

Now we need to apply the settings by the following command `gpupdate /force`

![Updating policy](https://user-images.githubusercontent.com/56129562/149037875-b81ce129-5b5a-4d6e-a6f7-1ceecc302200.png)

As a last step, we need to link the user to the GPO.
![Select GPO](https://user-images.githubusercontent.com/56129562/149037936-668005fc-4eec-4ba5-b70f-1ea19ec20b91.png)

**Results**
When trying to access control panel, or Windows Settings, we get the following error.
![Restrictions](https://user-images.githubusercontent.com/56129562/149038024-7c51e574-4f24-40f5-a864-121c6962c9c1.png)

