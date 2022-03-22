---
title: "[Databricks] Delta Lake"
date: 2022-03-23 05:40:00 +0900
categories: blog
tags: databricks delta_lake lakehouse
---

Databricks는 Data Lake와 Data Warehouse를 결합한 '델타 레이크(Delta Lake)'를 서비스하고 있다.

일반적인 데이터 파이프라인은 다양한 데이터 소스(source)에서 데이터를 수집하여 Data Lake에 보관하고, 특정 목적에 맞게 데이터를 가공(transformation)하여 Data Warehouse에 저장하여, 최종 데이터 사용자가 정제된 데이터를 소비할 수 있 하는 방식으로 구성된다.

Delta Lake를 활용하면 데이터의 처리와 소비를 하나의 공간에서 수행할 수 있다. 배치/스트리밍으로 수집한 데이터를  Delta Table로 변환 후 SQL로 쿼리하여 분석 및 2차 가공을 할 수 있으며, 자체 BI 도구로 바로 시각화도 할 수 있고, 3rd party로도 쉽게 데이터를 공유할 수 있다. 머신러닝 파이프라인과도 바로 붙여볼 수 있다.

이러한 사용자 시나리오가 가능한 수 있는 이유는, 무엇보다 - 기존 스파크에서는 커버하지 못했던 - 데이터에 대한 ACID 트랙잭션을 지원하기 때문이다. 더 나아가 스파크 위에서 대화형 쿼리 최적화 엔진을 제공함으로써, ETL, 스트림 처리는 물론 SQL 분석, MLflow 연동 기능까지 갖추었다. 데이터 레이크로는 기존의 HDFS, AWS S3, Azure Blob Storage 등을 그대로 사용하면 된다.

<figure>
  <img src='https://techcommunity.microsoft.com/t5/image/serverpage/image-id/244539i02EE86FA40EC52EF/image-dimensions/671x329?v=v2' alt='Lakehouse 원리 및 구성 요소'>
  <figcaption>Lakehouse 원리 및 구성 요소 [1]</figcaption>
</figure>

Delta Lake는 데이터 엔지니어/분석가/과학자가 하나의 공간으로 묶을 수는 큰 장점이 있기에, 이에 대한 니즈가 다분한 엔지니어링팀이라면 고려해 볼 만 한 서비스라고 생각한다.

[1] https://techcommunity.microsoft.com/t5/analytics-on-azure-blog/simplify-your-lakehouse-architecture-with-azure-databricks-delta/ba-p/2027272
