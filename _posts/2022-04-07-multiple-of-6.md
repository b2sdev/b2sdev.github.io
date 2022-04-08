---
title: "[초등수학] 6의 배수"
date: 2022-04-07 22:30:00 +0900
categories: blog ps
tags: 문제해결 ps 알고리즘 algorithm 초등수학 재귀 recursion
---

딸아이가 푸는 초등학교 5학년 수학 문제집에 나오는 문제를 프로그래밍하여 풀어보았다.

# 문제
다음 5장의 수 카드 중에서 3장을 뽑아 세 자리 수를 만들려고 합니다.
6의 배수는 모두 몇 개 만들 수 있습니까?
> [2, 3, 4, 5, 7]

# 전략
* 6의 배수가 되려면 2의 배수이면서 3의 배수이어야 한다.
* 2의 배수는 일의 자리 숫자가 짝수이다.
* 3의 배수는 각 자리 숫자의 합이 3의 배수이다.

# 정답
* 8개 (342, 432, 372, 732, 234, 324, 354, 534)

# 코드
```python
from collections import deque

def check(cards, used):
    global ans
    # 카드를 3장 다 뽑았을 때, 각 자리의 숫자의 합이 3의 배수가 되는지 확인한다.
    if len(cards) == 3:
        if sum(cards) % 3 == 0:
            # print(list(cards))
            ans += 1
        return
    for card in a:
        if card in used:
            continue
        # 일의 자리의 숫자는 짝수가 되게 한다.
        if len(cards) == 0 and card % 2 != 0:
            continue
        cards.appendleft(card)
        used[card] = card
        check(cards, used)
        used.pop(card)
        cards.popleft()

a = [2, 3, 4, 5, 7]

ans = 0
check(deque([]), dict())
print(ans)
```

# 참고
* 디딤돌 최상위 초등수학 5-1 2단원 11번
* https://www.youtube.com/watch?v=4b6-7PLk3Cs
