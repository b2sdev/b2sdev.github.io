---
title: "DBeaver에서 CSV를 import한 table을 조인하기"
date: 2022-05-27 22:30:00 +0900
categories: blog database
tags: 데이터베이스 database db dbeaver
---

DBeaver에서 CSV 파일을 import하여 table을 만들 수 있는데, 이렇게 CSV 파일로 생성한 table은 DBeaver에 포함된 표준 CSV 드라이버로는 조인을 할 수 없다. 3rd party에서 제공하는 CSV 드라이버가 필요하다.

https://dbeaver.io/forum/viewtopic.php?f=2&t=1657

https://stackoverflow.com/questions/56321526/dbeaver-will-not-allow-me-to-join-tables-or-run-window-functions