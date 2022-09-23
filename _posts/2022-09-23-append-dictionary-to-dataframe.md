---
title: "[Pandas] Dataframe에 dictionary를 append"
date: 2022-09-23 21:35:00 +0900
categories: dev Data+AI
tags: Pandas
---

### Problem

기존의 dataframe에 dictionary 형태로 주어진 data를 append하고 싶다.

### Solution

1. Dictionary를 dataframe으로 변환한다.

```python
df_dict = pd.DataFrame([data])
```

2. [concat](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.concat.html)으로 합친다.

```python
df_output = pd.concat([df_origin, df_dict], ignore_index=True)
```

### Reference
- https://stackoverflow.com/questions/51774826/append-dictionary-to-data-frame
