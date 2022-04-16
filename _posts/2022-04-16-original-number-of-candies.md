---
title: "[초등수학] 전체 사탕의 개수"
date: 2022-04-16 12:00:00 +0900
categories: blog ps
tags: 문제해결 ps 알고리즘 algorithm 초등수학 수학
---

초등학교 6학년 수학 문제를 프로그래밍하여 풀어본다.

# 문제
지율이가 전체 사탕의 2/9를 먹고, 지율이가 먹고 남은 사탕의 3/7을 어머니가 먹었습니다. 저녁 때 퇴근한 아버지가 지율이가 어머니가 먹고 남은 사탕의 9/20를 먹었더니, 사탕이 66개 남았습니다. 전체 사탕은 몇 개입니까?

# 전략
* 구체적인 수가 주어진 건 가장 마지막에 남은 66개라는 사탕 개수이다. 따라서 아버지에서부터 지율이가 먹기 전의 상황까지 역추적해야 한다.
* 프로그램에서는 reduce 연산을 활용한다.

# 풀이
```python
from functools import reduce
from fractions import Fraction
from typing import List

def solve(taken: List[Fraction], left: int) -> int:
    def candies(f, t):
        """
        남은 사탕의 개수와 먹은 비율이 주어질 때,
        먹기 전 사탕의 개수를 리턴하는 함수

        param f: 먹은 사탕의 비율
        param t: f만큼 먹고 남은 사탕의 개수
        return: 먹기 전 사탕의 개수
        """
        return t // (1 - f)

    return reduce(lambda acc, cur: candies(cur, acc), taken, left)


print(solve([Fraction(9, 20), Fraction(3, 7), Fraction(2, 9)], 66))
```

# 참고
* 류승재, 초등수학 심화 공부법, 블루무스, p422
* https://codepractice.tistory.com/66
