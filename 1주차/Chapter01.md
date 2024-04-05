# ch01 스프링 부트란?

---

---

스프링은 자바 언어를 이용해 엔터프라이즈급 개발을 편리하게 만들어주는 ‘오픈소스 경량급 app 프레임워크’로 불리고 있습니다.

<aside>
❓ 엔터프라이즈급 개발이란 ?
- 기업 환경을 대상으로 하는 개발을 뜻함.
대규모 데ㅔ이터를 처리하는 환경을 엔터프라이즈 환경이라고도 함.

</aside>

### 스프링 핵심 가치

애플리케이션 개발에 필요한 기반을 제공해서 개발자가 로직 구현에만 집중할 수 있게끔 하는 것

---

## 1.1_1 IoC - 제어 역전

자바의 경우 객체를 사용하기 위해 아래와 같은 코드를 사용합니다.

```java
package com.wikibooks.chapter1.controller;

import com.wikibooks.chapter1.service.MyService;
import com.wikibooks.chapter1.service.MyServiceImpl;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class NoDIController {

    private MyService service = new MyServiceImpl();

    @GetMapping("/no-di/hello")
    public String getHello() {
        return service.getHello();
    }

}
```

즉, 사용하려는 객체를 선언하고 해당 객체의 의존성을 생성한 후 객체에게 제공하는 기능을 사용합니다.
객체를 생성하고 사용하는 일련의 작업을 개발자가 직접 제어하는 구조입니다.

하지만 IoC을 특징으로 하는 스프링은 기존 자바 개발 방식과 다르게 동작합니다.
IoC를 적용한 환경에서는 사용할 객체를 직접 생성하지 않고 객체의 생명주기 관리를 외부에 위임합니다.
이때, “외부”는 Spring Container 또는 IoC Container를 의미합니다.

객체의 관리를 컨테이너에 맡겨 제어권이 넘어간 것을 “제어 역전”이라고 부르며, 제어 역전을 통해 DI, AOP(관점 지향 프로그래밍)등이 가능해집니다.

이렇듯 스프링을 사용하면 객체의 제어권을 컨테이너로 넘기기 때문에 개발자는 비즈니스 로직을 작성하는 데 더 집중할 수 있습니다.

---

## 1.1_2 DI - 의존성 주입

제어 역전의 방법 중 하나로, 사용할 객체를 직접 생성하지 않고 외부 컨테이너가 생성한 객체를 주입받아 사용하는 방식입니다.

### 스프링에서 DI받는 3가지 방법 (꼭 알아두길 바랍니다)

1. **생성자**
2. **필드 객체 선언**
3. **setter 메서드**

spring에선 @Autowired라는 어노테이션을 통해 DI할 수 있습니다.

스프링 4.3 이후 버전은 생성자를 통해 DI할 때 @Autowired 생략할 수도 있습니다.
하지만 웬만해선 어노테이션을 생략하지 않고 사용할 예정입니다. (실력 향상을 위해)

- 생성자를 통한 의존성 주입

```java
@RestController
public class DIController{
	MyService myservice;
	
	@Autowired
	public DIController(MyService myservice){
		this.myservice = myservice;
	}
	@GetMapping("/di/hello")
	public String getHello(){
		return myservice.getHello(); 
	}
```

- 필드 객체 선언을 통한 의존성 주입

```java
@RestController
public class FieldInjectionController{
	@Autowired
	private MyService mySerivce;
}
```

- setter 메서드

```java
@RestController
public class SetterInjectionController{
	MyService myservice;
	
	@Autowired
	public void setMyService(MyService myService){
		this.myService = myService;
	}
}
```

스프링 공식 문서에서는 DI 방법 중 생성자로 DI받는 것을 권장합니다.

---

## 1.1_3 AOP - 관점 지향 프로그래밍(Aspect-Oriented Programming)

스프링의 아주 중요한 특징입니다.
AOP는 OOP를 더욱 잘 사용하도록 돕는 개념으로 보는 것이 좋습니다.

AOP는 관점을 기준으로 묶어 개발하는 방식을 의미합니다.

### **aspect(관점)이란?**

어떤 기능을 구현할 때 그 기능을 ‘핵심 기능’과 ‘부가 기능’으로 구분해 각각을 하나의 관점으로 보는 것을 의미합니다.

**핵심기능이란?**

비즈니스 로직을 구현하는 과정에서 비즈니스 로직이 처리하려는 목적 기능을 말합니다.

예) client로부터 상품 등록 요청을 받아 db에 저장하고, 그 상품 정보를 조회하는 로직을 구현한다면
(1) 상품 정보를 db에 저장하고, (2) 저장된 상품 정보 데이터를 보여주는 코드가 ‘핵심 기능’입니다.

OOP 방식의 app 로직에선 객체마다 핵심 기능을 수행하기 위한 로직과 함께 부가 기능인 로깅, 트랜잭션 등의 코드를 작성하게 됩니다.
AOP 관점에서는 부가 기능은 핵심 기능이 어떤 기능인지에 무관하게 로직이 수행되기 전 또는 후에 수행되기만 하면 된다.

(image)

이처럼 여러 비즈니스 로직에서 반복되는 부가 기능을 하나의 공통 로직으로 처리하도록 모듈화해 삽입하는 방식을 AOP라고 한다.

다시 말해, 개발에서 공통적으로 사용되는 기능(로깅, 보안, 트랜잭션 등)을 핵심 비즈니스 로직에서 분리하여 관리할 수 있게 해주는 프로그래밍 패러다임입니다.

### AOP 구현하는 3가지 방법

1. 컴파일 과정에 삽입하는 방식
2. 바이트코드를 메모리에 로드하는 과정에 삽입하는 방식
3. 프락시 패턴을 이용한 방식
이 방식은 AOP 프레임워크가 대상 객체를 위한 프록시를 생성하고, 이 프록시를 통해 애스펙트를 적용하는 방법입니다.
스프링 AOP가 이 방식을 사용합니다.
프록시는 대상 객체의 호출을 가로채 애스펙트의 코드(예: 전/후 처리)를 실행하고, 
이후 실제 객체의 메서드를 호출합니다.
프록시 기반 AOP는 실행 시점에 프록시 객체를 통해 동적으로 애스펙트를 적용하기 때문에 구현이 비교적 간단하고, 컴파일 타임이나 로드 타임에 비해 설정이 쉽습니다. 
하지만, 메서드 호출마다 프록시를 거치기 때문에 성능상의 오버헤드가 발생할 수 있습니다.

이 가운데 스프링은 디자인 패턴 중 하나인 **프락시 패턴**을 통해 AOP 기능을 제공하고 있습니다.

### AOP 목적

- OOP와 마찬가지로 모듈화해서 재사용 가능한 구성을 만드는 것이고, 모듈화된 객체를 편하게 적용할 수 있게 함으로써 개발자가 비즈니스 로직을 구현하는 데만 집중할 수 있게 도와주는 것입니다.

주로 AspectJ와 같은 도구를 사용할 때 볼 수 있습니다. 
여기서는 스프링 AOP를 사용한 프록시 패턴을 이용한 방식의 간단한 예시를 제공하겠습니다.

## **스프링 부트에서 프록시 패턴을 이용한 AOP 구현 예시**

### 1. 의존성 추가

먼저, **`pom.xml`**에 스프링 부트 AOP 의존성을 추가합니다(Maven 기준).

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

Gradle을 사용한다면, **`build.gradle`**에 다음을 추가합니다.

```groovy
implementation 'org.springframework.boot:spring-boot-starter-aop'
```

### 2. 애스펙트 정의

**`@Aspect`** 어노테이션을 사용하여 애스펙트를 정의합니다. 로깅을 위한 간단한 애스펙트 예시입니다.

```java
package com.example.demo.aspect;

import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.aspectj.lang.annotation.AfterReturning;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LoggingAspect {

    private static final Logger logger = LoggerFactory.getLogger(LoggingAspect.class);

    // Service 레이어의 모든 메서드에 적용
    @Pointcut("execution(* com.example.demo.service.*.*(..))")
    public void serviceLayerExecution(){}

    // 메서드 실행 전에 로그 출력
    @Before("serviceLayerExecution()")
    public void logBeforeServiceLayerExecution() {
        logger.info("Before executing service method");
    }

    // 메서드가 성공적으로 반환된 후 로그 출력
    @AfterReturning("serviceLayerExecution()")
    public void logAfterReturningServiceLayerExecution() {
        logger.info("After returning from service method");
    }
}
```

### 3. 비즈니스 로직

**`@Aspect`**에 의해 로깅이 추가될 간단한 서비스 클래스 예시입니다.

```java
package com.example.demo.service;

import org.springframework.stereotype.Service;

@Service
public class SampleService {

    public String getSampleString() {
        return "This is a sample service method";
    }
}
```

이 예시에서는 **`SampleService`** 클래스의 모든 메서드 호출 전후에 로깅이 이루어집니다. 
스프링 AOP는 메서드 호출을 가로채 애스펙트(여기서는 로깅)를 적용합니다. 
이 방식은 비즈니스 로직에서 공통 기능의 코드를 분리하여, 코드의 재사용성과 유지 보수성을 향상시킵니다.

## 1.1_4 스프링 프레임워크의 다양한 모듈

## 1.2.1 의존성 관리

스프링 프레임워크와 비교했을 때, 스프링 부트가 가진 특징

- 의존성 추가만하면 각 라이브러리의 기능과 관련해서 자주 사용되고 서로 호환되는 버전의 모듈 조합을 제공합니다. → 이를 통해 개발자는 라이브러리 호환 문제를 해결할 수 있습니다 (원래라면 스프링 프레임워크나 라이브러리의 버전을 호환되는 버전을 확인하고 직접 명시해야 정상 작동 됨)

스프링 부트 app이 실행되면 우선 @ComponentScan 어노테이션이 @Component 시리즈 어노테이션이 붙은 클래스를 발견해  빈(bean)을 등록합니다.
이후 @EnableAutoConfiguration 어노테이션을 통해 패키지 안에 spring.factories 파일을 추가해 다양한 자동 설정이 일부 조건을 거쳐 적용되게끔 만듭니다.

@Component 어노테이션이 포괄하는 어노테이션들을 통칭하기 위해 사용한 표현입니다.
이러한 @Component 시리즈 어노테이션의 대표적인 예는

1. @Controller
2. @RestController
3. @Service
4. @Repository
5. @Configuration

이 있습니다.

또한 스프링 부트는 각 웹 app에 내장 **WAS**(Web Application Server)가 존재합니다.

스프링 부트의 자동 설정 기능은 톰캣에도 적용되므로 특별한 설정 없이도 톰캣을 실행할 수 있습니다.

스프링 부트 **엑추에이터**를 통해 개발이 끝나고 서비스를 운영하는 시기에 해당 시스템이 사용하는 스레드, 메모리 세션 등의 중요 요소들을 모니터링 할 수 있습니다.
