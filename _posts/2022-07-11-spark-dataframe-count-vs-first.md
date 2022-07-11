---
title: "[Spark] df.count()를 함부로 사용하지 않기!"
date: 2022-07-11 22:40:00 +0900
categories: blog
tags: spark
---

Large-scale 데이터를 다루기 위해 Pandas에서 (Py)Spark의 세계로 넘어오며, Pandas에서 dataframe의 인식을 그대로 Spark에 적용시켰다가 엄청난 성능 저하를 발생시키는 케이스가 여럿 된다. 그 중의 하나는 dataframe의 row의 개수를 반환하는 `DataFrame.count()` 함수의 오용이다.

데이터를 로딩하여 dataframe으로 변환했다던가 dataframe에 필터를 적용하여 조건에 부합하는 row들만 남긴 후, dataframe의 size(row들의 개수)를 확인하여 어떤 처리를 하고 싶다고 해보자. 많은 경우에 있어서 아래와 같은 코드를 쉽게 만나볼 수 있다.

```python
if df.count() > 0:
    do_something()
```

위의 코드는 논리적으로는 문제가 될만한 포인트가 전혀 없다. 그러나, Spark 애플리케이션이 실행될 때 바로 이 지점에서 엄청난 병목이 발생한다.

DataFrame.count() 함수는 **action**이다. Action 중에서도 매우 값비싼 action이다. 왜냐하면, 분산되어 있는 RDD의 row들의 수를 모두 세어 그것을 합산한 결과를 반환하기 때문이다. 

위의 코드에서 count() 함수 대신 DataFrame.first() 함수를 사용하면 성능을 크게 향상시킬 수 있다.

```python
if df.first():
    do_something()
```

Spark version 3.3.0 이후 버전에서는 DataFrame.isEmpty() 함수를 사용할 수도 있다.

```python
if not df.isEmpty():
    do_something()
```

Spark SQL의 DataFrame API는 여러 노드에서 분산으로 처리되는 Spark RDD를 단일한(single) dataframe처럼 인식하고 쉽게 사용할 수 있게끔 마법을 부려 놓았다. 이게 양날의 검과 같은데, Spark의 진입장벽을 크게 낮춤과 동시에, 분산 처리에 대한 인식의 수준까지 낮추어버렸다. 

df.count() 함수는 모든 RDD의 row들의 개수를 반환하는 매우 값비싼 연산이다. 운영 환경에 배포되는 코드에서는 절대로 사용하지 말고, df.first() 또는 df.isEmpty()로 대체하도록 하자.