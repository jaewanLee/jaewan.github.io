---
layout: post
title: 'Layered architecture'
author: jaewan
categories: [infra,architecture]
image: assets/images/layered_architecture.png
comments: false
---

# Layered architecture
계층화 패턴은 어플리케이션에 필요한 데이터 및 상태를 어떻게 유지하고 관리할것인가에 대해 계층을 나누어 분할하는 방식이다.  각 계층에는 명확한 역할이 존재하며, 각각의 계층간에는 독립성과 유연성이 보장되어야 한다. n티어 아키텍쳐 패턴인 만큼 각 하위 모듈들의 구조화는 다양하게 추상화가 가능하지만 주로 4개의 계층으로 나뉜다.

## Presentation Layer (View)
UI계층이라고도 하며, 사용자와 인접하여 사용자로부터 데이터를 받거나 출력해주는 계층이며, 컨트롤러와 상호작용 한다.

## Controller Layer (Controller)
사용자에게 요청을 받고 요청에 대한 유효성 검사 및 요처엥 대한 비지니스 로직을 결정하여 응답을 처리하는 계층이다. Exception과 error도 함게 처리된다. 주로 "What To do"를 결정한다.

## Business Logic Layer (Service)
controller Layer와 persistence Layer를 묶는 계층으로, 어플리케이션의 비지니스 로직 처리와 도메인 모델 적합성 검증 및 트랜잭션을 처리한다.

## Persistence Layer (DAO-data access object)
영구 데이터를 객체화 시키며, 데이터 저장,수정,삭제하는 계층이다. DB에 접근하여 CRUD를 담당한다.

## Domain Model Layer (DTO)
관계형 데이터의 엔티티개념으로 데이터 객체를 의미한다.

