---
title: Badstore v1.2.3
date: 2019-02-12 04:14:54
---

The following is a vulnhub engine search from 2004. While this website may not be completely safe from advanced attacks, it serves as a valuable learning resource for beginners like me to practice identifying and exploiting common and easy-to-find web application vulnerabilities.

Boot the machine, then perform host scanning using `netdiscover`.

```shell
$ sudo netdiscover
```

After obtaining the target IP, perform port scanning using `nmap`.

```shell
$ sudo nmap -sSC -Pn <IP>
```

- `nmap`: Calls the Nmap program.
- `-sS`: Performs a TCP SYNC scan, which is a fast and efficient type of port scan.
- `-C`: Enables Nmap's default script to get more information about the target.
- `-Pn`: Ignores host discovery and continues scanning on the given target, regardless of whether the target responds or not.
- `<IP>`: The IP address of the target to be scanned

So, this command will perform a TCP SYN port scan on `<IP>` display the version of Nmap in use, enable Nmap default scripts to gather additional information about the target, and will continue the scan without verifying whether the target is responding or not.

![Scanning The Target](https://pbs.twimg.com/media/GK6bE6vagAAu4EM?format=jpg&name=large)

we also found MySQL service open, Let’s try to access the database using **root** user.

```shell
...
PORT     STATE SERVICE  VERSION
80/tcp   open  http     Apache httpd 1.3.28 ((Unix) mod_ssl/2.8.15 OpenSSL/0.9.7c)
| http-robots.txt: 5 disallowed entries 
|_/cgi-bin /scanbot /backup /supplier /upload
|_http-server-header: Apache/1.3.28 (Unix) mod_ssl/2.8.15 OpenSSL/0.9.7c
|_http-title: Welcome to BadStore.net v1.2.3s
| http-methods: 
|_  Potentially risky methods: TRACE
443/tcp  open  ssl/http Apache httpd 1.3.28 ((Unix) mod_ssl/2.8.15 OpenSSL/0.9.7c)
| sslv2: 
|   SSLv2 supported
|   ciphers: 
|     SSL2_RC4_128_EXPORT40_WITH_MD5
|     SSL2_RC4_64_WITH_MD5
|     SSL2_DES_192_EDE3_CBC_WITH_MD5
|     SSL2_RC2_128_CBC_WITH_MD5
|     SSL2_IDEA_128_CBC_WITH_MD5
|     SSL2_DES_64_CBC_WITH_MD5
|     SSL2_RC4_128_WITH_MD5
|_    SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
| ssl-cert: Subject: commonName=www.badstore.net/organizationName=BadStore.net/stateOrProvinceName=Illinois/countryName=US
| Subject Alternative Name: email:root@badstore.net
| Not valid before: 2006-05-10T12:52:53
|_Not valid after:  2009-02-02T12:52:53
|_http-server-header: Apache/1.3.28 (Unix) mod_ssl/2.8.15 OpenSSL/0.9.7c
|_ssl-date: 2024-02-20T12:53:57+00:00; -1d01h58m15s from scanner time.
|_http-title: Welcome to BadStore.net v1.2.3s
| http-robots.txt: 5 disallowed entries 
|_/cgi-bin /scanbot /backup /supplier /upload
| http-methods: 
|_  Potentially risky methods: TRACE
3306/tcp open  mysql    MySQL 4.1.7-standard
| mysql-info: 
|   Protocol: 10
|   Version: 4.1.7-standard
...
```

```shell
$ mysql -h <IP> -u root -p
```

From this exposed Database we are able to obtain:

![Successfully Accessed The Database.](https://pbs.twimg.com/media/GK7ASVmaoAAYv1R?format=png&name=medium)