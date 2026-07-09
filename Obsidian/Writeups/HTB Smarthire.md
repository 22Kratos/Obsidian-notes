Nmap command:
```
sudo nmap -sC -sV -vv -oA smarthire smarthire.htb
```

Output:
```
# Nmap 7.99 scan initiated Tue Jun  9 02:56:37 2026 as: /usr/lib/nmap/nmap -sC -sV -vv -oA smarthire smarthire.htb
Nmap scan report for smarthire.htb (10.129.245.215)
Host is up, received reset ttl 63 (0.19s latency).
Scanned at 2026-06-09 02:56:37 EDT for 18s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.15 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 41:3c:e3:bb:88:70:99:7f:b8:96:59:48:9b:85:98:69 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLg1Y2xxe0euIHDjjKTIrxL+XZXgsBabs0FMAMKBL8arUuELui3vhlkgcDVGcZ4vFWnsiu4osw5INjfcQGkp2BY=
|   256 d5:9d:fd:6b:be:d8:39:6f:3f:43:ab:0e:f6:3e:22:db (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPc/kqsR+WxwGPMNTukcYPjzZRGjQL6N+0HsGIS1NV4U
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: OPTIONS GET HEAD
|_http-title: Overview | SmartHIRE
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Jun  9 02:56:55 2026 -- 1 IP address (1 host up) scanned in 17.60 seconds
```

We have 2 ports open
22, which is SSH, running on Ubutnu server
80, which is HTTP, running nginx server

