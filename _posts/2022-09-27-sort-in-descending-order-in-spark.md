---
title: "[Spark] 내림차순으로 정렬하기"
date: 2022-09-27 20:20:00 +0900
categories: dev spark
tags: spark
---

Spark SQL dataframe을 특정 컬럼의 값을 기준으로 내림차순으로 정렬하려면 `orderBy` 함수를 이용한다.

```python
df.orderBy('column_name', ascending=False)
```

```python
df.orderBy(df['column_name'].desc())
df.orderBy(df.column_name.desc())
```

### Reference

- https://stackoverflow.com/questions/34514545/sort-in-descending-order-in-pyspark
