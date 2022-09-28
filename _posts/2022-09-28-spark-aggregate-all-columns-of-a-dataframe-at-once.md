---
title: "[Spark] Dataframe의 모든 컬럼을 한 번에 집계하기"
date: 2022-09-28 19:00:00 +0900
categories: dev Data+AI
tags: Spark
---

Dataframe에 groupBy를 적용 시 모든 컬럼을 한 번에 집계할 수 있다.

### Problem

Dataframe에 groupBy를 적용 시 (key를 제외한) 모든 컬럼을 집계하고 싶다.

### Solution

* 모든 컬럼에 대해 동일한 집계 함수(eg. sum, min, avg 등)를 사용할 수 있다면, 집계 함수를 바로 사용한다.

```python
df.groupBy('key').sum()
df.groupBy('key').min()
df.groupBy('key').avg()
```

* 동일한 집계 함수를 사용할 수 없으면, 컬럼별로 적용할 집계 함수를 명시한 `그룹 표현식`을 정의한다.

```python
columns = ["col1", "col2", "col3"]
functions = [sum, min, avg]

exprs = [func(col) for col, func in zip(columns, functions)

df.groupBy('key').agg(*exprs)
```
