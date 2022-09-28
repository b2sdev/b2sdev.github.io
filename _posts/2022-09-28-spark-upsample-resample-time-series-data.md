---
title: "[Spark] 시계열 데이터를 일정한 주기를 갖도록 정리하기"
date: 2022-09-28 19:10:00 +0900
categories: dev Data+AI
tags: Spark
---

Window 함수를 사용하여 시계열 데이터를 일정한 주기(frequency)를 갖도록 정리할 수 있다.

### Problem

시계열 데이터를 일정한 주기를 갖는 데이터로 정리하고 싶다.

예를 들어, 아래처럼 13~15분 주기를 갖는 데이터를,

```text
+---+-------------------+----------+
| id|                 dt|some_value|
+---+-------------------+----------+
| J1|2019-12-29 12:07:38|       100|
| J1|2019-12-29 12:24:25|       200|
| J1|2019-12-29 12:37:58|       100|
| J8|2020-09-09 13:06:36|       300|
| J8|2020-09-09 13:21:37|       200|
| J8|2020-09-09 13:36:38|       400|
+---+-------------------+----------+
```

이렇게 정확히 15분 단위로 깔끔하게 정리하고 싶다.

```text
+---+-------------------+----------+
| id|                 dt|some_value|
+---+-------------------+----------+
| J1|2019-12-29 12:15:00|       100|
| J1|2019-12-29 12:30:00|       200|
| J1|2019-12-29 12:45:00|       100|
| J8|2020-09-09 13:00:00|       300|
| J8|2020-09-09 13:15:00|       200|
| J8|2020-09-09 13:30:00|       400|
+---+-------------------+----------+
```

### Solution

[`Window`](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html#pyspark.sql.functions.window) 함수를 사용하면 시계열 데이터를 일정한 주기(frequency)를 갖도록 정리할 수 있다.

```python
from pyspark.sql import functions as F

df.withColumn("datetime", F.window("dt", "15 minutes")["end"])
```

더 짧은 주기로 데이터를 정리하려면 window의 크기를 줄여 datetime 컬럼을 만들고 groupBy로 집계한다.

```python
df.withColumn("datetime", F.window("dt", "5 minutes")["end"] \
  .groupBy("datetime").agg(...)
```

### Reference
- https://stackoverflow.com/questions/63818647/pyspark-upsample-resample-time-series-data
