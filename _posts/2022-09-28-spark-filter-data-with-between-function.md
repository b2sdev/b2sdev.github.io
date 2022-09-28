---
title: "[Spark] date range로 시계열 데이터 필터링"
date: 2022-09-28 20:00:00 +0900
categories: dev data.ai
tags: Spark
---

Spark의 between 함수를 이용하여 특정 기간의 데이터를 필터링 시 주의할 점이 있다.

### Problem

Client에서 date range를 입력 받아 특정 기간의 데이터만을 다룰 때 between 함수를 이용한다.

```python
from pyspark.sql import functions as F

df.filter(F.col('datetime').between('2022-08-01', '2022-08-31'))
```

그런데, 이러한 경우 마지막 `2022-08-31`의 데이터를 모두 걸러지고 만다.

### Solution

원인은 입력으로 주어진 `2022-08-31`을 Spark 내부에서 `2022-08-31 00:00:00`로 인식하기 때문이다.

아래와 같이 초 단위까지 명시해야 누락 없이 데이터 필터링이 가능하다.

```python
df.filter(F.col('datetime').between('2022-08-01 00:00:00', '2022-08-31 23:59:59'))
```

### Reference

- https://stackoverflow.com/questions/43403903/pysparks-between-function-range-search-on-timestamps-is-not-inclusive
