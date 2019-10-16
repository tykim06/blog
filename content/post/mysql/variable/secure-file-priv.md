---
title: "secure_file_priv"
date: 2019-10-16T18:58:07+09:00
categories:
- mysql
- variable
tags:
- file
- secure_file_priv
#thumbnailImage: //example.com/image.jpg
---

This variable is used to limit the effect of data import and export operations, such as those performed by the ***LOAD DATA*** and ***SELECT ... INTO OUTFILE*** statements and the ***LOAD_FILE()*** function.
<!--more-->

These operations are permitted only to users who have the ***FILE*** privilege.

# 기본 정보
---

Property			| Value
---				| ---
Command-Line Format		| --secure-file-priv=dir_name
System Variable			| secure_file_priv
Scope				| Global
Dynamic				| No
Type				| String
Default Value (>= 5.7.6)	| platform specific
Default Value (<= 5.7.5)	| empty string
Valid Values (>= 5.7.6) 	| empty string, dirname, NULL
Valid Values (<= 5.7.5) 	| empty string, dirname

secure_file_priv may be set as follows:

* If empty, the variable has no effect. This is not a secure setting.

* If set to the name of a directory, the server limits import and export operations to work only with files in that directory. The directory must exist; the server will not create it.

* If set to NULL, the server disables import and export operations.
