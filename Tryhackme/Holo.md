### Network Scanning for Hosts


What is the last octet of the IP address of the public-facing web server?

We need to figure out which of our servers is running on port 80(http) or 443(https). So we are going to set rust to scan the 10.200.107.x network range using the /24 cidr notation this may take a while to complete. We can take note that port 80(http) is open on 10.200.107.33. That is likely going to be out public facing web server.

### Initial Probe
```shell
└─$ rustscan -a 10.200.107.0/24 --ulimit 5000 -- -sV -sC -p-                                                              
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Real hackers hack time ⌛

[~] The config file is expected to be at "/home/jowp/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.200.107.33:22
Open 10.200.107.33:80
Open 10.200.107.250:1337

```
How many ports are open on the web server?
> 3

What CME is running on port 80 of the web server?
> wordpress

What version of the CME is running on port 80 of the web server?
> 5.5.3

What is the HTTP title of the web server?
> holo.live

### Scan of the Webserver
```shell
└─$ rustscan -a 10.200.107.33 --ulimit 5000 -- -A          
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
🌍HACK THE PLANET🌍
...
Open 10.200.107.33:22
Open 10.200.107.33:80
Open 10.200.107.33:33060
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")
...
PORT      STATE SERVICE REASON  VERSION
22/tcp    open  ssh     syn-ack OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
|_http-title: holo.live
|_http-generator: WordPress 5.5.3
33060/tcp open  mysqlx? syn-ack
...
```
