---
title: "[Python] List에 담긴 dictionary의 값의 datatype을 변경하기"
date: 2022-09-23 23:05:00 +0900
categories: dev python
tags: Python
---

### Problem

아래와 같이 dictionary들의 list가 있는데, 값의 datatype을 문자열에서 int나 float로 변경하고 싶다.

```python
# AS-IS
lst = [ { 'a':'1' , 'b':'2' , 'c':'3' }, { 'd':'4' , 'e':'5' , 'f':'6' } ]

# TO-BE
lst = [ { 'a':1 , 'b':2 , 'c':3 }, { 'd':4 , 'e':5 , 'f':6 } ]
```

### Solution

* 주어진 list에서 dictionary를 읽어와 datatype을 변경하여 새로운 list로 만들기

```python
[dict([k, int(v)] for k, v in d.items()) for d in lst]
```

* 주어진 list에서 dictionaray의 값의 datatype을 변경하기

```python
for d in lst:
  for k in d:
    d[k] = int(d[k])

for d in lst:
  d.update((k, int(v)) for k, v in d.items()
```

### Reference
- https://stackoverflow.com/questions/5316720/how-to-convert-string-values-from-a-dictionary-into-int-float-datatypes
