# Active Directory Bulk User Creation

**Note: Compatibility Warning for Windows 11**

Before proceeding, it's important to note that there are known compatibility issues with running Windows 11 in the virtual environment created for this project. Windows 11 may not function properly within the VM, and certain features could be impacted.

This guide primarily focuses on setting up Active Directory and automating user creation using PowerShell scripts on Windows Server 2022. If you encounter difficulties with Windows 11, please be aware that it might not work as expected in this specific configuration.

Thank you for your understanding, and let's get started with the project!


## Overview 

This project involves creating a virtual environment with Windows Server 2022 and Windows 11, setting up Active Directory and using PowerShell scripts to automate the creation of multiple users.




## Prerequisites

  - Virtualization software (e.g VirtualBox)
      - Go to www.virtualbox.org and download a Virtual machine
  <img width="960" alt="image" src="https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/a8e4f075-bc6e-4c85-88ae-cd1e2730ab48">

  - Windows Server 2022 ISO
    - Go to https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022 and download the ISO
  <img width="960" alt="image" src="https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/b2f8ca48-0e61-4252-a2b1-19a256d3605a">

  - Windows 11 ISO
      - Go to https://www.microsoft.com/software-download/windows11 and download the ISO
  <img width="960" alt="image" src="https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/60a7067b-be85-4763-b841-aa66e0ca98f5">

  - PowerShell script for user creation

## Project Architecture 
![AD Powershell Diagram project](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/a412ae94-9dc8-4774-962d-b4fcb8227c67)

## Network Component Overview

| Component            | Description                      | Notes                                              |
|----------------------|----------------------------------|----------------------------------------------------|
| Internet Cloud       | External internet connection      | Connected to NIC (Internet)                         |
| Windows Server 2022  | Main server hosting AD DS, RAS/NAT| Connected to NIC (Internet) and NIC (Internal)      |
| Domain/AD DS         | Active Directory Domain Services  | Part of Windows Server 2022 (DC)                    |
| RAS/NAT              | Routing and Remote Access / NAT   | Part of Windows Server 2022 (DC)                    |
| DHCP                 | Dynamic Host Configuration       | Part of Windows Server 2022 (DC)                    |
| VMWare Network       | Internal network for VMs           | Connected to NIC (Internal) on Windows Server 2022  |
| Client 1 (Windows)   | Virtual machine connected to VMWare| Connected to NIC (Internal) on VMWare Network       |

## Setting up the DC VM 

Once Windows Server 2022 ISO is installed , Create a DC virtual machine. Make sure you create two network adapters. NIC (Internet) and NIC (Internal).

  - Adapter 1 NIC(Internet):
<img width="491" alt="image" src="https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/3708cd6c-a98b-43c3-b4d9-e3286af48fc5">

  - Adapter 2 NIC(Internal):
<img width="500" alt="image" src="https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/d8b688f8-a495-4af1-92d9-9d85791929b2">

  - <b> Make sure you choose the Standard Desktop Experience for GUI </b>

  - Rename PC into DC

    ![image](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/4a7351a3-ec0f-4ac2-b4f4-be403bafcf18)

  - Rename Network Connections( Internet & Internal)
    
    ![image](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/ffd9baf4-4861-458e-a685-6788981b41c5)


  - Network(Internal) IP address and DNS Server

    ![image](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/4643878e-cf48-48ee-8e12-8a4321d5b9ac)

  - Installing Active Directory Services
    - Click on Add roles and features and select Active Directory Domain Services
   
![image](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/ddb1f366-4108-4a00-a289-ecec10f117a7)

![image](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/72f344d0-c1e1-434b-a43f-9af4b1e2f67b)

  - Click on notifications, add a new forest and name the Root domain

![image](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/22c349c6-440a-4060-bbc7-8ce77b2c3fb9)

- Head onto Active Directory Users and Computers

![VirtualBox_DC_27_11_2023_21_41_27](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/6640d6c5-29ab-4c2d-9199-12233b82045e)



  - Click on mydomain.com , New , Organizational Unit

![Admins Creation](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/7a85ddc0-e1d6-4b7b-b174-054ff833afe2)


  - Create admin role 


![Create Admin role](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/2e9def82-cb94-4195-bfb5-90444a62f205)

  - Log out and see if your Admin Account is working

![Admin account login](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/5cb22b33-b211-4477-826b-4c3383b51b24)

- Installing NAT/RAS

The purpose of NAT/RAS is to allow the client Windows 11 to be in the private network but still be able to access the internet through the domain controller.

  - Go to Add roles and features
 
![Roles and features](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/327336a7-2143-4fb3-86ec-6b428ba8f93f)

  - Choose Remote access

![Choose Remote Access](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/ebbdba3d-64c7-4594-bcff-3541e7a92dd3)

  - Install routing

![install routing](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/2e533666-eddd-4155-8398-ba1e5751196d)

  - Head to tools, Routing and Remote Access 

 ![Tools and Routing and Remote Access](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/a4453022-e502-4de3-ab9a-36dc4fae8105)

 - Right click on DC (local) and click configure and enable routing and remote access
   
  ![Configure and enable routing and remote Access](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/7f826e66-8dce-4896-9c5d-8c7df8ec591a)

  - choose Network address translation (NAT)

![NAT](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/25212260-93cf-4cd7-b0cb-ba256d596262)

  - Make sure you click on the Internet interface

![Click on the internet interface](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/4fed0667-2ea7-48ab-bbdd-e79ade2bd37e)

- Things should look like this so far:

![So far](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/63a9c5eb-c406-497f-b6f0-4ca1d01f7145)

- Now its time to set up a DHCP Server

  - Go to Add roles and features

![Roles and features](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/327336a7-2143-4fb3-86ec-6b428ba8f93f)

  - Select DHCP Server

![Select DCHP Server](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/e3485db5-8b1a-48f2-9880-209831aa5de2)

  - Now go to tools, DHCP

![Tools  DHCP](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/798b053a-859e-4797-b066-eb69f770119e)

  - Select New Scope

![New Scope](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/216027a9-052e-445f-bb96-782de2b03fa8)

  - Create your range

![Create your range](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/27037379-dd15-49a9-866f-efc5da937d11)

  - Start & End IP addresses and Subnet mask

![Start, End and SubnetMask](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/2072b2f1-ef22-4a67-a878-2ae7bae157d1)

  - Entering in the Domain Controllers IP address

![Entering in the DC IP address](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/cf53e891-57e8-4f10-b2dd-7be688796342)

  - Authorize and Referesh the DHCP Server

![Authorize and Refresh](https://github.com/EliasMo/Active-Directory-Bulk-User-Creation/assets/45215421/3f00f4db-d209-4eb7-9599-6d558a90b798)


## Powershell  

List of names to add:

![names](https://github.com/EliasMo/ADBulkUserCreation/assets/45215421/c492a7f8-6064-4983-9536-932f9f9cdd4a)

- Using the Script

Navigate to the script directory

![Script directory](https://github.com/EliasMo/ADBulkUserCreation/assets/45215421/3071252f-b8db-42d4-ac2b-cf36e0796203)

- Click Play and Voilà!, USER CREATION!!!

![User creation](https://github.com/EliasMo/ADBulkUserCreation/assets/45215421/d11bc8eb-6606-4d0e-96ff-700f0e9127ea)

- Check active directory to see if it worked

![Users](https://github.com/EliasMo/ADBulkUserCreation/assets/45215421/01ae7436-8fec-420f-8457-b57dd7204bba)

- Use 'find' to look for yourself

![me](https://github.com/EliasMo/ADBulkUserCreation/assets/45215421/dc088a86-7fec-4e57-bed9-580f192f78fb)


** Client Windows 11 VM

- Create your Client1 VM
<img width="701" alt="image" src="https://github.com/EliasMo/ADBulkUserCreation/assets/45215421/0e4b903d-5a22-4449-a5f8-e0a2f69358b3">

- Go to Network adapter 1
Change Adapter 1 to Internal Network:

<img width="575" alt="image" src="https://github.com/EliasMo/ADBulkUserCreation/assets/45215421/77af6117-f4b8-40f7-9fa8-e087821f7515">


# Conclusion

In conclusion, this project successfully achieved its main objective of creating a virtual environment with Windows Server 2022, setting up Active Directory, and automating user creation using PowerShell scripts. Unfortunately, Windows 11 compatibility issues with the virtual machine have been encountered, leading to the decision to conclude the project at this point.

If you're interested in exploring further or encountering any issues, you can refer to Josh Madakor's video tutorial [here](https://www.youtube.com/watch?v=MHsI8hJmggI).

Thank you for following along with this guide. Feel free to reach out if you have any questions or if there are additional topics you'd like to explore in future projects.
