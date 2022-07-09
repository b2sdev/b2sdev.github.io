---
title: "[SQL] 각 카테고리의 최상위 레코드 추출하기"
date: 2022-07-06 23:00:00 +0900
categories: blog
tags: sql
---

데이터에서 category별로 score가 가장 높은 최상위의 레코드를 추출해보자.

아래와 같이 인기 상품의 상품 ID와 카테고리, 점수 정보를 가진 테이블이 있다. 

```
product_id|category|score|
----------+--------+-----+
A001      |action  |   94|
A002      |action  |   81|
A003      |action  |   78|
A004      |action  |   64|
D001      |drama   |   90|
D002      |drama   |   82|
D003      |drama   |   78|
D004      |drama   |   58|
```

기대하는 결과는 아래와 같다.

```
category|product_id|
--------+----------+
drama   |D001      |
action  |A001      |
```

[카테고리별 상위 n개의 레코드를 추출](/blog/2022-07-06-sql-top-n-for-each-category)할 때는 서브 쿼리를 사용했으나, **최상위 상품만을 추출**하는 경우는 FIRST_VALUE 윈도 함수를 사용하고 SELECT DISTINCT 구문으로 결과를 집약한다.

```sql
-- (2) DISTINCT 구문을 사용해 중복 제거   
SELECT DISTINCT
category
    -- (1) category별로 최상위 score의 product_id 추출
  , FIRST_VALUE(product_id)
      OVER(PARTITION BY category ORDER BY score DESC
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)
    AS product_id
FROM popular_products
;
```

다만, Hive에서는 DISTINCT 부분이 동작하지 않아 사용할 수 없다.

### Reference

가사키 나카토, 다미야 나오토, <<데이터 분석을 위한 SQL 레시피>>, p105
