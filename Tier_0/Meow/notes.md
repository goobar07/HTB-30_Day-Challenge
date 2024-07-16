# [Meow](https://app.hackthebox.com/starting-point)

## Target IP - 10.129.51.250

### Enumeration

Here we are checking if we are able to establish a connection with the target machine. In conclusion we are able to establish a connection between our lab with the target.

**Command 1** -

`ping 10.129.51.250`

**Output** -

``` 
PING 10.129.51.250 (10.129.51.250) 56(84) bytes of data.
64 bytes from 10.129.51.250: icmp_seq=1 ttl=62 time=705 ms
64 bytes from 10.129.51.250: icmp_seq=2 ttl=62 time=234 ms
64 bytes from 10.129.51.250: icmp_seq=3 ttl=62 time=236 ms
64 bytes from 10.129.51.250: icmp_seq=4 ttl=62 time=2383 ms
64 bytes from 10.129.51.250: icmp_seq=5 ttl=62 time=1962 ms
64 bytes from 10.129.51.250: icmp_seq=6 ttl=62 time=1199 ms
64 bytes from 10.129.51.250: icmp_seq=7 ttl=62 time=475 ms
^C
--- 10.129.51.250 ping statistics ---
7 packets transmitted, 7 received, 0% packet loss, time 6069ms
rtt min/avg/max/mdev = 233.603/1027.815/2383.110/793.317 ms, pipe 3
```

Here we are scanning the target machine for the open ports or services. In conclusion we find out that there is a service named as `telnet` which is open on 23/tcp port. We can try accessing the service.

**Command 2** -

`nmap -sV 10.129.51.250`

**Output** -

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-15 12:55 IST
Nmap scan report for 10.129.51.250
Host is up (0.86s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
23/tcp open  telnet  Linux telnetd
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 71.20 seconds
```

### Foothold

Here we tried to access the target machine through the telnet and tried different usernames like admin, administrator, and finally root which in the end worked without any password and we were able to get the root shell of the target machine in which we got the flag.txt.

**Command 3** - 

`telnet 10.129.51.250`

**Output** - 

```
Trying 10.129.51.250...
Connected to 10.129.51.250.
Escape character is '^]'.

  █  █         ▐▌     ▄█▄ █          ▄▄▄▄
  █▄▄█ ▀▀█ █▀▀ ▐▌▄▀    █  █▀█ █▀█    █▌▄█ ▄▀▀▄ ▀▄▀
  █  █ █▄█ █▄▄ ▐█▀▄    █  █ █ █▄▄    █▌▄█ ▀▄▄▀ █▀█


Meow login: root
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-77-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon 15 Jul 2024 07:42:35 AM UTC

  System load:           0.21
  Usage of /:            41.7% of 7.75GB
  Memory usage:          4%
  Swap usage:            0%
  Processes:             143
  Users logged in:       0
  IPv4 address for eth0: 10.129.51.250
  IPv6 address for eth0: dead:beef::250:56ff:feb0:3799

 * Super-optimized for small spaces - read how we shrank the memory
   footprint of MicroK8s to make it the smallest full K8s around.

   https://ubuntu.com/blog/microk8s-memory-optimisation

75 updates can be applied immediately.
31 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Mon Sep  6 15:15:23 UTC 2021 from 10.10.14.18 on pts/0
root@Meow:~#
``` 

### This machine has been pawned.
~goobar07
