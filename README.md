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
