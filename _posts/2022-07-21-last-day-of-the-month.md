---
title: "[Python] 월(month)의 마지막 날짜 구하기"
date: 2022-07-21 21:50:00 +0900
categories: blog dev
tags: python
---

Python으로 월(month)의 마지막 날짜를 구하려면 `calendar` 모듈의 `monthlen(year, month)` 함수를 사용한다.

```python
import calendar

assert calendar.monthlen(2002, 5) == 31
assert calendar.monthlen(2008, 2) == 29
assert calendar.monthlen(2100, 2) == 28
```

Python 3.7에서 사용 가능하며 Python 3.8에서는 `calendar._monthelen`를 사용한다.

### Reference
- https://stackoverflow.com/questions/42950/how-to-get-the-last-day-of-the-month