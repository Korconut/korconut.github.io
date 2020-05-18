---
layout: post-write
title:  "SPRING - Container"
study: true
---

# Spring - Container

## 개요

## Core Container

### Bean - spring-context
 프로젝트시 대표적인 반복작업인 new 메소드를 통한 메모리 할당이다.
 Spring에서는 해당 작업을 Bean을 통해 도와준다.

 `그리고 Bean을 spring-context에서 다루고 있다.`

 아마도 메모리 할당을 위한 클래스를 만들 때 다음과 같은 약속은 당연한것 같다.
 - 자유도가 넓어야 한다. 즉, 기본 생성자를 가지고 있어야 한다.
 - 보안 측면에서 필드는 private로 선언.
 - 필드가 private이기 떄문에 getter, setter 메소드는 필수.

 그렇기 때문에 위와 같은 약속이 Bean의 조건인건 자연스럽다. 위와 같은 조건을 만족한 Class를 Bean Class로 사용할수 있다. 
 
 추가로 Bean에서 아마도 확장성의 이유로 각각의 getter, setter를 프로퍼티라고 약속하고 관리한다. (ex setName => name이란 name을 가진 프로퍼티)

 Bean Class로 사용할수 있는 Class를 사용하기 위해선 '사용하겠다!'라고 선언(Bean 선언)을 해야한다. 
 그리고 Spring은 Bean 선언을 확인하고 getBean()을 통해 메모리 할당을 한다. 
 할당은 SingleTon. 즉, 여러번 getBean()을 하더라도 결국 인스턴스는 하나인 방식이다.
 
 선언하는 방법은 여러 방법이 있다.

- Xml에 선언
 Bean 선언을 Xml에 선언하여 따로 관리한다. 보통 src/main/resources에 applicationContext.xml로 관리한다. 이때, Spring:ApplicationContext가 Spring:ClassPathXmlApplicationContext 에서 'xml파일 이름'을 받아 src파일 내에 받은 xml 파일 이름에 xml 파일을 찾아 작성된 선언을 확인한다.
 프로퍼티를 활용하여 Bean Class를 객체로 가진 Bean Class를 다룰수도 있다.

- Class에 선언 (annotation 이용)
 Class에 선언하여 따로 관리한다. '@annotation'과 같이 Spring에서는 단순 Bean 선언과 더불어 더 많은 annotation을 지원하여 각 annotation에 맞는 다양한 동작을 도와준다. 
 
  - @configuration : Xml을 Class로 가져온 것이라고 할수 있는데, 선언문을 가진 class임을 나타낸다. 보통 configuration이기 때문에 config라는 패키지에 각 목적에 맞는 Config.class로 관리한다. Main config는 ApplicationConfig이때, Spring:ApplicationContext가 Spring:AnnotationConfigApplicationContext 에서 (이름.class) 또는 ("이름") 을 받아 해당 클래스에 annotation이 있는지 확인하고 선언을 다룬다. 클래스를 가지고 오기 때문에 method이름이 달라서 생기는 error를 걱정할 필요가 없다.

  - @Bean : Bean Class를 메모리 할당을 해주는 method임을 나타낸다. @configuration클레스에서 실제 메모리 할당하는 작업을 작성한다.

  - @Import : 두개 이상의 Configuration을 나눠서 다루고자 할때 사용한다. import할 클래스 이름을 받아 해당 클래스의 Configuration을 가져다 쓴다.

  - @ComponenetScan : annotation 선헌 할때 root를 받아(보통 프로젝트 전체) root에 속해있는 Class 중 ComponenetScan이 Scan할수 있는 annotation을 찾고 찾은 Class에서 각 annotation에 맞는 동작을 한다.
    - @Component : Bean Class로 사용안 Class안에 있으며 Bean Class임을 나타낸다.
    - @Autowired : Bean Class에서 다른 Bean 객체를 받게 될 경우 자동으로 해주도록 나타낸다.
  
 

 


 


 


  


 
