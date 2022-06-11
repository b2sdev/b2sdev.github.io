---
title: "재귀 호출에 대해 다시 생각하기"
date: 2022-03-16 05:00:00 +0900
categories: blog
tags: spark databricks dbconnect
---

> 컴퓨터 관련 서적의 문제점 중 하나가 재귀 호출의 예를 잘못 든다는 점이다. 전형적인 예가 팩토리얼이나 피보나치 수열을 계산하는 것이다. - 스티브 맥코넬, <<코드 컴플리트 2>>

<br/>

# 들어가기

재귀 호출은 팩토리얼이나 피보나치 수열에서 보여주는 것보다 훨씬 더 강력한 도구이다. 여기에서는 내포(nesting) 형태의 데이터 구조를 다루어보며, 재귀 호출이라는 프로그램 기법이 왜 태어났고 계속 사용되는 것인지 고찰해보자.

<br/>

# 내포 구조를 다루는 방법

예를 들어, [1, 2, [3, 4], 5]라는 리스트를 생각해보자. 이것은 [1, 2, ?, 5]라는 리스트의 ? 부분에 [3, 4]라는 별도의 리스트가 내포되어 있다. 이 내포 리스트에 있는 숫자를 전부 합산하고 싶을 때는 어떻게 하면 될까?

다음의 Python 코드에서는 for 문으로 리스트에서 하나씩 꺼내, 그것이 정수이면 합산해가는 처리를 하고 있다.

```Python
def total(xs):
  result = 0
  for x in xs:
    if is_integer(x):
      # x가 정수이면 더한다
      result += x
    else:
      # x가 정수가 아니면 무엇을 할까?
  return result
```
*  is_integer라는 함수는 Python에서는 존재하지 않고, isinstance(x, int)라는 형태로 사용된다. 주제에서 벗어남으로 알기 쉬운 이름의 함수를 사용하고 있다.

우선 '1'과 '2'가 나오면 정수이기 때문에 result에 더한다. 여기까진 간단하지만 다음에 정수가 아닌 [3, 4]가 나온다. 이것은 어떻게 하면 좋을까?

<br/>

# for로 구현할 수 없다

'추가적인 for 문을 써서 안에 있는 값을 별도 합산하면 되지 않나?'라고 생각하는 사람도 있을지 모르겠다. 이번 입력 데이터의 경우에 한해선, 그 방법도 잘 동작한다. 하지만 3중, 4중 내포 관계가 되면 처리할 수 없다.

```Python
def total(xs):
  result = 0
  for x in xs:
    if is_integer(x):
      # x가 정수이면 더한다
      result += x
    else:
      # x가 리스트이기 때문에 for 문으로 돌린다.
      for y in x:
        if is_integer(y):
          result += y
        else:
          # 추가로 리스트나 나오면 어떻게 하면 좋지?
  return result
```

<br/>

# 재귀 호출을 사용

그래서 존재하는 것이 재귀 호출이다. 재귀 호출을 사용해서 '내포된 리스트 안에 있는 수치의 합을 계산하는 함수 total'을 완성해보자.

```Python
def total(xs):
  result = 0
  for x in xs:
    if is_integer(x):
      result += x
    else:
      # x는 내포 리스트이기 때문에 total로 안에 든 값을 합산한다!
      result += total(x)
  return result
```

이것으로 완성이다. 함수 total은 몇 중으로 된 내포 리스트가 전달되어도 안에 있는 모든 수를 합산할 수 있도록 되었다.

```python
a = [1, [2, 3], [4, [5, 6]], [7, 8], 9]
print(total(a))  # 45
```

<br/>

# 연습 문제

동일한 컨셉으로 내포 구조의 리스트를 평평하게 펴는 flatten 함수를 작성해보자.

```python
a = [1, [2, 3], [4, [5, 6]], [7, 8], 9]
print(flatten(a))  # [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

<details>
<summary>접기/펼치기</summary>

```python
is_list = lambda x: isinstance(x, list)

def flatten(xs):
  result = []
  for x in xs:
    if is_list(x):
      result += flatten(x)
    else:
      result += [x]
  return result
```
</details>

<br/>

# 정리

재귀 호출이란 함수 X 안에서 함수 X 자신을 호출하는 것이다. 어떠한 처리를 재귀 호출을 사용해 만들면 매우 편하게 구현할 수 있다. 특히 내포된 형태의 데이터 구조를 다룰 때 재귀 호출은 매우 유용하다.

<br/>

# 참고

나시오 히로카즈, <<코딩을 지탱하는 기술>>