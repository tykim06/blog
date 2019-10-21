---
title: "Extending Varchar Column Size"
date: 2019-10-21T13:38:26+09:00
categories:
- mysql
tags:
- alter
- varchar
- online-ddl
- rebuilds-table
#thumbnailImage: //example.com/image.jpg
---

MySQL 5.7 은 VARCHAR column size 변경 시 online DDL을 지원하지만 일부 지원 되지 않는 경우가 있다.
<!--more-->


# Rebuilds Table
---

공식 문서에서 extending varchar column size 지원은 다음과 같다.

Operation			| In Place	| Rebuilds Table	| Permits Concurrent DML	| Only Modifies Metadata
---				| ---		| ---			| --- 				| ---
Extending VARCHAR column size	| Yes		| No			| Yes				| Yes

MySQL 5.7은 공식적으로 extending VARCHAR column size online DDL을 지원한다. 그렇다면, online DDL이 지원되지 않는 경우는 언제 일까?

> The number of length bytes required by a VARCHAR column must remain the same. For VARCHAR columns of 0 to 255 bytes in size, one length byte is required to encode the value. For VARCHAR columns of 256 bytes in size or more, two length bytes are required. As a result, in-place ALTER TABLE only supports increasing VARCHAR column size from 0 to 255 bytes, or from 256 bytes to a greater size. ***In-place ALTER TABLE does not support increasing the size of a VARCHAR column from less than 256 bytes to a size equal to or greater than 256 bytes. In this case, the number of required length bytes changes from 1 to 2, which is only supported by a table copy (ALGORITHM=COPY).***


255 bytes 이하에서 256 bytes 이상으로 변경하는 경우에 ALGORITHM=COPY로 동작하여 online DDL 지원이 불가하다. VARCHAR 255 bytes 까지는 column size 저장을 위해 1 byte가 필요하지만 256 bytes 부터는 column size 저장을 위해 2 bytes가 필요하다.

# 255 bytes 계산
---

MySQL 4.1 버전부터 varchar(N)의 N 값은 byte가 아닌 size로 변경 되었기 때문에 byte 계산을 위해서 column 문자집합 확인이 필요하다. MySQL에서 utf8은 3 bytes, utf8mb4는 4 bytes로 설계 되어 utf8 인 경우 N*3 bytes, utf8mb4인 경우 N*4 bytes가 된다.

```
CREATE TABEL `tbl1` (
  `col1` VARCHAR(50) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- utf8mb4 문자집합으로 255/4 > 63, 63자 이하에서 64자 이상으로 변경 시 Rebuilds Table이 발생하여 ALGORITMH=COPY가 적용된다.
ALTER TABLE tbl1 ALGORITHM=INPLACE, CHANGE COLUMN col1 col1 VARCHAR(70);
ERROR 0A000: ALGORITHM=INPLACE is not supported. Reason: Cannot change column type INPLACE. Try ALGORITHM=COPY.
```

# 참고
---

> https://dev.mysql.com/doc/refman/5.7/en/innodb-online-ddl-operations.html#online-ddl-column-operations
