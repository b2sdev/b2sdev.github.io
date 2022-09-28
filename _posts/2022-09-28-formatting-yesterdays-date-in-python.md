---
title: "[Python] 어제 날짜 구하기"
date: 2022-09-28 19:55:00 +0900
categories: dev python
tags: Python
---

Python으로 어제 날짜를 구하려면 datetime 모듈의 timedelta 함수를 활용한다.

### Problem

Python으로 어제 날짜를 구하고 싶다.

### Solutio

[`datetime.timedelta()`](https://docs.python.org/library/datetime.html#timedelta-objects)를 이용한다.

```python
from datetime import date, datetime, timedelta

# yesterday = date.today() - timedelta(days=1)
# yesterday = date.today() + timedelta(days=-1)
yeaterday = datetime.now() - timedelta(days=1)
yesterday.strftime("%Y%m%d)
```

### Reference
- https://stackoverflow.com/questions/1712116/formatting-yesterdays-date-in-python
