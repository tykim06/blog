---
title: "Change Replication Filter"
date: 2019-10-11T17:22:20+09:00
categories:
- mysql
- syntax
tags:
- change-replication-filter
- replication
---

***CHANGE REPLICATION*** FILTER sets one or more replication filtering rules on the slave in the same way as starting the slave mysqld with replication filtering options such as *--replicate-do-db* or *--replicate-wild-ignore-table*. Unlike the case with the server options, this statement does not require restarting the server to take effect, only that the slave SQL thread be stopped using ***STOP SLAVE SQL_THREAD*** first (and restarted with ***START SLAVE SQL_THREAD*** afterwards). ***CHANGE REPLICATION FILTER*** requires the ***REPLICATION_SLAVE_ADMIN*** or ***SUPER*** privilege. Use the FOR CHANNEL channel clause to make a replication filter specific to a replication channel, for example on a multi-source replication slave. Filters applied without a specific FOR CHANNEL clause are considered global filters, meaning that they are applied to all replication channels.
<!--more-->

# 구문
---

```
CHANGE REPLICATION FILTER filter[, filter]
	[, ...] [FOR CHANNEL channel]

filter:
    REPLICATE_DO_DB = (db_list)
  | REPLICATE_IGNORE_DB = (db_list)
  | REPLICATE_DO_TABLE = (tbl_list)
  | REPLICATE_IGNORE_TABLE = (tbl_list)
  | REPLICATE_WILD_DO_TABLE = (wild_tbl_list)
  | REPLICATE_WILD_IGNORE_TABLE = (wild_tbl_list)
  | REPLICATE_REWRITE_DB = (db_pair_list)

db_list:
    db_name[, db_name][, ...]

tbl_list:
    db_name.table_name[, db_name.table_name][, ...]
wild_tbl_list:
    'db_pattern.table_pattern'[, 'db_pattern.table_pattern'][, ...]

db_pair_list:
    (db_pair)[, (db_pair)][, ...]

db_pair:
    from_db, to_db
```

# 예시 구문
---

```
CHANGE REPLICATION FILTER REPLICATE_DO_DB = (d1), REPLICATE_IGNORE_DB = (d2);
CHANGE REPLICATION FILTER REPLICATE_WILD_DO_TABLE = ('db1.old%'); 
CHANGE REPLICATION FILTER REPLICATE_WILD_IGNORE_TABLE = ('db1.new%', 'db2.new%');
```

# 출처
---

> https://dev.mysql.com/doc/refman/8.0/en/change-replication-filter.html
