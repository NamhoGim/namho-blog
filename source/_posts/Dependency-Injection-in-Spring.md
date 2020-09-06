---
title: Dependency Injection in Spring
tags:
  - Spring
  - Dependency Injection
date: 2020-09-06 23:31:52
---


## Container

Application 코드를 작성할때, 특정 기능이 필요하면 Library를 호출하여 사용한다. 프로그램의 흐름을 제어아하는 주체가 Application code이다. Framework 기반의 개발에서는 Framework 자신이 흐름을 제어하는 주체가 되어, 필요할 때마다 Application 코드를 호출하여 사용한다.

컨테이너는 instance의 생명주기를 관리하며, 생성된 instance에게 추가적인 기능을 제공합니다.
예를 들어, Servlet을 실행해주는 WAS는 Servlet Contatiner를 가지고 있다고 말합니다.
WAS는 Web browser로부터 Servlet URL에 해당하는 요청을 받으면, Servlet을 메모리에 올린 후 실행 합니다.
개발자가 Servlet Class를 작성했지만 실제로 메모리에 올리고 실행하는 것은 WAS가 가지고 있는 Servlet Container 입니다.
Servlet Container는 동일한 Servlet에 해당하는 요청을 받으면, 또 메모리에 올리지 않고 기존에 메모리에 올라간 Servlet을 실행하여 그 결과를 Web browser에게 전달 합니다.
Container는 보통 instance의 생명 주기를 관리하며, 생성된 instance들에게 추가적인 기능을 제공하는 것을 말합니다.

## DI(Dependency Injection)

DI는 의존성 주입이라는 뜻을 가지고 있다. Class사이의 의존 관계를 Bean 설정 정보를 바탕으로 Container가 자동으로 연결 해 주는 것을 말한다.
개발자들은 제어를 담당할 필요 없이 Bean 설정 파일에 의존관계가 필요하다는 정보만 추가해주면 된다.
Object reference를 Container로부터 주입 받아서, 실행 시에 동적으로 의존 관계가 생성된다.
Container가 흐름의 주체가 되어서 Application code에 의존관계를 주입해주는 것이 된다.
이것을 *eInversion of Control** 이라 부른다.

## Spring Framework에서 DI 적용 예 2가지

### xml 파일을 이용한 설정

- src/main 아래에 UserBean class 작성

Bean class의 몇가지 특성

1. 기본 생성자를 가지고 있다.
2. 필드는 private하게 선언한다.
3. getter, setter method를 가진다. getName(), setName() method를 name property라고 한다.

```java
package kr.or.connect.diexam01;

//빈클래스
public class UserBean {

  //필드는 private한다.
  private String name;
  private int age;
  private boolean male;

  //기본생성자를 반드시 가지고 있어야 한다.
  public UserBean() {
  }

  public UserBean(String name, int age, boolean male) {
    this.name = name;
    this.age = age;
    this.male = male;
  }

  // setter, getter메소드는 프로퍼티라고 한다.
  public void setName(String name) {
    this.name = name;
  }

  public String getName() {
    return name;
  }

  public int getAge() {
    return age;
  }

  public void setAge(int age) {
    this.age = age;
  }

  public boolean isMale() {
    return male;
  }

  public void setMale(boolean male) {
    this.male = male;
  }
}
```

- src/main/reource folder에 applicationContext.xml을 생성

UserBean class 의 instance 를 userBean이라는 이름으로 생성 할 수 있도록 xml을 작성 합니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

  <bean id="userBean" class="kr.or.connect.diexam01.UserBean"></bean>
</beans>
```

- ApplicationContext를 이용해서 설정파일을 읽어들여서 실행

```java
package kr.or.connect.diexam01;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class ApplicationContextExam01 {

  public static void main(String[] args) {
    ApplicationContext ac = new ClassPathXmlApplicationContext(
        "classpath:applicationContext.xml");
    System.out.println("초기화 완료.");

    UserBean userBean = (UserBean)ac.getBean("userBean");
    userBean.setName("kim");
    System.out.println(userBean.getName());

    UserBean userBean2 = (UserBean)ac.getBean("userBean");
    if(userBean == userBean2) {
      System.out.println("같은 인스턴스이다.");
    }
  }
}
```

#### xml을 이용한 설정으로 DI 확인

Car의 Engine class 작성

```java
package kr.or.connect.diexam01;

public class Engine {
  public Engine() {
    System.out.println("Engine 생성자");
  }

  public void exec() {
    System.out.println("엔진이 동작합니다.");
  }
}
```

Car class 작성

```java
package kr.or.connect.diexam01;

public class Car {
  Engine v8;

  public Car() {
    System.out.println("Car 생성자");
  }

  public void setEngine(Engine e) {
    this.v8 = e;
  }

  public void run() {
    System.out.println("엔진을 이용하여 달립니다.");
    v8.exec();
  }
}
```

Spring container에게 engine 객체의 injection을 맡기기 위해 다음과 같이 xml에 작성합니다.

```xml
<bean id="e" class="kr.or.connect.diexam01.Engine"></bean>
<bean id="car" class="kr.or.connect.diexam01.Car">
  <property name="engine" ref="e"></property>
</bean>
```

위의 xml파일을 읽어들여서 작동하게끔 작성하면 다음과 같습니다.

```java
package kr.or.connect.diexam01;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class ApplicationContextExam02 {

  public static void main(String[] args) {
    ApplicationContext ac = new ClassPathXmlApplicationContext(
        "classpath:applicationContext.xml");

    Car car = (Car)ac.getBean("car");
    car.run();
  }
}
```

### Java config를 이용한 설정

- Annotaions
  - @Configuration
    - 스프링 설정 클래스를 선언하는 어노테이션
  - @Bean
    - bean을 정의하는 어노테이션
  - @ComponentScan
    - @Controller, @Service, @Repository, @Component 어노테이션이 붙은 클래스를 찾아 컨테이너에 등록
  - @Component
    - 컴포넌트 스캔의 대상이 되는 애노테이션 중 하나로써 주로 유틸, 기타 지원 클래스에 붙이는 어노테이션
  - @Autowired
    - 주입 대상이되는 bean을 컨테이너에 찾아 주입하는 어노테이션

Java Config 설정

```java
package kr.or.connect.diexam01;
import org.springframework.context.annotation.*;

@Configuration
public class ApplicationConfig {
  @Bean
  public Car car(Engine e) {
    Car c = new Car();
    c.setEngine(e);
    return c;
  }

  @Bean
  public Engine engine() {
    return new Engine();
  }
}
```

@Configuration 은 스프링 설정 클래스라는 의미를 가집니다.
JavaConfig로 설정을 할 클래스 위에는 @Configuration가 붙어 있어야 합니다.
ApplicationContext중에서 AnnotationConfigApplicationContext는 JavaConfig클래스를 읽어들여 IoC와 DI를 적용하게 됩니다.
이때 설정파일 중에 @Bean이 붙어 있는 메소드들을 AnnotationConfigApplicationContext는 자동으로 실행하여 그 결과로 리턴하는 객체들을 기본적으로 싱글턴으로 관리를 하게 됩니다.

DI 적용 확인

```java
package kr.or.connect.diexam01;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class ApplicationContextExam03 {

  public static void main(String[] args) {
    ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);

    Car car = (Car)ac.getBean("car"); // Car.class로도 가능
    car.run();

  }
}
```

### Annotation만을 활용한 ApplicationConfig 설정

```java
package kr.or.connect.diexam01;
import org.springframework.context.annotation.*;

@Configuration
@ComponentScan("kr.or.connect.diexam01")
public class ApplicationConfig2 {
}
```

@ComponentScan이라는 annotation 추가
@ComponentScan annotation은 parameter로 들어온 package 이하에서 @Controller, @service, @Repository, @Component annotation이 붙어있는 class를 찾아서 메모리에 몽따 올린다.

기존에 작성한 Car class에 @Component annotation 추가

```java
package kr.or.connect.diexam01;

import org.springframework.stereotype.Component;

@Component
public class Engine {
  public Engine() {
    System.out.println("Engine 생성자");
  }

  public void exec() {
    System.out.println("엔진이 동작합니다.");
  }
}
```

DI 가 필요한 부분에는 @Autowired annotation 적용

```java
package kr.or.connect.diexam01;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class Car {
  @Autowired
  private Engine v8;

  public Car() {
    System.out.println("Car 생성자");
  }

  public void run() {
    System.out.println("엔진을 이용하여 달립니다.");
    v8.exec();
  }
}
```

DI 확인

```java
package kr.or.connect.diexam01;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class ApplicationContextExam04 {

  public static void main(String[] args) {
    ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig2.class);

    Car car = ac.getBean(Car.class);
    car.run();

  }
}
```
