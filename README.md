# Active Directory Bulk User Creation

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



  - Click on mydomain.com , New , Organizational Unit



  - Create new user for admin 





 

