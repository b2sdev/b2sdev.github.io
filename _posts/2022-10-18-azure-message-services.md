---
title: "애저 메시지 서비스 비교"
date: 2022-10-18 22:20:00 +0900
categories: dev architecture
tags: msa azure cloud architecture
---

Azure의 메시지 서비스를 비교해보고 목적에 맞는 서비스를 이용해보자.

### Azure Event Grid
- 이벤트-드리븐, 반응형 프로그래밍을 가능하게 하는 이벤트 백플레인 
- 게시-구독(publish-subscribe) 모델을 사용 
- 게시자는 이벤트를 내보내지만 이벤트가 처리되는 방식에 대한 기대치는 없음 
- 구독자는 어떤 이벤트를 처리할지 결정 

### Azure Event Hubs 
- 빅 데이터 스트리밍 플랫폼 및 이벤트 수집 서비스 
- 초당 수백만 개의 이벤트를 수신하고 처리 가능 
- 실시간 처리를 위한 신속한 데이터 검색과 저장된 원시 데이터의 반복 재생이 가능 

### Azure Service Bus 
- 메시지 큐와 게시-구독 토픽이 있는 메시지 브로커
- 트랜잭션, 순서 지정, 중복 검색 및 즉각적인 일관성이 필요한 엔터프라이즈 애플리케이션을 대상으로 함 
- 손실되거나 중복될 수 없는 높은 가치의 메시지를 처리할 때 사용 
- 소비 주체에서 메시지를 받을 준비가 될때가지 메시지를 브로커(큐)에 저장

### Summary

서비스 |용도 |Type |사용 시기
----|-----|------|-----
Event Grid |반응형 프로그래밍 |이벤트 배포(불연속) |상태 변경에 대응 
Event Hubs | 빅 데이터 파이프라인  | 이벤트 스트리밍(시리즈) | 원격 분석 및 분산 데이터 스트리밍 
Service Bus | 높은 가치의 엔터프라이즈 메시지 | 메시지  | 주문 처리 및 금융 거래 

### Reference
- https://learn.microsoft.com/ko-kr/azure/event-grid/compare-messaging-services
