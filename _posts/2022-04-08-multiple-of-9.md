---
title: "[초등수학] 9의 배수"
date: 2022-04-08 20:02:00 +0900
categories: blog ps
tags: 문제해결 ps 알고리즘 algorithm 초등수학 재귀 자기호출 recursion 완전탐색 brute-force
---

초등학교 5학년 수학 문제를 다양한 방식으로 프로그래밍하여 풀어 본다.

# 문제
다음과 같은 다섯 자리 수를 9의 배수가 되게 만들려고 합니다. 만들 수 있는 가장 큰 수와 가장 작은 수를 각각 구하시오.
  > 13■△6

# 전략
* 3의 배수는 각 자리 숫자의 합이 9의 배수이다.
* 즉, 1 + 3 + ■ + △ + 6 = 10 + ■ + △가 18 또는 27이어야 한다.

# 정답
* 가장 큰 수 : 13986
* 가장 작은 수 : 13086

# 코드 #1  
* 0~9의 숫자를 중복하여 두 번 선택하는 중복 순열 문제와 같다. 경우의 수가 100이므로 brute-force로 풀 수 있다.
```python
num = "13■△6"
s = 0
for c in num:
  if c.isdigit():
    s += int(c)
cand = []
for a in range(0, 10):
  for b in range(0, 10):
    if (s + a + b) % 9 == 0:
      cand.append((a, b))
print(cand[-1], cand[0])
```

# 코드 #2
* 찾아야 할 숫자가 두 개밖에 없으니, 최소값과 최대값을 바로 찾아내면 시간복잡도를 O(N)으로 줄일 수는 있다.
```python
mul9 = 9 # 9의 배수
# 최소값 찾기
for n in range(0, 10):
  while mul9 < s + a:
    mul9 += 9
  b = mul9 - s - a
  cand.append((a, b))
  break

mul9 = 9
# 최대값 찾기
for n in range(9, -1, -1):
  while mul9 < s + a:
    mul9 += 9
  b = mul9 - s - a
  cand.append((a, b))
  break

print(cand[-1], cand[0])
```

# 코드 #3
* 찾아야 하는 숫자가 두 개가 아니라 훨씬 더 많다면 위와 같은 방법으로는 해결할 수 없다. 재귀 호출을 적용해본다.
```python
def pick(picked, to_pick, target):
  """ 
  0~9의 숫자 중 중복해서 m개를 고르는 모든 조합 중
  가장 작은 숫자들로 이루어진 조합의 합이 
  target과 같으면 True를 리턴

  :param picked: 지금까지 선택한 숫자들
  :param to_pick: 더 고를 숫자의 수
  :param target: 고른 숫자들의 합의 목표값
  """
  if to_pick == 0:
    if sum(picked) == target:
      return True
    return False
  ret = False
  for next in range(0, 10):
    picked.append(next)
    if pick(picked, to_pick - 1, target):
      ret = True
      break
    picked.pop()
  return ret
  
num = "13■△6"

# Python에서 String은 immutable하여 replace가 되지 않는다.
# 찾은 숫자로 수를 만들기 위해 mutablie한 list로 변환 후,
# 찾은 값을 특정 index에 assign 후 수로 변환할 것이다.
nums = list(num)
partial_sum = 0 # 주어진 숫자들의 합
unknowns = [] # 찾아야 할 숫자들의 위치

for i, c in enumerate(nums):
  if c.isdigit():
    partial_sum += int(c)
  else:
    unknowns.append(i)

to_pick = len(unknowns) # 찾아야 할 숫자의 개수
cand = [] # 찾은 숫자 조합

# 모든 자리의 숫자들로 만들 수 있는 9의 배수들
for mul9 in range(9, partial_sum + (to_pick * 9) + 1, 9):
  if mul9 < partial_sum:
    continue
  target = mul9 - partial_sum
  picked = []
  if pick(picked, to_pick, target):
    # 가장 작은 숫자들로 만들 수 있는 조합을 찾음
    cand.append(picked)
    # 이 조합을 뒤집으면 가장 큰 수의 조합이 됨
    cand.append(picked[::-1])

# 찾은 숫자 조합 리스트를 역순으로 정렬
cand.sort(reverse=True)

# ■와 △를 찾은 숫자 조합의 숫자들로 변환 후
# 가장 큰 수와 가장 작은 수를 출력
for comb in (cand[0], cand[-1]):
  for i in range(len(comb)):
    nums[unknowns[i]] = str(comb[i])
  print(''.join(nums))
```

# 코드 #4
* 재귀가 아닌 Python의 itertools 라이브러리를 활용하여 구현할 수도 있다.
```python
import itertools

num = "13■△6"

nums = list(num)
partial_sum = 0
unknowns = []

for i, c in enumerate(nums):
  if c.isdigit():
    partial_sum += int(c)
  else:
    unknowns.append(i)

cand = []

mul9 = 9
while mul9 < partial_sum:
    mul9 += 9

while mul9 <= partial_sum + (len(unknowns) * 9):
  target = mul9 - partial_sum
  for comb in itertools.product(range(10), repeat=len(unknowns)):
    if sum(comb) == target:
      cand.append(list(comb))
      cand.append(list(comb)[::-1])
      break
  mul9 += 9
  
cand.sort(reverse=True)

for comb in (cand[0], cand[-1]):
  for i in range(len(comb)):
    nums[unknowns[i]] = str(comb[i])
  print(''.join(nums))
```

# 참고
* 디딤돌 최상위 초등수학 5-1 2단원 12번
* https://www.youtube.com/watch?v=G843vLq4sVU
