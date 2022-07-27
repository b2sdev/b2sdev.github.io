---
title: "[Dash] Multi-Page Apps and URL Support"
date: 2022-07-26 22:30:00 +0900
categories: blog
tags: plotly dash dashboard 대시보드
---

Dash에서 [`Dash Pages`](https://dash.plotly.com/urls) 기능을 활용하면 멀티 페이지 앱을 손쉽게 개발할 수 있다.

사이드잡 프로젝트로 실무에 필요한 대시보드를 [Dash](https://dash.plotly.com/)로 개발하여 운영하고 있다. 운영하는 서비스의 모니터링 목적으로 단일(single) 페이지에 테이블 형태의 정보를 보여 주는 것으로 개발을 시작했다. 시간이 지나면서 다양한 표와 차트가 추가되어 더 이상 single 페이지로 서빙할 수가 없어, 그룹으로 묶을 수 있는 정보는 [tab](https://dash.plotly.com/dash-core-components/tab)을 선택하면 표시하도록 개선을 했다. 

Tab의 사용으로 주제별로 시각화를 할 수 있어서 나름대로 적절한 효과를 볼 수 있었다. 그렇지만, 다수의 장문의 SQL 문과 callback 함수를 하나의 파일로 다루다보니, 추가로 요구되는 신규 기능을 지속적으로 적용하며 확장하기가 쉽지 않았다. 새롭게 요구되는 chart들은 workaround로 새로운 single-page 앱을 만들고 다른 port로 서빙하는 상태에까지 이르게 되었다.

이런 상황으로 인해 언젠가부터 multi-page 앱을 구상을 하고 있었다. 먼저 아래와 같이 간단하게 앱 구조를 잡았다. 그리고, 시간이 날 때마다 조금씩 tab으로 제공하던 table과 chart들을 분리하여 신규 페이지에 배치하는 작업을 해 두었다. 페이지 전환을 위한 밑작업도 일부 진행 후 공사 날짜를 기다리고 있었다.

오늘 드디어 full-time으로 대시보드를 손 볼 수 있는 시간이 났다. 다행히 Dash 2.5.0 버전부터 multi-page 손쉽게 다룰 수 있는 `Dash Pages` 기능이 추가된 것을 확인할 수 있었다. 페이지 전환을 위해 일부 적용한 navigation 기능을 걷어내고 바로 Dash 버전을 업데이트하여 페이지 라우팅 기능을 추가했다. 마지막으로, sidebar를 붙이고 chart와 table 배치 등의 layout을 조금 손보면서 고대하던 multi-page 앱 공사를 일단락지었다.

```
- app.py
- pages
   |-- board1.py
   |-- board2.py
   |-- board3.py
   |-- ...
```

앞으로 내가 선택할 수 있는 길이 몇 가지가 있다. 첫째는, 대시보드를 지속적으로 업그레이드하는 것이다. 대시보드의 사용자가 처음에는 개인과 프로젝트 멤버에 국한되었다면, 이제는 팀 밖의 이해관계자에게까지 확대되었다. 확대된 사용자에게 더 많은 정보를 제공하기 위해 지속적으로 업그레이드를 하고 싶다. 문제는 UX다. UX를 얼마나 빠르게 고수준으로 높일 것이냐가 대시보드 개선의 관건이 될 것 같다. 둘째는, tableau, Power BI, re:dash, Databricks SQL 등으로 얼른 갈아타는 것이다. Kibana를 포함한 대시보드 서비스를 여러 개 써보긴 했는데, 손쉽게 대시보드를 구성할 수 있다는 점에서 나쁘지 않은 선택이라고 생각한다. 다만, 데이터를 다루긴해도 명색이 개발자 출신인지라, 조금이나마 코드를 만지는 Dash를 아직까지는 선호하는 편이다. 셋째는, 내가 개발한 대시보드는 내리고 다른 목적으로 개발된 상위의 공식 대시보드에 편입하는 것이다. 상위 대시보드는 협력 부서에서 Grafana로 잘 만들어 서비스하고 있으니, 여기에 데이터를 제공함으로써 단일의 통합 대시보드 개발에 기여하는 것이다.

어떤 식으로든 내 자신에게, 함께 일하는 동료에게, 그리고 조직과 사용자에게 가치있는 방향으로 일이 진행되기를 바란다.

### Note

amazon.com에서 확인해보니 Plotly와 Dash를 소개하는 [Interactive Dashboards and Data Apps with Plotly and Dash](https://www.amazon.com/Interactive-Dashboards-Data-Apps-Plotly/dp/1800568916)라는 제목의 책이 작년(2021) 5월에 발행된 것으로 확인된다. 

Multi-page 앱을 위해 Dash에 추가된 소스 코드는 [Add pages for multi-page app #1947](https://github.com/plotly/dash/pull/1947)를 참고한다.