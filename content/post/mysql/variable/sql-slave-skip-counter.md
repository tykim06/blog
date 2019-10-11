---
title: "sql_slave_skip_counter"
date: 2019-10-11T17:10:34+09:00
categories:
- mysql
- variable
tags:
- replication
- sql_slave_skip_counter
---

This statement skips the next N events from the master. This is useful for recovering from replication stops caused by a statement.
<!--more-->

# 구문
---

```
SET GLOBAL sql_slave_skip_counter = N
```

- For transactional tables, an event group corresponds to a transaction.
- For nontransactional tables, an event group corresponds to a single SQL statement.

# 예시 구문
---

```
STOP SLAVE;
SET GLOBAL sql_slave_skip_counter = 1;
START SLAVE;
```

# 출처
---

> https://dev.mysql.com/doc/refman/5.7/en/set-global-sql-slave-skip-counter.html
