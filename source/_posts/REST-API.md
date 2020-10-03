---
title: REST API
tags:
  - REST
categories:
  - REST
  - REST API
date: 2020-10-03 11:30:57
---


## Background

2000년도에 Roy Fielding 박사의 학위논문에 REST라는 개념이 처음 등장
**REST**는 'Representational State Transfer'의 약자로 자원을 이름으로 구분 하여
해당 자원의 상태(정보)를 주고 받는 것을 의미한다.
Roy Fielding은 Web Architecture의 요구사항과 해결해야할 문제를 설명하고 이를 해결하기 위한
접근 방법을 논문에서 제시 하며, 이를 위한 아키텍처 스타일인 REST를 소개 합니다.

REST는 기본적으로 Web의 기존 기술과 HTTP protocol을 그대로 활용하기 때문에 Web의 장점을 최대한
활용할 수 있는 아키텍처 스타일이다.

### REST 구성

REST API는 다음과 같은 3가지 구분으로 구성 됩니다.

- 자원(Resource): 자원은 Data, Meta Data, HATEOAS
- 행위(Verb): HTTP Method로 표현됩니다.
- 표현(Representations)

### REST의 특징

1. Uniform Interface
구성 요소(Client, Server 등) 사이의 인터페이스는 균일(uniform) 해야한다.
인터페이스를 일반화함으로써, 전체 시스템의 아키텍처가 단순해지고, 상호 작용의 가시성이 개선되며,
구현과 서비스가 분리되므로 독립적인 진화가 가능해질 수 있다.

2. Stateless
클라이언트와 서버의 통신에는 상태가 없어야한다. 모든 요청은 필요한 모든 정보를 담고 있어야 한다.
요청 하나만 봐도 뭔지 알 수 있으므로 가시성이 개선되고, task 실패시 복원이 쉬우므로 신뢰성이 개선되며,
상태를 저장할 필요가 없으므로 규모 확장성이 개선될 수 있다.

3. Cacheable
캐시가 가능해야한다. 즉, 모든 서버 응답은 캐시가 가능한지 아닌지 알 수 있어야함
효율, 규모 확장성, 사용자 입장에서의 성능이 개선 된다.

4. Self-descriptiveness
REST의 또 다른 큰 특징 중 하나는 REST API 메시지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현 구조로 되어 있다는 것

5. Client - Server 구조
클라이언트 - 서버 스타일은 사용자 인터페이스에 대한 관심(concern)을 데이터 저장에 대한 관심으로 부터 분리
함으로써 클라이언트의 이식성과 서버의 규모 확장성을 개선할 수 있다.

6. Layered System
REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용할 수 있게 합니다.

### REST API 설계 가이드

REST API 설계시 가장 중요한 항목은 다음의 2가지로 요약 할 수 있다.
첫 번째, URI는 정보의 자원을 표현해야 한다.
두 번째, 자원에 대한 행위는 HTTP Method (GET, POST, PUT, DELETE)로 표현한다.

1. URI는 정보의 자원을 표현해야 한다.
2. 자원에 대한 행위는 HTTP Method (GET, POST, PUT, DELETE)로 표현 한다.
3. URI에 HTTP Method가 들어가면 안된다.
4. URI에 행위에 대한 동사 표현이 들어가면 안된다.
5. 경로 부분 중 변하는 부분은 유일한 값으로 대체 한다.
6. 슬래시 구분자(/)는 계층 관계를 나타내는데 사용한다.
7. URI 마지막 문제로 (/)를 포함하지 않는다.
8. URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며,
  URI가 다르다는 것은 리소스가 다르다는 것이고, 역으로 리소스가 다르면 URI도 달라져야 한다.
9. 하이픈(-)은 URI 가독성을 높이는데 사용할 수 있다.
10. under score는 URI에 사용하지 않는다.
11. URI 경로에는 소문자가 적합하다.
12. 파일 확장자는 URI에 포함하지 않는다 Accept header를 사용하도록 한다.
13. 리소스 간에 연관 관계가 있는 경우 다음과 같은 방법으로 표현 한다.
ex) GET: /books/{bookid}/viewers (일반적으로 소유 'has' 관계를 표현할 때)
14. 자원을 표현하는 Collection과 Document
Collection은 객체의 집합, Document는 객체라고 생각하면 된다. Collection과 Document모두 리소스로 표현 할 수 있으며 URI로 표현 할 수 있다.

-> REST API를 완벽하게 구현하지 못할 경우 Web API라 한다.
