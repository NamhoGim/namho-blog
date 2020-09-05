---
title: Java DAO & DTO
tags:
  - Java
  - Data Access Object
  - Data Transfer Object
date: 2020-09-02 13:03:34
category:
- [Java, Design Pattern]
---


## DAO(Data Access Object)

- 실제로 DB에 접근 하는 객체
  - Persistance Layer(DB에 data를 CRUD하는 계층) 이다.
- Service와 DB를 연결하는 고리의 역할을 한다.
- SQL를 사용(개발자가 직접 코딩)하여 DB에 접근한 후 적절한 CRUD API를 제공한다.

## DTO(Data Transfer Object)

- 계층간 데이터 교환을 위한 객체(Java Beans)이다.
  - DB에서 데이터를 얻어 Service나 Controller 등으로 보낼 때 사용하는 객체를 말한다.
  - DB의 데이터가 Presentiation Logic Tier로 넘어오게 될 때 DTO의 구조로 바뀌게 된다.
  - Logic을 갖지 않는 순수한 데이터 객체, getter/setter method만을 갖는다.
  - DB에서 꺼낸 값을 변경할 필요가 없기 때문에 DTO class에는 setter가 없다.
