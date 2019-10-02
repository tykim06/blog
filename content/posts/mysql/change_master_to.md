---
title: "[MYSQL] CHANGE MASTER TO"
date: 2019-10-02T18:27:04+09:00
draft: true
---

# 구문
---
	CHANGE MASTER TO
	  MASTER_HOST='{ip}',
	  MASTER_POST={port},
	  MASTER_USER='{user}',
	  MASTER_PASSWORD='{password}',
	  MASTER_LOG_FILE='{bin_log_file}',
	  MASTER_LOG_POS={bin_log_position};
