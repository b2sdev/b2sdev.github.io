---
title: "[ALGOSPOT] 도시락 데우기 (LUNCHBOX)"
date: 2022-04-02 12:00:00 +0900
categories: blog ps
tags: 종만북 알고스팟 algospot 문제해결 ps 알고리즘 algorithm 그리디 greedy 탐욕법
---

# 문제
* https://algospot.com/judge/problem/read/LUNCHBOX

# 전략
* 먹는 데 오래 걸리는 도시락부터 순차적으로 데워 먹는다.

```python
for _ in range(int(input())):
    n = int(input())
    m = list(map(int, input().split()))  # 데우는 데 걸리는 시간
    e = list(map(int, input().split()))  # 먹는 데 걸리는 시간

    # 먹는 데 걸리는 시간의 역순으로 정렬
    order = [(-e[i], i) for i in range(n)]
    order.sort()
    
    ans, beginEat = 0, 0
    for i in range(n):
        box = order[i][1]
        beginEat += m[box]
        ans = max(ans, beginEat + e[box])
        print(box, beginEat, ans)
    print(ans)
```
