---
title: "[plotly] Multi-Page Apps and URL Support"
date: 2022-07-26 22:30:00 +0900
categories: blog
tags: plotly dash
---

Dash에서 [`Dash Pages`](https://dash.plotly.com/urls) 기능을 활용하면 멀티 페이지 앱을 손쉽게 개발할 수 있다.

사이드잡 프로젝트로 실무에 필요한 대시보드를 [Dash](https://dash.plotly.com/)로 개발하여 운영하고 있다. 처음에는 단일(single) 페이지에 테이블 형태의 정보를 보여 주는 것으로 개발을 시작했다. 시간이 지나면서 다양한 표와 차트가 추가되어 더 이상 single 페이지로 서빙할 수가 없어, 그룹으로 묶을 수 있는 정보는 [tab](https://dash.plotly.com/dash-core-components/tab)을 선택하면 표시하도록 개선했다. Tab의 사용으로 시각화에서는 적절한 효과를 볼 수 있었다. 그렇지만, 다수의 장문의 SQL 문과 callback 함수를 하나의 파일로 다루다보니, 추가로 요구되는 신규 기능을 지속적으로 적용하며 확장하기가 쉽지 않았다. Workaround로 신규 chart들은 새로운 single 페이지 앱을 만들고 port를 다르게 하여 서빙하는 상태에까지 이르게 되었다.

이런 상황으로 인해 오래 전부터 멀티 페이지 앱을 구상을 하고 있었다. 먼저 아래와 같이 간단하게 앱 구조를 잡았다. 시간이 날 때마다 조금씩 하나의 파일에 있던 table과 chart를 주제별로 분리하여 각각의 페이지에 배치하는 작업을 해 두었다. 그리고, 오늘 드디어 페이지 전환까지 될 수 있도록 라우팅 기능까지 추가했다. Dash 2.5.0 버전부터 멀티 페이지를 손쉽게 다룰 수 있는 `Dash Pages` 기능이 추가되어, 페이지별로 URL 라우팅을 쉽게 적용할 수 있었다.

```
- app.py
- pages
   |-- board1.py
   |-- board2.py
   |-- board3.py
   |-- ...
```

대시보드 리팩터링이 좀 되고 나면 plotly 라이브러리의 코드 변경점에 대해서도 확인해 볼 생각이다.

Add pages for multi-page app #1947
https://github.com/plotly/dash/pull/1947

