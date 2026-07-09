# Important Directories

|Name:|Location:|Description:|
|---|---|---|
|%SYSTEMROOT%\Temp|`C:\Windows\Temp`|Global directory containing temporary system files accessible to all users on the system. All users, regardless of authority, are provided full read, write, and execute permissions in this directory. Useful for dropping files as a low-privilege user on the system.|
|%TEMP%|`C:\Users\<user>\AppData\Local\Temp`|Local directory containing a user's temporary files accessible only to the user account that it is attached to. Provides full ownership to the user that owns this folder. Useful when the attacker gains control of a local/domain joined user account.|
|%PUBLIC%|`C:\Users\Public`|Publicly accessible directory allowing any interactive logon account full access to read, write, modify, execute, etc., files and subfolders within the directory. Alternative to the global Windows Temp Directory as it's less likely to be monitored for suspicious activity.|
|%ProgramFiles%|`C:\Program Files`|folder containing all 64-bit applications installed on the system. Useful for seeing what kind of applications are installed on the target system.|
|%ProgramFiles(x86)%|`C:\Program Files (x86)`|Folder containing all 32-bit applications installed on the system. Useful for seeing what kind of applications are installed on the target system.|
# System Information

![[Pasted image 20260122183414.png]]

|Type|Description|
|---|---|
|`General System Information`|Contains information about the overall target system. Target system information includes but is not limited to the `hostname` of the machine, OS-specific details (`name`, `version`, `configuration`, etc.), and `installed hotfixes/patches` for the system.|
|`Networking Information`|Contains networking and connection information for the target system and system(s) to which the target is connected over the network. Examples of networking information include but are not limited to the following: `host IP address`, `available network interfaces`, `accessible subnets`, `DNS server(s)`, `known hosts`, and `network resources`.|
|`Basic Domain Information`|Contains Active Directory information regarding the domain to which the target system is connected.|
|`User Information`|Contains information regarding local users and groups on the target system. This can typically be expanded to contain anything accessible to these accounts, such as `environment variables`, `currently running tasks`, `scheduled tasks`, and `known services`.|

## Commands

```cmd-session
C:\htb> systeminfo


Host Name:                 DESKTOP-htb
OS Name:                   Microsoft Windows 10 Pro
OS Version:                10.0.19042 N/A Build 19042
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Workstation
OS Build Type:             Multiprocessor Free

<snipped>
```

```cmd-session
C:\htb> hostname

DESKTOP-htb
```

```cmd-session
C:\htb> ver

Microsoft Windows [Version 10.0.19042.2006]
```

```cmd-session
C:\htb> arp /a

<SNIP>

Interface: 10.0.25.17 --- 0x17
  Internet Address      Physical Address      Type
  10.0.25.1             00-e0-67-15-cf-43     dynamic
  10.0.25.5             54-9f-35-1c-3a-e2     dynamic
  10.0.25.10            00-0c-29-62-09-81     dynamic
  10.0.25.255           ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
  255.255.255.255       ff-ff-ff-ff-ff-ff     static

Interface: 172.16.50.15 --- 0x1a
  Internet Address      Physical Address      Type
  172.16.50.1           15-c0-6b-58-70-ed     dynamic
  172.16.50.20          80-e5-53-3c-72-30     dynamic
  172.16.50.32          fb-90-01-5c-1f-88     dynamic
  172.16.50.65          7a-49-56-10-3b-76     dynamic
  172.16.50.255         ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static

<SNIP>
```

## Checking our priveleges

```cmd-session
C:\htb> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                          State
============================= ==================================== ========
SeShutdownPrivilege           Shut down the system                 Disabled
SeChangeNotifyPrivilege       Bypass traverse checking             Enabled
SeUndockPrivilege             Remove computer from docking station Disabled
SeIncreaseWorkingSetPrivilege Increase a process working set       Disabled
SeTimeZonePrivilege           Change the time zone                 Disabled
```

# Finding files and directories

## Using Where

```cmd-session
C:\Users\student\Desktop>where calc.exe

C:\Windows\System32\calc.exe

C:\Users\student\Desktop>where bio.txt

INFO: Could not find files for the given pattern(s).
```

## Recursive Where

```cmd-session
C:\Users\student\Desktop>where /R C:\Users\student\ bio.txt

C:\Users\student\Downloads\bio.txt
```

## Using Wildcards

```cmd-session
C:\Users\student\Desktop>where /R C:\Users\student\ *.csv

C:\Users\student\AppData\Local\live-hosts.csv
```

# Environment Variables

## Variable Scope

In this context, `Scope` is a programming concept that refers to where variables can be accessed or referenced. 'Scope' can be broadly separated into two categories:

- **Global:**
    - Global variables are accessible `globally`. In this context, the global scope lets us know that we can access and reference the data stored inside the variable from anywhere within a program.
- **Local:**
    - Local variables are only accessible within a `local` context. `Local` means that the data stored within these variables can only be accessed and referenced within the function or context in which it has been declared.


|Scope|Description|Permissions Required to Access|Registry Location|
|---|---|---|---|
|`System (Machine)`|The System scope contains environment variables defined by the Operating System (OS) and are accessible globally by all users and accounts that log on to the system. The OS requires these variables to function properly and are loaded upon runtime.|Local Administrator or Domain Administrator|`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment`|
|`User`|The User scope contains environment variables defined by the currently active user and are only accessible to them, not other users who can log on to the same system.|Current Active User, Local Administrator, or Domain Administrator|`HKEY_CURRENT_USER\Environment`|
|`Process`|The Process scope contains environment variables that are defined and accessible in the context of the currently running process. Due to their transient nature, their lifetime only lasts for the currently running process in which they were initially defined. They also inherit variables from the System/User Scopes and the parent process that spawns it (only if it is a child process).|Current Child Process, Parent Process, or Current Active User|`None (Stored in Process Memory)`|

Both [set](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/set_1) and [setx](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/setx) are command line utilities that allow us to display, set, and remove environment variables. The difference lies in how they achieve those goals. The `set` utility only manipulates environment variables in the current command line session. This means that once we close our current session, any additions, removals, or changes will not be reflected the next time we open a command prompt. Suppose we need to make permanent changes to environment variables. In that case, we can use `setx` to make the appropriate changes to the registry, which will exist upon restart of our current command prompt session.

# Important Environment Variables

|Variable Name|Description|
|---|---|
|`%PATH%`|Specifies a set of directories(locations) where executable programs are located.|
|`%OS%`|The current operating system on the user's workstation.|
|`%SYSTEMROOT%`|Expands to `C:\Windows`. A system-defined read-only variable containing the Windows system folder. Anything Windows considers important to its core functionality is found here, including important data, core system binaries, and configuration files.|
|`%LOGONSERVER%`|Provides us with the login server for the currently active user followed by the machine's hostname. We can use this information to know if a machine is joined to a domain or workgroup.|
|`%USERPROFILE%`|Provides us with the location of the currently active user's home directory. Expands to `C:\Users\{username}`.|
|`%ProgramFiles%`|Equivalent of `C:\Program Files`. This location is where all the programs are installed on an `x64` based system.|
|`%ProgramFiles(x86)%`|Equivalent of `C:\Program Files (x86)`. This location is where all 32-bit programs running under `WOW64` are installed. Note that this variable is only accessible on a 64-bit host. It can be used to indicate what kind of host we are interacting with. (`x86` vs. `x64` architecture)|
# Managing services

[SC](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc754599\(v=ws.11\)) is a Windows executable utility that allows us to query, modify, and manage host services locally and over the network. We have other tools, like Windows Management Instrumentation (`WMIC`) and `Tasklist` that can also query and manage services for local and remote hosts.



