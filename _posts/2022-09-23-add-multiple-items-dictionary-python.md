---
title: "[Python] Dictionary에 새로운 item들을 추가"
date: 2022-09-23 23:15:00 +0900
categories: dev python
tags: Python
---

### Problem

주어진 dictionary에 새로운 item들을 추가하고 싶다.

```python
# AS-IS
myDict = {'one': 'Sally', 'two': 13, 'three': 'Dingra', 'four': 'Lilop'};

# TO-BE
myDict = {'one': 'Sally', 'two': 13, 'three': 'Dingra', 'four': 'Lilop', 'five': 'Olive', 'six': 'Nivo', 'seven': 'Xavier'}
```

### Solution

Dictionary의 update 함수를 사용한다.

```python
myDict.update({'five':'Olive','six':'Nivo','seven':'Xavier'});
```

### Reference
- https://tutorialdeep.com/knowhow/add-multiple-items-dictionary-python/
