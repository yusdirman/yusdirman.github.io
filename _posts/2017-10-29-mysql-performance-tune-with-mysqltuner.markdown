---
layout: post
title:  "MySQL performance tune with mysqltuner"
date:   2017-10-29 14:05:00 +0800
categories: MySQL
---

# Tune MySQL Performance with mysqltuner

1. Install mysqltuner at our server

```
diman@admin:~$ sudo apt-get install mysqltuner
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
libtext-template-perl
The following NEW packages will be installed:
libtext-template-perl mysqltuner
0 upgraded, 2 newly installed, 0 to remove and 3 not upgraded.
Need to get 135 kB of archives.
After this operation, 863 kB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://ftp.us.debian.org/debian stretch/main amd64 libtext-template-perl all 1.46-1 [53.1 kB]
Get:2 http://ftp.us.debian.org/debian stretch/main amd64 mysqltuner all 1.6.18-1 [81.8 kB]
Fetched 135 kB in 1s (71.4 kB/s)
Selecting previously unselected package libtext-template-perl.
(Reading database ... 39546 files and directories currently installed.)
Preparing to unpack .../libtext-template-perl_1.46-1_all.deb ...
Unpacking libtext-template-perl (1.46-1) ...
Selecting previously unselected package mysqltuner.
Preparing to unpack .../mysqltuner_1.6.18-1_all.deb ...
Unpacking mysqltuner (1.6.18-1) ...
Setting up mysqltuner (1.6.18-1) ...
Setting up libtext-template-perl (1.46-1) ...
diman@admin:~$
```

2. Run mysqltuner. You will need to enter administrative username and password for your database.


```
diman@admin:~$ mysqltuner

 >>  MySQLTuner 1.3.0 - Major Hayden <major@mhtx.net>
 >>  Bug reports, feature requests, and downloads at http://mysqltuner.com/
 >>  Run with '--help' for additional options and output filtering
Please enter your MySQL administrative login: admin
Please enter your MySQL administrative password:
[OK] Currently running supported MySQL version 5.5.50-0+deb8u1-log
[OK] Operating on 64-bit architecture

-------- Storage Engine Statistics -------------------------------------------
[--] Status: +ARCHIVE +BLACKHOLE +CSV -FEDERATED +InnoDB +MRG_MYISAM
[--] Data in PERFORMANCE_SCHEMA tables: 0B (Tables: 17)
[--] Data in InnoDB tables: 18M (Tables: 215)
[!!] Total fragmented tables: 215

-------- Security Recommendations  -------------------------------------------
[!!] User 'admindb@%' has no password set.

-------- Performance Metrics -------------------------------------------------
[--] Up for: 150d 22h 7m 12s (36M q [2.777 qps], 1K conn, TX: 25B, RX: 5B)
[--] Reads / Writes: 99% / 1%
[--] Total buffers: 192.0M global + 2.7M per thread (151 max threads)
[OK] Maximum possible memory usage: 597.8M (15% of installed RAM)
[OK] Slow queries: 0% (0/36M)
[OK] Highest usage of available connections: 15% (23/151)
[OK] Key buffer size / total MyISAM indexes: 16.0M/103.0K
[OK] Query cache efficiency: 31.6% (11M cached / 35M selects)
[OK] Query cache prunes per day: 0
[OK] Sorts requiring temporary tables: 0% (0 temp sorts / 7K sorts)
[!!] Temporary tables created on disk: 36% (37K on disk / 101K total)
[OK] Thread cache hit rate: 82% (204 created / 1K connections)
[OK] Table cache hit rate: 98% (377 open / 384 opened)
[OK] Open file limit used: 4% (51/1K)
[OK] Table locks acquired immediately: 100% (24M immediate / 24M locks)
[OK] InnoDB buffer pool / data size: 128.0M/18.7M
[OK] InnoDB log waits: 0
-------- Recommendations -----------------------------------------------------
General recommendations:
    Run OPTIMIZE TABLE to defragment tables for better performance
    Enable the slow query log to troubleshoot bad queries
    When making adjustments, make tmp_table_size/max_heap_table_size equal
    Reduce your SELECT DISTINCT queries without LIMIT clauses
Variables to adjust:
    tmp_table_size (> 16M)
    max_heap_table_size (> 16M)

```

The application will provide you with the recommendations and actions for you to do.


fyi: this is the database server of a strandard Rails application that I've built.

Thanks..


yusdirman
