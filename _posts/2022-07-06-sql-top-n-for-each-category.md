---
title: "[SQL] 각 카테고리의 상위 n개 레코드 추출하기"
date: 2022-07-06 22:40:00 +0900
categories: blog
tags: sql
---

데이터에서 category별로 score가 가장 높은 top 3의 레코드를 추출해보자.

인기 상품의 상품 ID와 카테고리, 점수 정보를 가진 인기 상품 테이블이 있다. 

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

기대하는 결과는 아래와 같다(오른쪽의 rank 컬럼은 참고를 위해 표현).

```
category|product_id|score|rank|
--------+----------+-----+----+
action  |A001      |   94|   1|
action  |A002      |   81|   2|
action  |A003      |   78|   3|
drama   |D001      |   90|   1|
drama   |D002      |   82|   2|
drama   |D003      |   78|   3|
```

결론적으로는, 윈도(window) 함수를 이용하여 category별로 높은 score 순서대로 순위를 붙이고, 이 결과를 서브 쿼리를 만들고 외부에서 WHERE 구문을 적용하여 top 3 결과를 추출한다.

```sql
SELECT *
FROM
  ( SELECT
        category
      , product_id
      , score
      , ROW_NUMBER()
          OVER(PARTITION BY category ORDER BY score DESC)
        AS rank
    FROM popular_products
  ) AS popular_products_with_rank
WHERE rank <= 3
ORDER BY category, rank
;
```

위의 쿼리에서 윈도 함수를 사용하여 순위를 추가한 서브 쿼리만의 결과는 아래와 같다.

```
category|product_id|score|rank|
--------+----------+-----+----+
action  |A001      |   94|   1|
action  |A002      |   81|   2|
action  |A003      |   78|   3|
action  |A004      |   64|   4|
drama   |D001      |   90|   1|
drama   |D002      |   82|   2|
drama   |D003      |   78|   3|
drama   |D004      |   58|   4|
```

여기에서 top 3 레코드를 추출하기 위해 외부 쿼리에서 rank를 활용해 레코드를 filtering한다.

### Reference

가사키 나카토, 다미야 나오토, <<데이터 분석을 위한 SQL 레시피>>, p104