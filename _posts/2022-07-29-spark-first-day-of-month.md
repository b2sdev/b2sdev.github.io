---
title: "[Spark] 월의 시작 날짜와 마지막 날짜 구하기"
date: 2022-07-11 22:40:00 +0900
categories: blog
tags: spark
---

Spark에서는 월(month)의 시작 날짜와 마지막 날짜를 바로 구할 수 있는 함수를 제공한다.

- 시작 날짜 : [pyspark.sql.functions.trunc](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.functions.trunc.html)
- 마지막 날짜 : [pyspark.sql.functions.last_day](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.functions.last_day.html)

```python
import pyspark.sql.functions as *

df \
    .withColumn("first_date", trunc(col("date"), "month")) \
    .withColumn("last_date", last_day(col("date"))) \
    .show()
```

```
+----------+----------+----------+
|      date|first_date| last_date|
+----------+----------+----------+
|2022-03-20|2022-03-01|2022-03-31|
|2022-04-19|2022-04-01|2022-04-30|
|2022-05-18|2022-05-01|2022-05-31|
+----------+----------+----------+
```