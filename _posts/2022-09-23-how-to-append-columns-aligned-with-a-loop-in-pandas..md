---
title: "[Pandas] 분산된 여러 파일의 데이터를 읽어서 하나의 dataframe으로 만들기"
date: 2022-09-23 22:05:00 +0900
categories: blog dev Data+AI
tags: Pandas
---

1. 각각의 파일을 읽어 dataframe의 list를 만든다.

```python
import pandas as pd
import glob

f_list = glob.glob("C:\\Users\\*.xlsx") 
all_data = [pd.read_excel(f) for f in f_list]
```

2. [concat](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.concat.html)으로 합친다.

```python
df = pd.concat(all_data, axis=1)
```

### Reference
- https://stackoverflow.com/questions/51956918/how-to-append-columns-aligned-with-a-loop-in-pandas
