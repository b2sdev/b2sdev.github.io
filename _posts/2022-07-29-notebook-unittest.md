---
title: "[Python][Databricks] 노트북에서 unittest 사용하기"
date: 2022-07-29 22:40:00 +0900
categories: blog
tags: spark databricks python
---

Jupyter 노트북이나 Databricks 노트북에서도 Python의 unittest 모듈을 활용하여 단위 테스트를 할 수 있다. 노트북의 cell에 정의된 함수를 테스트해보자.

```python
def add(a, b):
    return a + b
```

노트북의 하단에 테스트를 작성한다.

```python
import unittest

class TestNotebook(unittest.TestCase):
    
    def test_add(self):
        self.assertEqual(add(2, 3), 5)
```

테스트는 아래의 코드로 실행한다.

```python
unittest.main(argv=[''], verbosity=2, exit=False)
```

### Reference
- https://stackoverflow.com/questions/40172281/unit-tests-for-functions-in-a-jupyter-notebook