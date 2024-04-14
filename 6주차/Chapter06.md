# DB 설정

![스크린샷 2024-04-13 192513.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/3af8406d-0321-481b-b505-41f2f35bfb2c/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2024-04-13_192513.png)

db설치 진행시 TCP 포트 번호 3306이 관례적으로 쓰는 번호이지만, 나는 이미 사용중이라고 하여 ***3307***로 포트번호 지정해서 다운로드 진행.

## **MariaDB 실행**

MariaDB를 설치할 때 같이 설치된 프로그램이 있다. 

바로 ***HeidiSQL***이라는 프로그램인데 이 프로그램은 데이터베이스를 보다 편리하게 관리할 수 있도록 도와주는 프로그램인 DBMS(DataBase Management System)이다. 이 프로그램을 실행하려면 Windows의 경우 검색했을 때 다음과 같은 화면을 볼 수 있다.

![스크린샷 2024-04-13 192708.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/e6e39970-46c7-4f39-8c4e-07e675033c0f/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2024-04-13_192708.png)

요렇게 생긴 아이콘이다. 검색창에 HeidiSQL검색해도 나오니 참고하자.

![스크린샷 2024-04-13 192830.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/a7136b11-12f2-44f7-9b91-d0128859d866/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2024-04-13_192830.png)

위 사진은 HeidiSQL을 실행한 화면이다. 

앞으로 사용할 db의 접속 정보를 등록하기 위해 좌측 하단의 [신규] 버튼을 누르면 된다.

그러면 아래와 같이 세션 목록에 새 항목이 등록되면서 우측에 접속 정보를 설정할 수 있는 항목들이 등장한다.

![스크린샷 2024-04-13 193009.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/5df51019-8dc3-4ae4-9837-31e1a7e2afa6/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2024-04-13_193009.png)

연결 주소와 사용자 ID, 암호를 입력하고 **[열기]** 버튼을 클릭하면 데이터베이스에 접속할 수 있다. 여기서 연결 주소는 “127.0.0.1”로 이는 본인의 로컬(local)환경, 즉 본인의 PC에서 데이터베이스를 사용하겠다는 의미이다. 만약 다른 PC나 서버의 데이터베이스를 사용하고자 한다면 해당 장비의 IP주소를 입력하면 된다. 그리고 사용자 “ID”는 “root”이고 암호는 설치할 때 설정한 암호를 입력하면 된다.

나는 root 와 85947ads, 3307의 포트번호를 사용했다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/196e2b28-8394-4509-8835-daeaced5f30a/Untitled.png)

이제 db 생성하는 쿼리를 실행해준다.

# ORM

Object Relational Mapping

자바와 같은 객체지향 언어에서 의미하는 객체와 RDB(Relational Database)의 테이블을 자동으로 매핑하는 방법입니다.

객체지향 언어에서의 객체는 클래스를 의미하며, 클래스는 db의 테이블과 매핑하기 위해 만들어진 것이 아니기 떄문에 RDB 테이블과 어쩔 수 없는 불일치가 존재한다.

ORM은 이 둘의 불일치와 제약사항을 해결하는 역할입니다.

간략하게 app의 클래스와 db의 테이블을 매핑하는 것의 로직을 아래에서 그림에서 확인.

IMG

ORM을 이용하면 쿼리문 작성이 아닌 코드(메서드)로 데이터를 조작할 수 있습니다.

## ORM 장점

1. db 쿼리를 객체지향적으로 조작할 수 있다.
    - 쿼리문을 작성하는 양이 현저히 줄어 개발 비용이 줄어든다.
    - 객체지향적으로 db에 접근할 수 있어 코드의 가독성을 높인다.
2. 재사용 및 유지보수가 편리하다.
    - ORM을 통해 매핑된 객체는 모두 독립적으로 작성되어 있어 재사용이 용이하다.
    - 객체들은 각 클래스로 나뉘어 있어 유지보수가 수월하다.
3. DB에 대한 종속성이 줄어든다 → 객체지향적에 가까워진다는 것이다.
상속을 지양하고 합성을 지향해라. 라는 말이 있다. 이것과 유사하다고 생각하면 된다
    - ORM을 통해 자동 생성된 SQL문은 객체를 기반으로 db 테이블을 관리하기 때문에 db에 종속적이지 않다.
    - db를 교체하는 상황에서도 비교적 적은 리스크를 부담한다.

## ORM 단점

1. ORM만으로 온전한 서비스를 구현하기에는 한계가 있다.
    - 복잡한 서비스의 경우 직접 쿼리를 구현하지 않고 코드로 구현하기 어렵다.
    - 복잡한 쿼리를 정확한 설계 없이 ORM만으로 구성하게 되면 속도 저하등의 성능 이슈가 발생할 수 있다.
2. app의 객체 관점과 db 관계 관점의 불일치가 발생한다.
    1. **세분성 (Granularity)**
        - 세분성 불일치는 객체의 상세한 구조와 데이터베이스 테이블 구조 간의 차이를 의미합니다. 예를 들어, 하나의 객체가 여러 테이블에 걸쳐 데이터를 저장해야 할 수도 있고, 반대로 여러 객체가 하나의 테이블에 매핑될 수도 있습니다. ORM을 사용하면 이러한 매핑을 관리하고 적절하게 처리해야 하는 복잡성이 증가합니다.
        - 즉, **orm의 자동 설계 방법에 따라 db에 있는 테이블의 수와 app의 엔티티 클래스의 수가 다른 경우에 생긴다는 것입니다.**(클래스가 테이블의 수보다 많아질 수 있다)
    2. **상속성 (Inheritance)**
        - 객체지향 프로그래밍에서는 상속을 통해 클래스 계층을 구성할 수 있지만, 대부분의 관계형 데이터베이스는 상속 구조를 직접적으로 지원하지 않습니다. 이로 인해, 상속 구조를 갖는 객체 모델을 데이터베이스의 테이블로 표현하려 할 때 추가적인 전략이 필요하며, 이는 종종 단일 테이블 상속, 클래스 테이블 상속, 구체적 테이블 상속 등의 전략을 필요로 합니다.
        - **RDBMS에는 상속이라는 개념이 없습니다.**
    3. **식별성 (Identity)**
        - 객체의 식별성은 메모리 내에서 각 객체 인스턴스의 고유성을 보장합니다. 반면, 데이터베이스에서는 주로 기본 키를 사용하여 레코드의 식별성을 관리합니다. 객체와 데이터베이스 사이의 식별 방식의 차이는 특히 객체를 데이터베이스에 영속화하거나 데이터베이스에서 객체를 재구성할 때 문제가 될 수 있습니다.
        - **RDBMS는 기본키(Primary key)로 동일성을 정의합니다.
        하지만 자바는 두 객체의 값이 같아도 다르다고 판단할 수 있습니다.
        식별과 동일성의 문제입니다.**
    4. **연관성 (Association)**
        - 객체 모델에서는 다양한 방식으로 객체 간의 관계를 표현할 수 있으며, 이는 일반적으로 참조를 통해 이루어집니다. 반면, 데이터베이스는 외래 키를 사용하여 테이블 간의 관계를 정의합니다. 이러한 차이로 인해, ORM을 사용하는 경우 객체 간의 관계를 데이터베이스의 테이블 관계로 변환하는 과정에서 추가적인 매핑 로직이 필요하게 됩니다.
        - **객체지향 언어는 객체를 참조함으로써 연관성을 나타내지만 RDBMS에서는 외래키(foreign key)를 삽입함으로써 연관성을 표현합니다.
        또한 객체지향 언어에서 객체를 참조할 때는 방향성이 존재하지만 RDBMS에서 외래키를 삽입하는 것은 양방향의 관계를 가지기 떄문에 방향성이 없습니다.**
    5. **탐색 (Navigation)**
        - 객체 모델에서는 객체 간의 참조를 통해 쉽게 탐색할 수 있습니다. 하지만 데이터베이스에서는 이러한 탐색이 쿼리를 통해 이루어지며, 이는 성능 저하를 초래할 수 있습니다. ORM을 사용하면 객체 간의 탐색을 데이터베이스 쿼리로 변환하는 과정에서 효율성이 감소할 수 있습니다.
        - **자바와 RDBMS는 어떤 값(객체)에 접근하는 방식이 다릅니다.
        자바에서는 특정 값에 접근하기 위해 객체 참조 같은 연결 수단을 활용합니다.
        이 방식은 객체를 연결하고 또 연결해서 접근하는 그래프 형태의 접근 방식입니다.
        (예: 어떤 멤버의 회사 주소를 구하기 위해 member.getOrganization.getAddress()와 같이 접근할 수 있습니다) 
        반면 RDBMS에선 쿼리를 최소화하고 조인(JOIN)을 통해 여러 테이블을 로드하고 값을 추출하는 접근 방식을 채택하고 있습니다.**

## JPA

Java Persistence API
자바에 진영의 ORM 기술 표준으로 채택된 인터페이스의 모음입니다.

ORM이 큰 개념이라면 JPA는 더 구체화된 스펙을 포함합니다.
즉, JPA 또한 실제로 동작하는 것이 아니고 어떻게 동작해야 하는지 메커니즘을 정리한 표준 명세로 생각하면 됩니다.

JPA의 메커니즘을 보면 내부적으로 JDBC를 사용합니다.
개발자가 직접 JDBC를 구현하면 SQL에 의존하게 되는 문제 등이 있어 개발의 효율성이 떨어지는데,
JPA는 이 같은 문제점을 보완해서 개발자 대신 적절한 SQL을 생성하고 DB를 조작해서 객체를 자동 매핑하는 역할을 수행합니다.

JPA 기반의 구현체는 대표적으로 3가지가 있지만 하이버네이트를 가장 많이 사용합니다.

## 하이버네이트

자바의 ORM 프레임워크

이 책은 하이버네이트의 기능을 더욱 편하게 사용하도록 모듈화한 Spring Data JPA를 활용하기 떄문에 JPA 자체를 직접 사용할 일은 거의 없다는 점을 참고.

### Spring Data JPA

JPA를 편리하게 사용할 수 있도록 지원하는 스프링 라이브러리다.
CRUD 처리에 필요한 인터페이스를 제공하며, 하이버네이트의 엔티티 매니저(EntityManager)를 직접 다루지 않고 리포지토리를 정의해 사용함으로써 스프링이 적합한 쿼리를 동적으로 생성하는 방식으로 db를 조작하게 됩니다.

## 영속성 컨텍스트

app와 db 사이에서 엔티티와 레코드의 괴리를 해소하는 기능과 객체를 보관하는 기능을 수행한다.
엔티티 객체가 영속성 컨텍스트에 들어오면 JPA는 엔티티 객체의 매핑 정보를 DB에 반영하는 작업을 수행한다.
이처럼 엔티티 객체가 영속성 컨텍스트에 들어와 JPA의 관리 대상이 되는 시점부터는 해당 객체를 영속 객체라고 부른다.

IMG

영속성 컨텍스트는 세션 단위의 생명주기를 가진다. → 세션 단위의 생명주기란 ?

→ 데이터베이스 세션과 연결된 컨텍스트의 생명주기를 의미합니다. 즉, 영속성 컨텍스트는 사용자 세션 또는 트랜잭션 세션과 같은 특정 세션의 시작부터 종료까지 유효하며, 세션 종료와 함께 소멸됩니다.

→ 즉, db에 접근하기 위한 세션이 생성되면 영속성 컨텍스트가 만들어지고, 세션이 종료되면 영속성 컨텍스트도 없어진다. Entity Manager는 이러한 일련의 과정에서 영속성 컨텍스트에 접근하기 위한 수단으로 사용된다.

### 엔티티 매니저 Entity Manager

엔티티를 관리하는 객체

엔티티 매니저는 db에 접근해서 CRUD 작업을 수행한다.
SPring Data JPA를 사용하면 리포지토리를 사용해서 db에 접근하는데, 실제 내부 구현체인 SimpleJpaRepository가 리포지토리에서 엔티티 매니저를 사용하는 것을 알 수 있습니다.

엔티티 매니저는 엔티티 매니저 팩토리(ENtity Manager Factory)가 만듭니다.
엔티티 매니저 팩토리는 db에 대응하는 객체로서 스프링 부트에서는 자동 설정 기능이 있기 떄문에 yml에서 작성한 최소한의 설정만으로도 동작하지만 JPA의 구현체 중 하나인 하이버네이트에서는 xml이라는 설정 파일을 구성하고 사용해야 하는 객체입니다.

### 엔티티의 생명주기

엔티티 객체는 영속성 컨텍스트에서 다음과 같은 4가지 상태로 구분됩니다.

비영속

- 영속성 컨텍스트에 추가되지 않은 엔티티 객체의 상태를 의미한다.

영속

- 영속성 컨텍스트에 의해 엔티티 객체가 관리되는 상태이다.

준영속

- 영속성 컨텍스트에 의해 관리되던 엔티티 객체가 컨텍스트와 분리된 상태다.

삭제

- db에서 레코드를 삭제하기 위해 영속성 컨텍스트에 삭제 요청을 한 상태다.

### db 연동

하이버 네이트 옵션

`spring.jpa.hibernate.ddl-auto=create` 

- create : app이 가동되고 SessionFactory가 실행될 때 기존 테이블을 지우고 새로 생성한다.
이 책에서는 모든 예제를 create로 설정하고 진행했다.
- create-drop : create와 동일한 기능을 수행하나 app을 종료하는 시점에 테이블을 지운다.
- update : SessionFactory가 실행될 때 객체를 검사해서 변경된 스키마를 갱신한다.
기존에 저장된 데이터는 유지된다.
- validate : update처럼 객체를 검사하지만 스키마는 건드리지 않는다.
검사 과정에서 db의 테이블 정의와 객체의 정보가 다르면 에러가 발생한다.
- none : ddl-auto 기능을 사용하지 않는다.

참고로 OS 환경에서는 create, create-drop, update 기능응ㄴ 사용하지 않는다.

db에 축적된 데이터를 지워버릴 수도 있고, 사람의 실수로 객체의 정보가 변경됐을 때 os  환경의 db정보까지 변경될 수 있기 때문입니다.

OS 환경에서는 대체러 validate나 none을 사용합니다.

반면 개발 환경에서는 create 또는 update를 주로 사용하는 편입니다.

### 엔티티 설계

Spring Data JPA를 사용하면 db에 테이블을 생성하기 위해 직접 쿼리를 작성할 필요가 없습니다.
이 기능을 가능하게 하는 것이 엔티티입니다.

JPA에서 엔티티는 db의 테이블에 대응하는 클래스입니다.
엔티티에는 db에 쓰일 테이블과 칼럼을 정의합니다.
엔티티에 어노테이션을 사용하면 테이블 간의 연관관계를 정의할 수 있습니다.

## 엔티티 관련 기본 어노테이션

### `**@Entity**`

해당 클래스가 엔티티임을 명시하기 위한 어노테이션.
클래스 자체는 테이블과 일대일로 매칭되며, 해당 클래스의 인스턴스는 매핑되는 테이블에서 하나의 레코드를 의미한다.

### `@Table`

엔티티 클래스는 테이블과 매핑되므로 특별한 경우가 아니면 @Table 어노테이션이 필요하지 않는다.
@Table 어노테이션을 사용할 떄는 클래스의 이름과 테이블의 이름을 다르게 지정해야 하는 경우이다.

@Table 어노테이션을 명시하지 않으면 테이블의 이름과 클래스의 이름이 동일하다는 의미이며,
서로 다른 이름을 쓰려면 @Table(name =값) 형태로 db의 테이블명을 명시해야 한다.
대체로 자바의 명명법과 db의 명명법은 다르기 떄문에 자주 사용됩니다.

### `@Id`

엔티티 클래스의 필드는 테이블의 칼럼과 매핑된다.

@Id 어노테이션이 선언된 필드는 테이블의 기본값 역할로 사용된다.
모든 엔티티는 @Id 어노테이션이 필요하다. 꼭꼮꼮꼮꼬꼬꼬ㅗ꼬꼬꼮! 기억하자 !!!!

### `@GeneratedValue`

일반적으로 @Id 어노테이션과 함께 사용됩니다.
해당 필드의 값을 어떤 방식으로 자동으로 생성할지 결정할 때 사용합니다.
값 생성 방식은 다음과 같습니다.

**@GenerateValue를 사용하지 않는 방식(직접 할당)**

- 애플리케이션에서 자체적으로 고유한 기본값을 생성할 경우 사용하는 방식
- 내부에 정해진 규칙에 의해 기본값을 생성하고 식별자로 사용한다

**AUTO**

- @GenerateValue의 기본 설정값
- 기본값을 사용하는 db에 맞게 자동 생성한다.

**IDENTITY**

- 기본값 생성을 db에 위임하는 방식
- db의 AUTO_INCREMENT를 사용해 기본값을 생성
- 필자도 GenertaeValue 어노테이션 사용하면 거의 IDENTITY 옵션을 썻었다.

**SEQUENCE**

- @SequenceGenerator 어노테이션으로 식별자 생성기를 설정하고 이를 통해 값을 자동 주입받는다.
- SequenceGenerator를 정의할 때는 name, sequenceName, allocationSize를 활용한다.
- @GeneratedValue에 생성기를 설정한다.

**TABLE**

- 어떤 DBMS를 사용하더라도 동일하기 동작하기를 원할 경우 사용한다.
- 식별자로 사용할 숫자의 보관 테이블을 별도로 생성해서 엔티티를 생성할 때마다 값을 갱신하며 사용한다.
- @TableGenerator 어노테이션으로 테이블 정보를 설정한다.

### `@Column`

엔티티 클래스의 필드는 자동으로 테이블 칼럼으로 매핑된다.
그래서 별다른 설정을 하지 않을 예정이라면 이 어노테이션을 명시하지 않아도 된다.
해당 어노테이션은 필드에 몇 가지 설정을 더할 때 사용한다.

1. name : db 칼럼명을 설정하는 속성, 명시하지 않으면 필드명으로 지정
2. nullable : 레코드를 생성할 때 칼럼 값에 null 처리가 가능한지를 명시하는 속성
3. length : db에 저장하는 최대 길이를 설정 (생각보다 자주 쓰임)
4. unique : 해당 칼럼을 유니크로 설정 (생각보다 은근 쓰임)

### `@Transient`

엔티티 클래스에는 선언돼 있는 필드지만 db에선 필요 없을 경우 이 어노테이션을 사용해 db에서 이용하지 않게 할 수 있다.

## 리포지토리 인터페이스 설계

SPring Data JPA는 JpaRepository를 기반으로 더욱 쉽게 db를 사용할 수 있는 아키텍처를 제공한다.
스프링 부트로 JpaRepository를 상속하는 인터페이스를 생성하면 기존의 다양한 메서드를 손쉽게 활용할 수 있습니다.

엔티티를 db의 테이블과 구조를 생성하는 데 사용헀다면 리포지토리는 엔티티가 생성한 db에 접근하는 데 사용됩니다.

리포지토리를 생성하기 위해서는 접근하려는 테이블과 매핑되는 엔티티에 대한 인터페이스를 생성하고 아래와 같이 JpaRepository를 상속받으면 됩니다.

```java
public interface ProductRepository extends JpaRepository<Product, Long>{
}
```

ProductRepository가  JpaRepository를 상속받을 때는 대상 엔티티와 기본값 타입을 지정해야 한다.
엔티티를 사용하기 위해서는 위와 같이 **대상 엔티티를 Product로 설정하고 해당 엔티티의 @Id필드 타입인 Long을 설정하면 된다**.

JpaRepository 상속 구조

IMG

### 리포지토리 메서드의 생성 규칙

IMG

- OrderBy : SQL문에서 order by와 동일한 기능을 수행한다. 예를 들어, 가격 순으로 이름 조회를 수행한다면 `List<Product> findByNameOrderByPriceAsc(String name);`과 같이 작성한다.
- countBy : SQL 문의 count와 동일한 기능을 수행하며, 결괏값의 개수(count)를 추출한다.

## DAO 설계

Data Access Object

db에 접근하기 위한 로직을 관리하기 위한 객체다.
비즈니스 로직의 동작 과정에서 데이터를 조작하는 기능은 DAO 객체가 수행한다.

다만 SPring data JPA에서 DAO의 개념은 Repository가 대체하고 있다.

규모가 작은 서비스에선 DAO를 별도로 설계하지 않고 바로 서비스 레이어에서 db에 접근해서 구현하기도 한다.
하지만, 이번 챕터에선 DAO를 서비스 레이어와 리포지토리의 중간 계층을 구성하는 역할로 사용할 예정이다.

이 책에서는 간단한 db 호출만 다루고 있기 떄문에 큰 의미는 없지만 실제로 업무에 필요한 비즈니스 로직을 개발하다 보면 데이터를 다루는 중간 계층을 두는 것이 유지보수 측면에서 용이한 경우가 많다.

<aside>
👉 ***DAO* vs *Repository***
 DAO와 리포지토리는 역할이 비슷하다.
그렇기 때문에 아직도 DAO와 리포지토리를 비교하거나 어떤 차이가 있는지 논쟁하는 경우가 많다. 실제로 리포지토리는 SPring Data JPA에서 제공하는 기능이기 때문에 기존의 스프링 프레임워크나 스프링 MVC의 사용자는 리포지토리라는 개념을 사용하지 않고 DAO 객체로 DB에 접근헀습니다.
이런 측면에서 각 컴포넌트의 역할을 고민하는 시간을 가져보면 좋을 것 같습니다.

</aside>

DAO 클래스는 일반적으로 ‘인터페이스-구현체’구성으로 생성합니다.

DAO클래스는 의존성 결합을 낮추기 위한 디자인 패턴이며, 서비스 레이어에 DAO 객체를 주입받을 때 인터페이스를 선언하는 방식으로 구성할 수 있습니다.

일반적으로 db에 접근하는 메서드는 리턴 값으로 데이터 객체를 전달합니다.
이때 데이터 객체를 엔티티 객체로 전달할지, DTO 객체로 전달할지에 대해서는 개발자마다 의견이 분분합니다.
일반적인 설계 원칙에서 엔티티 객체는 db에 접근하는 계층에서만 사용하도록 정의합니다.
다른 계층으로 데이터를 전달할 때는  DTO 객체를 사용합니다.
그러나 회사 및 부서마다 견해 차이가 있으므로 각자 정해진 원칙에 따라 진행하는 것이 좋다.

```java
@Component
public class ProductDAOImpl implements ProductDAO {

    private ProductRepository productRepository;

    @Autowired
    public ProductDAOImpl(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    // 예제 6.11
    @Override
    public Product insertProduct(Product product) {
        Product savedProduct = productRepository.save(product);

        return savedProduct;
    }
```

해당 클래스를 스프링이 관리하는 빈으로 등록하려면 위와 같이 @Component 또는 @Service 어노테이션을 지정해야 합니다.
빈으로 등록된 객체는 다른 클래스가 인터페이스를 가지고 의존성을 주입받을 때 이 구현체를 찾아 주입하게 되기 떄문입니다.

마찬가지로 DAO 객체에서도 db에 접근하기 위해 리포지토리 인터페이스를 사용해 의존성 주입을 받아야 합니다. 아래와 같이 리포지토리를 정의하고 생성자를 통해 의존성 주입을 받으면 된다.

```java
   private ProductRepository productRepository;

    @Autowired
    public ProductDAOImpl(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }
```

```java
@Override
    public Product insertProduct(Product product) {
        Product savedProduct = productRepository.save(product);

        return savedProduct;
    }
```

inserProduct() 메서드가 사용한 리포지토리는 save메서드입니다.

```java
// 예제 6.12
    @Override
    public Product selectProduct(Long number) {
        Product selectedProduct = productRepository.getById(number);

        return selectedProduct;
    }
```

selectProduct() 메서드가 사용한 리포지토리의 메서드는 getById()입니다.
리포지토리에서는 단건 조회를 위한 기본 메서드로 두 가지를 제공하는데, 바로 `getById()` 메서드와 `findById()` 메서드입니다.

두 메서드는 조회한다는 기능 측면에서는 동일하지만 세부 내용이 다릅니다.

### **`findById()`**

- **반환 타입**: **`Optional<T>`**
- **작동 방식**: 데이터베이스에 실제로 접근하여 해당 ID의 엔티티를 찾습니다. 만약 엔티티가 존재하지 않으면 **`Optional.empty()`**를 반환합니다. 이 메서드는 즉시 데이터베이스에서 값을 조회하기 때문에 데이터가 존재하는지 바로 확인할 수 있습니다.

### **`getById()`**

- **반환 타입**: **`T`**
- **작동 방식**: 프록시 객체를 반환합니다. 이 메서드는 실제 엔티티 객체를 즉시 로드하지 않고, 해당 엔티티에 처음 접근할 때 데이터베이스 조회를 수행합니다. 만약 엔티티가 존재하지 않을 경우 접근하는 시점에 **`EntityNotFoundException`**을 발생시킵니다.

### **스프링 부트 코드 예제**

아래 코드는 스프링 부트 환경에서 각 메서드를 사용하는 간단한 예제입니다. **`UserRepository`** 인터페이스는 JPA의 **`JpaRepository`**를 확장합니다.

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Service;

import java.util.Optional;

interface UserRepository extends JpaRepository<User, Long> {
}

@Service
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User getUserById(Long id) {
        // getById 메서드를 사용하여 프록시 객체를 반환받음
        return userRepository.getById(id);
    }

    public Optional<User> findUserById(Long id) {
        // findById 메서드를 사용하여 실제 데이터베이스에서 엔티티 검색
        return userRepository.findById(id);
    }
}
```

이 코드에서 **`getUserById()`** 메서드는 프록시를 통해 엔티티를 지연 로딩할 때 사용되며, **`findUserById()`** 메서드는 데이터베이스에서 직접 해당 ID의 유저를 조회하고, 존재하지 않을 경우 **`Optional.empty()`**를 반환합니다.

각 메서드의 사용 상황을 결정할 때는 애플리케이션의 필요에 따라 선택하면 됩니다. **`findById()`**는 데이터 존재 유무를 즉시 확인할 필요가 있을 때 유용하고, **`getById()`**는 엔티티를 실제 사용하기 전까지 데이터 로딩을 지연시키고 싶을 때 유용합니다.

여기서 궁금해 해야한다. 프록시 객체를 반환받는다는데, 이게 무슨말이지?

프록시 객체(Proxy Object)는 객체지향 프로그래밍에서 흔히 사용되는 패턴 중 하나로, 실제 객체를 대신하여 그 객체의 일부 기능을 제어하는 데 사용됩니다. 프록시는 실제 객체와 동일한 인터페이스를 제공하여 실제 객체가 하는 일을 대신하거나, 실제 객체에 대한 접근을 제어하거나, 추가적인 기능을 제공할 수 있습니다.

### **ORM에서의 프록시 객체**

ORM (Object-Relational Mapping) 환경, 특히 스프링 데이터 JPA에서 프록시 객체는 주로 엔티티의 지연 로딩(Lazy Loading)을 관리하는 데 사용됩니다. 지연 로딩은 엔티티의 데이터가 실제로 필요할 때까지 로드를 지연시키는 기법입니다. 이를 통해 애플리케이션의 성능을 최적화하고, 불필요한 데이터베이스 접근을 줄일 수 있습니다.

### **프록시 객체의 작동 방식**

- **프록시 생성**: **`getById()`**와 같은 메서드를 통해 요청된 엔티티의 프록시 객체가 생성됩니다. 이 객체는 실제 엔티티 클래스를 상속받거나 같은 인터페이스를 구현하여 실제 객체처럼 보이게 됩니다.
- **행동 대리**: 프록시 객체는 실제 엔티티의 모든 메서드 호출을 처리할 수 있으며, 이 메서드들을 실제 엔티티로 전달합니다. 하지만 실제 엔티티의 데이터가 필요할 때까지는 데이터베이스에서 엔티티를 로드하지 않습니다.
- **데이터 로딩**: 실제 엔티티의 데이터가 필요한 시점(예: 어떤 속성에 접근하거나 메서드를 호출할 때)에 프록시 객체는 데이터베이스에서 해당 데이터를 로드하고, 이후의 요청에 대해 실제 데이터를 반환합니다.

### **예시**

예를 들어, **`User`** 엔티티의 프록시 객체가 있고, 이 객체의 **`getName()`** 메서드를 호출한다고 가정해 보겠습니다. 사용자가 실제로 **`getName()`**을 호출하기 전까지는 프록시 객체가 **`User`**의 실제 인스턴스를 데이터베이스에서 로드하지 않습니다. **`getName()`**이 호출되는 순간, 프록시 객체는 데이터베이스에서 실제 **`User`** 객체를 로드하고, 로드된 실제 객체의 **`getName()`** 메서드를 실행하여 결과를 반환합니다.

프록시 객체는 효율적인 리소스 관리와 성능 최적화를 위해 널리 사용되지만, 프록시 초기화 시점의 부하, 데이터 접근 시 예외 처리 필요성 등을 고려해야 하는 복잡성을 도입할 수도 있습니다. 따라서 이러한 특성을 잘 이해하고 사용하는 것이 중요합니다.

사실상 조회 기능을 구현하기 위해서는 어떤 메서드를 사용하더라도 무관하다.
비즈니스 로직을 구현하는 데 적합한 방식을 선정해 활용하면 되는 것이다.

### 업데이트 메서드 구현

```java
 // 예제 6.15
    @Override
    public Product updateProductName(Long number, String name) throws Exception {
        Optional<Product> selectedProduct = productRepository.findById(number);

        Product updatedProduct;
        if (selectedProduct.isPresent()) {
            Product product = selectedProduct.get();

            product.setName(name);
            product.setUpdatedAt(LocalDateTime.now());

            updatedProduct = productRepository.save(product);
        } else {
            throw new Exception();
        }

        return updatedProduct;
    }
```

JPA에서 데이터의 값을 변경할 때는 다른 메서드와는 다른 점이 있다.
JPA는 값을 갱신할 때 update라는 키워드를 절대 사용하지 않는다.

여기서는 영속성 컨텍스트를 활용해 값을 갱신하는데, find() 메서드를 통해 db에서 값을 가져오면 가져온 객체가 영속성 컨텍스트에 추가됩니다.

영속성 컨텍스트가 유지되는 상황에서 객체의 값을 변경하고 다시 save()를 실행하면  JPA에서는 더티체크(Dirty Check)라고 하는 변경 감지를 수행하게 됩니다.

simpleJpaRepository에 구현돼 있는 save() 메서드를 살펴보자.

@Transactional 어노테이션이 선언돼 있는 것을 확인할 수 있습니다.

이 어노테이션이 지정돼 있으면 메서드 내 작업을 마칠 경우 자동으로 flush() 메서드를 실행합니다.

이 과정에서 변경이 감지되면 대상 객체에 해당하는 db 레코드를 업데이트 하는 쿼리가 실행됩니다.

### 삭제 메서드 구현

```java
// 예제 6.17
    @Override
    public void deleteProduct(Long number) throws Exception {
        Optional<Product> selectedProduct = productRepository.findById(number);

        if (selectedProduct.isPresent()) {
            Product product = selectedProduct.get();

            productRepository.delete(product);
        } else {
            throw new Exception();
        }
    }
```

db의 레코드를 삭제하기 위해서는 삭제하고자 하는 레코드와 매핑된 영속 객체를 영속성 컨텍스트에 가져와야 한다. 
deleteProduct() 메서드는 findById() 메서드를 통해 객체를 가져오는 작업을 수행하고 delete() 메서드를 통해 해당 객체를 삭제하게끔 삭제 요청을 한다.

simpleJpaRepository를 살펴보면 알 수 있다.

delete()메서드는 delete()메서드로 전달 받은 엔티티가 영속성 컨텍스트에 잇는지 파악하고, 
해당 엔티티를 영속성 컨텍스트에 영속화하는 작업을 거쳐 db의 레코드와 매핑한다.

그렇게 매핑된 영속 객체를 대상으로 삭제 요청을 수행하는 메서드를 실행해 작업을 마치고 커밋(commit) 단계에서 삭제를 진행하게 된다.

## DAO 연동을 위한 컨트롤러와 서비스

앞에서 설계한 구성 요소들을 클라이언트의 요청과 연결하려면 컨트롤러와 서비스를 생성해야 한다.

이를 위해 **먼저 (1) DAO의 메서드를 호출**하고  그 외 **(2)비즈니스 로직을 수행하는 서비스 레이어를 생성**한 후 **(3)컨트롤러**를 생성해야한다.
