---
title: "[Python] 클래스의 속성(attribute)를 한방에 설정하기"
date: 2022-07-21 22:05:00 +0900
categories: blog dev
tags: python 파이썬
---

Python에서 클래스 생성자에 전달된 키워드 인수를 속성(attribute)에 일괄 설정해보자.

파이썬 표준 라이브러리에서 발췌한 아래 예제를 살펴보자.

[`multiprocessing.Namespace`](http://bit.ly/1cPM2uJ)

```python
class Namespace(_AttributeHolder):
    def __init__(self, **kwargs):
        for name in kwargs:
            setattr(self, name, kwargs[name])
```

`setattr(object, name, value)` 함수는 Object가 허용하면 name 속성에 value를 할당한다. 이 메서드에 의해 새로운 속성이 생성되거나 기존 속성의 값이 변경된다.

[`argparse.Namespace`](http://bit.ly/1cPM4Ti)

```python
class Namespace(object):
    def __init__(self, **kwds):
        self.__dict__.update(kwds)
```

`__dict__`는 객체나 클래스의 쓰기가능(writable) 속성을 저장하는 매핑이다. __dict__를 직접 매핑형으로 설정하면, 그 객체의 속성 묶음(bunch)을 빠르게 정의할 수 있다.


```python
class Employee(object):
    def __init__(self, _dict):
        self.__dict__.update(_dict)

dict = {'name': 'Oscar', 'lastName': 'Reyes', 'age': 32}

e = Employee(dict)

assert e.name == 'Oscar'
assert e.age == 32
```

```python
class Person:
    def __init__(self, **kwargs):
        self.__dict__.update(kwargs)

dict = {'ssn': '444-44-4444', 'firstname': 'Alonzo', 'lastname': 'Church'}

person = Person(**dict)

assert person.ssn == '444-44-4444'
assert person.firstname == 'Alonzo'
```

### Reference
- <전문가를 위한 파이썬>, 19장. 동적 속성과 프로퍼티
- https://stackoverflow.com/questions/2466191/set-attributes-from-dictionary-in-python