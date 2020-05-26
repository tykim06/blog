---
title: "mysqldump 주의사항"
date: 2020-05-26T10:43:34+09:00
---
# Release Version이 다른 경우
Release Version이 다른 경우 시스템 스키마가 다들 수 있기 때문에 백업 시 --all-databases 옵션은 권장 하지 않는다.
--all-databases 옵션을 추가한 경우 data restore 후에 mysql_upgrade script를 반드시 실행한다.

