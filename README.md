# Active Directory Server Documentation

This repository documents an existing Active Directory (AD) server setup on Windows Server 2022, hosted in a VMware environment. The configuration includes Group Policy Management, File Services, Service Accounts, Single Purpose Computers, Windows File Sharing (NTFS and Shared), Effective Permissions and Inheritance, and Access-Based Enumeration (ABE). The environment consists of one Windows Server 2022 VM and two Windows 11 client VMs.

## Table of Contents
1. [Overview](#overview)
2. [Environment Details](#environment-details)
3. [Active Directory Structure](#active-directory-structure)
4. [Group Policy Management](#group-policy-management)
5. [File Services](#file-services)
6. [Service Accounts](#service-accounts)
7. [Single Purpose Computers](#single-purpose-computers)
8. [Windows File Sharing](#windows-file-sharing)
9. [Effective Permissions and Inheritance](#effective-permissions-and-inheritance)
10. [Access-Based Enumeration](#access-based-enumeration)
11. [Validation](#validation)
12. [Troubleshooting Tips](#troubleshooting-tips)
13. [Contributing](#contributing)
14. [License](#license)

## Overview
This documentation captures the configuration of a lightweight AD environment for managing users, groups, and resources in a lab setup. Key features include:
- **Group Policy Management**: Centralized user and computer settings.
- **File Services**: Shared file storage.
- **Service Accounts**: Accounts for running services securely.
- **Single Purpose Computers**: Restricted client for specific tasks.
- **Windows File Sharing**: NTFS and share permissions for secure access.
- **Effective Permissions and Inheritance**: Granular access control.
- **Access-Based Enumeration**: Hides folders from unauthorized users.

## Environment Details
- **Hypervisor**: VMware Workstation/Player
- **Server VM**:
  - OS: Windows Server 2022
  - RAM: 4 GB
  - Disk: 60 GB
  - IP Address: [e.g., 192.168.1.10]
  - Hostname: [e.g., DC01]
- **Client VM 1 (Security Accounts/Single Purpose)**:
  - OS: Windows 11 Pro
  - RAM: 4 GB
  - Disk: 62 GB
  - IP Address: [e.g., 192.168.1.11]
  - Hostname: [e.g., CLIENT1]
- **Client VM 2 (General Client)**:
  - OS: Windows 11 Pro
  - RAM: 4 GB
  - Disk: 62 GB
  - IP Address: [e.g., 192.168.1.12]
  - Hostname: [e.g., CLIENT2]
- **Network**: Internal network (NAT/Host-Only)
- **Domain**: [e.g., lab.local]

**How to Document**:
- Check IP addresses and hostnames in `ipconfig` (Command Prompt) on each VM.
- Confirm domain name in ADUC or System Properties.

**Image Placeholder**:
![VM Overview](images/vm-overview.png)
*Insert a screenshot of VMware showing all three VMs with their names and statuses.*

## Active Directory Structure
- **Domain**: [e.g., lab.local]
- **Organizational Units (OUs)**:
  - [e.g., Users, Computers, SinglePurpose]
  - Describe any custom OUs and their purpose.
- **Groups**:
  - [e.g., Docs_Users, Admins]
  - Note security groups used for permissions.
- **Users**:
  - [e.g., user1, user2]
  - List key user accounts (exclude passwords).

**How to Document**:
- Open ADUC (`dsa.msc`) on the server.
- Note the OU structure, groups, and key users.
- Export structure: Right-click domain > Export List.

**Image Placeholder**:
![AD Structure](images/ad-structure.png)
*Insert a screenshot of ADUC showing the OU structure and groups.*

## Group Policy Management
- **GPOs Configured**:
  - [e.g., User Restrictions: Disables Control Panel for users]
  - [e.g., Computer Security: Enables Windows Defender]
- **Linked OUs**:
  - [e.g., User Restrictions linked to Users OU]
- **Key Settings**:
  - Describe specific policies (e.g., disable USB drives, restrict apps).

**How to Document**:
- Open GPMC (`gpmc.msc`) on the server.
- Expand Group Policy Objects, note GPO names and settings.
- Check OU links under the domain.
- Run `gpresult /r` on clients to confirm applied GPOs.

**Image Placeholder**:
![GPO Settings](images/gpo-settings.png)
*Insert a screenshot of GPMC showing a GPO’s settings or OU links.*

## File Services
- **File Server Role**: Installed
- **Shared Folders**:
  - [e.g., Path: C:\Shares\Docs, Share Name: Docs]
  - List shared folders and their purposes.
- **Quotas** (if any):
  - [e.g., 1GB limit on Docs folder]
- **Groups with Access**:
  - [e.g., Docs_Users has Read/Write]

**How to Document**:
- Open Server Manager > File and Storage Services > Shares.
- List shared folders and their properties.
- Check folder properties in File Explorer for share details.

**Image Placeholder**:
![Shared Folders](images/shared-folders.png)
*Insert a screenshot of Server Manager showing configured shares.*

## Service Accounts
- **Accounts**:
  - [e.g., svc_app: Used for scheduled tasks]
  - List service accounts and their purposes.
- **Permissions**:
  - [e.g., svc_app has Write access to C:\Shares\Logs]
- **Usage**:
  - [e.g., Runs a backup script]

**How to Document**:
- In ADUC, filter for service accounts (e.g., check “Password never expires”).
- Note permissions in File Explorer or service properties.
- Check Task Scheduler for tasks using service accounts.

**Image Placeholder**:
![Service Account](images/service-account.png)
*Insert a screenshot of ADUC showing a service account’s properties.*

## Single Purpose Computers
- **Client VM**: [e.g., CLIENT1 in SinglePurpose OU]
- **Restrictions**:
  - [e.g., AppLocker restricts to Notepad and Calculator]
  - Describe GPOs or settings limiting functionality.
- **Purpose**:
  - [e.g., Used for secure script execution]

**How to Document**:
- In ADUC, check which OU contains CLIENT1.
- In GPMC, note GPOs linked to that OU.
- Log in to CLIENT1, test restrictions, and describe results.

**Image Placeholder**:
![AppLocker Policy](images/applocker-policy.png)
*Insert a screenshot of AppLocker settings in the GPO for SinglePurpose OU.*

## Windows File Sharing
- **NTFS Permissions**:
  - [e.g., C:\Shares\Docs: Docs_Users has Read/Write, Admins has Full Control]
- **Share Permissions**:
  - [e.g., Docs share: Authenticated Users has Full Control]
- **Access Path**:
  - [e.g., \\DC01\Docs]

**How to Document**:
- Right-click shared folder > Properties > Security for NTFS permissions.
- Go to Sharing tab > Share Permissions for share permissions.
- Test access from CLIENT2 using `\\server\share`.

**Image Placeholder**:
![NTFS and Share Permissions](images/ntfs-share-permissions.png)
*Insert a screenshot of the Security and Sharing tabs for a shared folder.*

## Effective Permissions and Inheritance
- **Inheritance Settings**:
  - [e.g., C:\Shares\Docs\Restricted has inheritance disabled]
- **Effective Permissions**:
  - [e.g., user1 has Read only to Restricted folder]
- **Examples**:
  - Describe a folder with custom permissions.

**How to Document**:
- In File Explorer, go to folder > Properties > Security > Advanced.
- Check “Disable inheritance” status.
- Use Effective Access tab to check a user’s permissions.

**Image Placeholder**:
![Effective Permissions](images/effective-permissions.png)
*Insert a screenshot of the Effective Access tab for a user on a folder.*

## Access-Based Enumeration
- **Status**: [e.g., Enabled on Docs share]
- **Behavior**:
  - [e.g., Users without permissions cannot see subfolders]
- **Configuration**:
  - Describe where ABE is applied.

**How to Document**:
- In Server Manager > File and Storage Services > Shares, check share properties for ABE.
- Log in to CLIENT2 with a limited user, access the share, and note visible folders.

**Image Placeholder**:
![ABE Settings](images/abe-settings.png)
*Insert a screenshot of the share properties showing ABE enabled.*

## Validation
- **GPOs**: [e.g., Control Panel disabled on CLIENT2]
- **File Access**: [e.g., user1 can access Docs from CLIENT2]
- **Single Purpose**: [e.g., CLIENT1 only runs approved apps]
- **Service Accounts**: [e.g., svc_app runs task successfully]
- **ABE**: [e.g., Limited user cannot see Restricted folder]

**How to Document**:
- Log in to CLIENT1 and CLIENT2 with test users.
- Test each feature and note results.
- Use Command Prompt or PowerShell to verify settings (e.g., `net share` for shares).

**Image Placeholder**:
![Validation Test](images/validation-test.png)
*Insert a screenshot of a client accessing a share or a restricted app failing.*

## Troubleshooting Tips
- **AD Issues**: Verify DNS settings (`nslookup lab.local`).
- **GPO Issues**: Run `gpresult /r` to check applied policies.
- **File Access**: Use Effective Access tool for permission conflicts.
- **ABE**: Ensure share is on Windows Server and ABE is enabled.

## Contributing
Contributions to improve this documentation are welcome. Submit issues or pull requests on GitHub.

## License
This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
