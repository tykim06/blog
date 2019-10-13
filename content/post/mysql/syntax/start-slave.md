---
title: "Start Slave"
date: 2019-10-11T14:19:02+09:00
categories:
- mysql
- syntax
tags:
- start-slave
- replication
---

***START SLAVE*** with no thread_type options starts both of the slave threads. The I/O thread reads events from the master server and stores them in the relay log. The SQL thread reads events from the relay log and executes them.

<!--more-->

***START SLAVE*** requires the ***REPLICATION_SLAVE_ADMIN*** or ***SUPER*** privilege.

# 구문
---

```
START SLAVE [thread_types] [until_option] [connection_options] [channel_option]

thread_types:
    [thread_type [, thread_type] ... ]

thread_type:
    IO_THREAD | SQL_THREAD

until_option:
    UNTIL {   {SQL_BEFORE_GTIDS | SQL_AFTER_GTIDS} = gtid_set
          |   MASTER_LOG_FILE = 'log_name', MASTER_LOG_POS = log_pos
          |   RELAY_LOG_FILE = 'log_name', RELAY_LOG_POS = log_pos
          |   SQL_AFTER_MTS_GAPS  }

connection_options:
    [USER='user_name'] [PASSWORD='user_pass'] [DEFAULT_AUTH='plugin_name'] [PLUGIN_DIR='plugin_dir']


channel_option:
    FOR CHANNEL channel

gtid_set:
    uuid_set [, uuid_set] ...
    | ''

uuid_set:
    uuid:interval[:interval]...

uuid:
    hhhhhhhh-hhhh-hhhh-hhhh-hhhhhhhhhhhh

h:
    [0-9,A-F]

interval:
    n[-n]

    (n >= 1)
```

# 예시 구문
---

```
START SLAVE;
START SLAVE IO_THREAD;
START SLAVE UNTIL MASTER_LOG_FILE = 'mysql-bin.000012', MASTER_LOG_POS = 12016845;
```

# 출처
---

> https://dev.mysql.com/doc/refman/5.7/en/start-slave.html
