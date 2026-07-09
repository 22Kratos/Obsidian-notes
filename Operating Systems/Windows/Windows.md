# Operating System Structure

|Directory|Function|
|---|---|
|Perflogs|Can hold Windows performance logs but is empty by default.|
|Program Files|On 32-bit systems, all 16-bit and 32-bit programs are installed here. On 64-bit systems, only 64-bit programs are installed here.|
|Program Files (x86)|32-bit and 16-bit programs are installed here on 64-bit editions of Windows.|
|ProgramData|This is a hidden folder that contains data that is essential for certain installed programs to run. This data is accessible by the program no matter what user is running it.|
|Users|This folder contains user profiles for each user that logs onto the system and contains the two folders Public and Default.|
|Default|This is the default user profile template for all created users. Whenever a new user is added to the system, their profile is based on the Default profile.|
|Public|This folder is intended for computer users to share files and is accessible to all users by default. This folder is shared over the network by default but requires a valid network account to access.|
|AppData|Per user application data and settings are stored in a hidden user subfolder (i.e., cliff.moore\AppData). Each of these folders contains three subfolders. The Roaming folder contains machine-independent data that should follow the user's profile, such as custom dictionaries. The Local folder is specific to the computer itself and is never synchronized across the network. LocalLow is similar to the Local folder, but it has a lower data integrity level. Therefore it can be used, for example, by a web browser set to protected or safe mode.|
|Windows|The majority of the files required for the Windows operating system are contained here.|
|System, System32, SysWOW64|Contains all DLLs required for the core features of Windows and the Windows API. The operating system searches these folders any time a program asks to load a DLL without specifying an absolute path.|
|WinSxS|The Windows Component Store contains a copy of all Windows components, updates, and service packs|
# File System

There are 5 types of Windows file systems: FAT12, FAT16, FAT32, NTFS, and exFAT. FAT12 and FAT16 are no longer used on modern Windows operating systems.
FAT32 (File Allocation Table) is widely used across many types of storage devices such as USB memory sticks and SD cards but can also be used to format hard drives. The "32" in the name refers to the fact that FAT32 uses 32 bits of data for identifying data clusters on a storage device.

**`Pros of FAT32:`**

- Device compatibility - it can be used on computers, digital cameras, gaming consoles, smartphones, tablets, and more.
- Operating system cross-compatibility - It works on all Windows operating systems starting from Windows 95 and is also supported by MacOS and Linux.

**`Cons of FAT32:`**

- Can only be used with files that are less than 4GB.
- No built-in data protection or file compression features.
- Must use third-party tools for file encryption.

NTFS (New Technology File System) is the default Windows file system since Windows NT 3.1. In addition to making up for the shortcomings of FAT32, NTFS also has better support for metadata and better performance due to improved data structuring.

**`Pros of NTFS:`**

- NTFS is reliable and can restore the consistency of the file system in the event of a system failure or power loss.
- Provides security by allowing us to set granular permissions on both files and folders.
- Supports very large-sized partitions.
- Has journaling built-in, meaning that file modifications (addition, modification, deletion) are logged.

**`Cons of NTFS:`**

- Most mobile devices do not support NTFS natively.
- Older media devices such as TVs and digital cameras do not offer support for NTFS storage devices.

## Permissions in NTFS

|Permission Type|Description|
|---|---|
|Full Control|Allows reading, writing, changing, deleting of files/folders.|
|Modify|Allows reading, writing, and deleting of files/folders.|
|List Folder Contents|Allows for viewing and listing folders and subfolders as well as executing files. Folders only inherit this permission.|
|Read and Execute|Allows for viewing and listing files and subfolders as well as executing files. Files and folders inherit this permission.|
|Write|Allows for adding files to folders and subfolders and writing to a file.|
|Read|Allows for viewing and listing of folders and subfolders and viewing a file's contents.|
|Traverse Folder|This allows or denies the ability to move through folders to reach other files or folders. For example, a user may not have permission to list the directory contents or view files in the documents or web apps directory in this example c:\users\bsmith\documents\webapps\backups\backup_02042020.zip but with Traverse Folder permissions applied, they can access the backup archive.|

## Integrity Control Access Control List (icacls)

We can list out the NTFS permissions on a specific directory by running either `icacls` from within the working directory or `icacls C:\Windows` against a directory not currently in.

```cmd-session
icacls c:\windows
c:\windows NT SERVICE\TrustedInstaller:(F)
           NT SERVICE\TrustedInstaller:(CI)(IO)(F)
           NT AUTHORITY\SYSTEM:(M)
           NT AUTHORITY\SYSTEM:(OI)(CI)(IO)(F)
           BUILTIN\Administrators:(M)
           BUILTIN\Administrators:(OI)(CI)(IO)(F)
           BUILTIN\Users:(RX)
           BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
           CREATOR OWNER:(OI)(CI)(IO)(F)
           APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(RX)
           APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(OI)(CI)(IO)(GR,GE)
           APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(RX)
           APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

The resource access level is listed after each user in the output. The possible inheritance settings are:

- `(CI)`: container inherit
- `(OI)`: object inherit
- `(IO)`: inherit only
- `(NP)`: do not propagate inherit
- `(I)`: permission inherited from parent container

Basic access permissions are as follows:
- `F` : full access
- `D` :  delete access
- `N` :  no access
- `M` :  modify access
- `RX` :  read and execute access
- `R` :  read-only access
- `W` :  write-only access

# NTFS vs Share Permission

The `Server Message Block protocol` (`SMB`) is used in Windows to connect shared resources like files and printers. It is used in large, medium, and small enterprise environments.
![[Pasted image 20260120125902.png]]

## Share Permissions

| Permission     | Description                                                                                                                                 |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `Full Control` | Users are permitted to perform all actions given by Change and Read permissions as well as change permissions for NTFS files and subfolders |
| `Change`       | Users are permitted to read, edit, delete and add files and subfolders                                                                      |
| `Read`         | Users are allowed to view file & subfolder contents                                                                                         |
## NTFS Basic Permissions

|Permission|Description|
|---|---|
|`Full Control`|Users are permitted to add, edit, move, delete files & folders as well as change NTFS permissions that apply to all allowed folders|
|`Modify`|Users are permitted or denied permissions to view and modify files and folders. This includes adding or deleting files|
|`Read & Execute`|Users are permitted or denied permissions to read the contents of files and execute programs|
|`List folder contents`|Users are permitted or denied permissions to view a listing of files and subfolders|
|`Read`|Users are permitted or denied permissions to read the contents of files|
|`Write`|Users are permitted or denied permissions to write changes to a file and add new files to a folder|
|`Special Permissions`|A variety of advanced permissions options|
## NTFS Special Permissions

|Permission|Description|
|---|---|
|`Full control`|Users are permitted or denied permissions to add, edit, move, delete files & folders as well as change NTFS permissions that apply to all permitted folders|
|`Traverse folder / execute file`|Users are permitted or denied permissions to access a subfolder within a directory structure even if the user is denied access to contents at the parent folder level. Users may also be permitted or denied permissions to execute programs|
|`List folder/read data`|Users are permitted or denied permissions to view files and folders contained in the parent folder. Users can also be permitted to open and view files|
|`Read attributes`|Users are permitted or denied permissions to view basic attributes of a file or folder. Examples of basic attributes: system, archive, read-only, and hidden|
|`Read extended attributes`|Users are permitted or denied permissions to view extended attributes of a file or folder. Attributes differ depending on the program|
|`Create files/write data`|Users are permitted or denied permissions to create files within a folder and make changes to a file|
|`Create folders/append data`|Users are permitted or denied permissions to create subfolders within a folder. Data can be added to files but pre-existing content cannot be overwritten|
|`Write attributes`|Users are permitted or denied to change file attributes. This permission does not grant access to creating files or folders|
|`Write extended attributes`|Users are permitted or denied permissions to change extended attributes on a file or folder. Attributes differ depending on the program|
|`Delete subfolders and files`|Users are permitted or denied permissions to delete subfolders and files. Parent folders will not be deleted|
|`Delete`|Users are permitted or denied permissions to delete parent folders, subfolders and files.|
|`Read permissions`|Users are permitted or denied permissions to read permissions of a folder|
|`Change permissions`|Users are permitted or denied permissions to change permissions of a file or folder|
|`Take ownership`|Users are permitted or denied permission to take ownership of a file or folder. The owner of a file has full permissions to change any permissions|
# Windows Services

```powershell-session
Get-Service | ? {$_.Status -eq "Running"} | select -First 2 |fl


Name                : AdobeARMservice
DisplayName         : Adobe Acrobat Update Service
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32OwnProcess

Name                : Appinfo
DisplayName         : Application Information
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {RpcSs, ProfSvc}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32OwnProcess, Win32ShareProcess
```

|Service|Description|
|---|---|
|smss.exe|Session Manager SubSystem. Responsible for handling sessions on the system.|
|csrss.exe|Client Server Runtime Process. The user-mode portion of the Windows subsystem.|
|wininit.exe|Starts the Wininit file .ini file that lists all of the changes to be made to Windows when the computer is restarted after installing a program.|
|logonui.exe|Used for facilitating user login into a PC|
|lsass.exe|The Local Security Authentication Server verifies the validity of user logons to a PC or server. It generates the process responsible for authenticating users for the Winlogon service.|
|services.exe|Manages the operation of starting and stopping services.|
|winlogon.exe|Responsible for handling the secure attention sequence, loading a user profile on logon, and locking the computer when a screensaver is running.|
|System|A background system process that runs the Windows kernel.|
|svchost.exe with RPCSS|Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Remote Procedure Call (RPC) Service (RPCSS).|
|svchost.exe with Dcom/PnP|Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Distributed Component Object Model (DCOM) and Plug and Play (PnP) services.|
## Local Security Authority Subsystem Service (LSASS)

`lsass.exe` is the process that is responsible for enforcing the security policy on Windows systems. When a user attempts to log on to the system, this process verifies their log on attempt and creates access tokens based on the user's permission levels. LSASS is also responsible for user account password changes.

# Sessions

|Account|Description|
|---|---|
|Local System Account|Also known as the `NT AUTHORITY\SYSTEM` account, this is the most powerful account in Windows systems. It is used for a variety of OS-related tasks, such as starting Windows services. This account is more powerful than accounts in the local administrators group.|
|Local Service Account|Known as the `NT AUTHORITY\LocalService` account, this is a less privileged version of the SYSTEM account and has similar privileges to a local user account. It is granted limited functionality and can start some services.|
|Network Service Account|This is known as the `NT AUTHORITY\NetworkService` account and is similar to a standard domain user account. It has similar privileges to the Local Service Account on the local machine. It can establish authenticated sessions for certain network services.|
# Execution Policy

| **Policy**     | **Description**                                                                                                                                                                                                                                                  |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `AllSigned`    | All scripts can run, but a trusted publisher must sign scripts and configuration files. This includes both remote and local scripts. We receive a prompt before running scripts signed by publishers that we have not yet listed as either trusted or untrusted. |
| `Bypass`       | No scripts or configuration files are blocked, and the user receives no warnings or prompts.                                                                                                                                                                     |
| `Default`      | This sets the default execution policy, `Restricted` for Windows desktop machines and `RemoteSigned` for Windows servers.                                                                                                                                        |
| `RemoteSigned` | Scripts can run but requires a digital signature on scripts that are downloaded from the internet. Digital signatures are not required for scripts that are written locally.                                                                                     |
| `Restricted`   | This allows individual commands but does not allow scripts to be run. All script file types, including configuration files (`.ps1xml`), module script files (`.psm1`), and PowerShell profiles (`.ps1`) are blocked.                                             |
| `Undefined`    | No execution policy is set for the current scope. If the execution policy for ALL scopes is set to undefined, then the default execution policy of `Restricted` will be used.                                                                                    |
| `Unrestricted` | This is the default execution policy for non-Windows computers, and it cannot be changed. This policy allows for unsigned scripts to be run but warns the user before running scripts that are not from the local intranet zone.                                 |
Sometimes we will find that we are unable to run scripts on a system. This is due to a security feature called the `execution policy`, which attempts to prevent the execution of malicious scripts.

# Windows Management Instrumentation (WMI)

WMI is a subsystem of PowerShell that provides system administrators with powerful tools for system monitoring. The goal of WMI is to consolidate device and application management across corporate networks.

|**Component Name**|**Description**|
|---|---|
|WMI service|The Windows Management Instrumentation process, which runs automatically at boot and acts as an intermediary between WMI providers, the WMI repository, and managing applications.|
|Managed objects|Any logical or physical components that can be managed by WMI.|
|WMI providers|Objects that monitor events/data related to a specific object.|
|Classes|These are used by the WMI providers to pass data to the WMI service.|
|Methods|These are attached to classes and allow actions to be performed. For example, methods can be used to start/stop processes on remote machines.|
|WMI repository|A database that stores all static data related to WMI.|
|CIM Object Manager|The system that requests data from WMI providers and returns it to the application requesting it.|
|WMI API|Enables applications to access the WMI infrastructure.|
|WMI Consumer|Sends queries to objects via the CIM Object Manager.|
Some of the uses for WMI are:

- Status information for local/remote systems
- Configuring security settings on remote machines/applications
- Setting and changing user and group permissions
- Setting/modifying system properties
- Code execution
- Scheduling processes
- Setting up logging

# Microsoft Management Console (MMC)

The MMC can be used to group snap-ins, or administrative tools, to manage hardware, software, and network components within a Windows host. It has been around since Windows Server 2000 and runs on all Windows versions. We can also use MMC to create custom tools and distribute them to users. MMC works with the concept of snap-ins, allowing administrators to create a customized console with only the administrative tools needed to manage several services. These snap-ins can be added to manage both local and remote systems.

# Security Identifier (SID)

Each of the security principals on the system has a unique security identifier (SID). The system automatically generates SIDs. This means that even if, for example, we have two identical users on the system, Windows can distinguish the two and their rights based on their SIDs. SIDs are string values with different lengths, which are stored in the security database. These SIDs are added to the user's access token to identify all actions that the user is authorized to take.

A SID consists of the Identifier Authority and the Relative ID (RID). In an Active Directory (AD) domain environment, the SID also includes the domain SID.

```powershell-session
whoami /user

USER INFORMATION
----------------

User Name           SID
=================== =============================================
ws01\bob S-1-5-21-674899381-4069889467-2080702030-1002
```

|**Number**|**Meaning**|**Description**|
|---|---|---|
|S|SID|Identifies the string as a SID.|
|1|Revision Level|To date, this has never changed and has always been `1`.|
|5|Identifier-authority|A 48-bit string that identifies the authority (the computer or network) that created the SID.|
|21|Subauthority1|This is a variable number that identifies the user's relation or group described by the SID to the authority that created it. It tells us in what order this authority created the user's account.|
|674899381-4069889467-2080702030|Subauthority2|Tells us which computer (or domain) created the number|
|1002|Subauthority3|The RID that distinguishes one account from another. Tells us whether this user is a normal user, a guest, an administrator, or part of some other group|

## Security Accounts Manager (SAM) and Access Control Entries (ACE)

The access rights themselves are managed by Access Control Entries (ACE) in Access Control Lists (ACL). The ACLs contain ACEs that define which users, groups, or processes have access to a file or to execute a process, for example.
The permissions to access a securable object are given by the security descriptor, classified into two types of ACLs: the `Discretionary Access Control List (DACL)` or `System Access Control List (SACL)`. Every thread and process started or initiated by a user goes through an authorization process. An integral part of this process is access tokens, validated by the Local Security Authority (LSA). In addition to the SID, these access tokens contain other security-relevant information.

## User Account Control (UAC)

security feature in Windows to prevent malware from running or manipulating processes that could damage the computer or its contents. There is the Admin Approval Mode in UAC, which is designed to prevent unwanted software from being installed without the administrator's knowledge or to prevent system-wide changes from being made. Surely you have already seen the consent prompt if you have installed a specific software, and your system has asked for confirmation if you want to have it installed. Since the installation requires administrator rights, a window pops up, asking you if you want to confirm the installation. With a standard user who has no rights for the installation, execution will be denied, or you will be asked for the administrator password. This consent prompt interrupts the execution of scripts or binaries that malware or attackers try to execute until the user enters the password or confirms execution.
![[Pasted image 20260120153922.png]]


