---
title: Layered Architecture
tags:
  - Model-view-controller
  - Layered Architecture
categories:
  - Web Architecture
date: 2020-10-03 11:31:23
---


## Background: MVC model

> Model-view-controller (usually known as MVC) is a software design pattern
> commonly used for developing user interfaces that divides the related program logic
> into three interconnected elements.

### Components

![MVC Process](/images/MVC-Process-crop.pdf)

#### Model

> The central component of the pattern. It is the application's dynamic data structure,
> independent of the user interface. It directly manages the data, logic and rules of the application.

#### View

> Any representation of information such as chart, diagram or table.
> Mulitple view of the same informantion are possible,
> such as a bar chart for management and tabular view for accountants.

#### Controller

> Accepts input and converts it to commands for the model or view
>
> - The model is responsible for managing the data of the application. It receives user input from the controller.
> - The view means presentation of the model in a particular format.
> - The controller responds to the user input and performs interactions on the data model objects.
> The controller receives the input, optionally validates it and then passes the input to the model.

웹 페이지는 중복 개발되는 요소가 존재한다.

Controller에서 중복으로 호출 되는 부분을 처리하기 위해서

- 별도의 객체로 분리한다.
- 별도의 메소드로 분리한다.

## Controller와 Service

Business method를 별도의 Service 객체에서 구현하도록 하고 컨트롤러는 Service객체를 사용하도록 한다.

### Service 객체

> Between the controller and the model, a layer called a _service_ is sometimes interposed.
> It fetches data from the model and lets the controller use the fetched data.
> This layer allows a cleaner separation of data storage (model), data fetching (service) and data manipulation (controller).
> Since this layer is not part of the original MVC concept, it is optional,
> but can be useful for code management and reusability purposes in some cases.

Business logic을 수행하는 method를 가지고 있는 객체를 서비스 객체라고 한다.
보통 하나의 Business logic은 하나의 _transaction_ 으로 동작한다.

### Transaction

1. 원자성 (Atomicity)
모든 과정이 성공했을 때만 정보를 반영한다

2. 일관성 (Consistency)
Transaction 작업 처리 결과가 항상 일관성이 있어야 한다. 처음에 참조한 데이터로 transaction이 진행 되어야 한다. 각 사용자가 일관된 데이터를 볼 수 있다.

3. 독립성 (Isolation)
독립성은 둘 이상의 Transaction이 동시에 병행 실행되고 있을 경우, 어느 하나의 transaction이라도 다른 transaction의 연산에 끼어들 수 없다.

4. 지속성 (Durability)
지속성은 transaction이 성공적으로 완료되었을 경우, 결과는 영구적으로 반영되어야 한다는 점 입니다.
