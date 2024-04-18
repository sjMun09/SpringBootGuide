공식문서 : https://docs.spring.io/spring-data/jpa/docs/current-SNAPSHOT/reference/html/#reference

챕터 6에선 Spring Data JPA 기본 기능을 살펴봤습니다.
또한 리포지토리를 활용해 DB에 접근하고 기본적인 CRUD 기능을 사용해 봤습니다.

# JPQL

JPA Query Language

JPA에서 사용할 수 있는 쿼리를 의미한다.

JPQL의 문법은 SQL과 매우 비슷해서 db 쿼리에 익숙한 사람들이라면 어렵지 않게 사용할 수 있다.

SQL과의 차이는 sql에선 테이블이나 칼럼의 이름을 사용하는 것과 달리 JPQL은 엔티티 객체를 대상으로 수행하는 쿼리이기 때문에 매핑된 엔티티의 이름과 필드의 이름을 사용한다는 것이다.

### JPQL 쿼리의 기본구조

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/3aa0feec-e59d-4038-837c-5f30c26c951c/Untitled.heic)

## 쿼리 메서드란 ?

리포지토리는 JpaRepository를 상속받는 것만으로도 다양한 CRUD 메서드를 제공합니다.
하지만 이러한 기본 메서드들은 식별자 기반으로 생성되기 때문에 결국 별도의 메서드를 정의해서 사용하는 경우가 많다. 이때 간단한 쿼리문을 작성하기 위해 사용되는 것이 쿼리 메서드이다.

### 쿼리 메서드의 생성

쿼리 메서드는 크게 동작을 결정하는 주제(subject)와 서술어(Predicate)로 구분한다.
’`find…by`’, ‘`exists…By`’와 같은 키워드로 쿼리의 주제를 정하며 ‘`By`'는 서술어의 시작을 나타내는 구분자 역할을 합니다.
서술어 부분은 검색 및 정렬 조건을 지정하는 영역입니다.
기본적으로 엔티티의 속성으로 정의할 수 있고, AND나 OR를 사용해 조건을 확장하는 것도 가능합니다.

리포지토리의 쿼리 메서드 생성 예

```java
// (리턴타입) + {주제 + 서술어(속성)} 구조의 메서드
List<Person> findByLastNameAndEmail(String lastName, String email);
```

서술어에 들어가는 엔티티의 속성 식(Expression)은 위의 예시와 같이 엔티티에서 관리하고 있는 속성(필드)만 참조할 수 있다.

## 쿼리 메서드의 주제 키워드

- find…By
- read…By
- get…By
- query…By
- search…By
- stream…By

보다시피 조회하는 기능을 수행하는 키워드입니다.
’`…`’으로 표시한 영역에는 도메인(엔티티)을 표현할 수 있다.
그러나 리포지토리에서 이미 도메인을 설정한 후에 메서드를 사용하기 때문에 중복으로 판단해 생략하기도 한다.
리턴 타입으로는 Controller이나 Stream에 속한 하위 타입을 설정할 수 있습니다.

아래 예제와 같이 ProductRepository에 쿼리 메서드를 작성한다.

```java
// find...By
Optional<Product> findByNumber(Long number);
List<Product> findAllByName(String name);
Product queryByNumber(Long number);
```

### `exists…By`

특정 데이터가 존재하는지 확인하는 키워드이며, 리턴 타입으로는 boolean 타입을 사용한다.

```java
// exists...By
boolean existsByNumber(Long number);
```

### `count…By`

조희 쿼리를 수행한 후 쿼리 결과로 나온 레코드의 개수를 리턴한다.

```java
//count...By
long countByName(String name);
```

### `delete…By`, `remove…By`

삭제 쿼리를 수행한다.
리턴 타입이 없거나 삭제한 횟수를 리턴한다.

```java
// delete...By, remove...By
void deleteByNumber(Long number);
long removeByName(String name);
```

### `…First<number>…`, `…TOP<number>…`

쿼리를 통해 조회된 결괏값의 개수를 제한하는 키워드이다.
두 키워드는 동일한 동작을 수행하며, 주제와 By 사이에 위치한다.
일반적으로 이 키워드는 한 번의 동작으로 여러 건을 조회할 때 사용되며, 단 건으로 조회하기 위해서는 <number>를 생략하면 된다.

**First, Top 키워드를 활용한 쿼리 메서드**

```java
// …First<number>…, …TOP<number>… 
List<Product> findFirst5ByName(String name);
List<Product> findTop100ByName(String name);
```

## 쿼리 메서드의 조건자 키워드

### `IS`

값의 일치를 조건으로 사용하는 조건자 키워드이며, 생략되는 경우가 많으며 Equals와 동일한 기능 수행

```java
// findByNumber 메서드와 동일하게 동작
Product findByNumberIs(Long number);
Product findByNumberEquals(Long number);
```

### `(Is)Not`

값의 불일치를 조건으로 사용하는 조건자 키워드이며, Is는 생략하고 Not 키워드만 사용할 수도 있다.

```java
Product findByNumberIsNot(Long number);
Product findByNumberNot(Long number);
```

### `(Is)Null, (Is)NotNull`

값이 null인지 검사하는 조건자 키워드

```java
List<Product> findByUpdatedAtNull();
List<Product> findByUpdateAtIsNull();
List<Product> findByUpdateAtNotNull();
List<Product> findByUpdateIsNotNull();
```

### `(Is)True, (Is)False`

boolean 타입으로 지정된 칼럼값을 확인하는 키워드이며,
아래 예제는 Product Entity에 boolean 타입을 사용하는 칼럼이 없기 때문에 실제 코드에 반영하면 에러가 발생하므로 사용법만 참고합니다.

```java
Product findByisActiveTrue();
Product findByisActiveIsTrue();
Product findByisActiveFalse();
Product findByisActiveIsFalse();
```

### `And, Or`

여러 조건을 묶을 때 사용

### (Is)GreaterThan, (Is)LessThan, (Is)Between

숫자나 dateTime 칼럼을 대상으로 한 비교 연산에 사용할 수 있는 조건자 키워드

GreaterThan, LessThan 키워드는 비교 대상에 대한 초과/미만의 개념으로 비교 연산을 수행하고, 경곗값을 포함하려면 Equals 키워드를 추가하면 된다.

### (Is)StartingWith(==startsWith), (Is)EndingWith(==EndsWith), (Is)Containing(==Contains), (Is)Like

칼럼값에서 일부 일치 여부를 확인하는 조건자 키워드

SQL 쿼리문에서 값의 일부를 포함하는 값을 추출할 때 사용하는 ‘%’ 키워드와 동일한 역할을 하는 키워드이다.
자동으로 생성되는 SQL문을 보면 Containing 키워드는 문자열의 양 끝, StartingWith 키워드는 문자열의 앞, EndingWith 키워드는 문자열의 끝 ‘%’가 배치된다.
여기서 별도로 고려해야 하는 키워드는 Like 키워드인데, 이 키워든느 코드 수준에서 메서드를 호출하면서 전달하는 값에 %를 명시적으로 입력해야 한다.

# 정렬과 페이징 처리

app에서 자주 사용되는 정렬과 페이징 처리는 앞서 소개한 쿼리 메서드를 작성하는 방법을 기반으로 수행할 수 있다. 기본 쿼리 메서드인 이름을 통한 정렬과 페이징 처리도 가능하지만 다른 방법들도 많이 쓰인다.

### 정렬 처리하기

쿼리문에서는 정렬을 사용할 때는 ORDER BY 구문을 사용한다.
쿼리 메서드도 정렬 기능에 동일한 키워드가 사용된다.

쿼리메서드의 정렬 처리

```java
// ASC : 오름차순, Desc : 내림차순
List<Product> findByNameOrderByNumberAsc(String name);
// 상품 정보를 이름으로 검색하고, 상품번호로 정렬한다는 소리
List<Product> findByNameOrderByNumberDsec(String name);
```

위와 같이 기본 쿼리 메서드를 작성한 후 OrderBy 키워드를 삽입하고 정렬하고자 하는 칼럼과 오름차순/내림차순을 설정하면 정렬이 수행된다.

쿼리 메서드에서 여러 정렬 기준 사용

```java
List<Product> findByNameOrderByPriceAscStockDesc(String name);
//Price 기준으로 오름차순 정렬한 후, 후순위로 재고수량을 기준으로 내림차순 정렬 수행.
```

쿼리메서드의 이름에 정렬 키워드를 삽입해서 정렬을 수행하는 것도 가능하지만 메서드의 이름이 길어질수록 가독성이 떨어지는 문제점가 생깁니다.

이를 해결하기 위해 아래와 같이 매개변수를 활용해 정렬할 수도 있습니다.

```java
// 매개변수를 활용한 쿼리 정렬
List<Product> findByName(String name, Sort sort);
```

이 메서드는 이름에 키워드를 넣지 않고 Sort 객체를 활용해 매개변수로 받아들인 정렬 기준을 가지고 쿼리문을 작성한다. 

매개변수를 활용한 쿼리 메서드를 사용하면 쿼리 메서드를 정의하는 단계에서 코드가 줄어드는 장점이 있습니다.
그러나, 호출하는 위치에서는 여전히 정렬 기준이 길어져 가독성이 많이 떨어집ㄴ디ㅏ.
해당 코든느 정렬 기준을 설정하기 위한 필수적인 구문이기 때문에 코드의 양을 줄이기는 어렵습ㄴ디ㅏ.
하지만 아래 예제와 같이 Sort 부분을 하나의 메서드로 분리해서 쿼리 메서드를 호출하는 코드를 작성하는 방법도 가능합니다.

```java
@SpringBootTest
class ProdcutRepositoryTest{
	@Autowired
	ProductRepository productRepository;
	
	@Test
	vodi sortingAndPagingTest(){
		...
		sout(productrepository.findByName("pen",getSort()));
	}
	
	private Sort getSort(){
		return Sort.by(
					Order.Asc("price"),
					Order,Desc("stock")
					);
		}
	}
```

## 페이징 처리

페이징이란 db의 레코드를 개수로 나눠 페이지를 구분하는 것을 의미합니다.
예) 25개의 레코드가 있다면 레코드를 총 7개씩, 총 4개의 페이지로 구분하고 그중에서 특정 페이지를 가져오는 것이다.
흔히 볼 수 있는 웹 페이지에서 각 페이지를 구분해서 데이터를 제공할 때 그에 맞게 데이터를 요청하는 것이라고 보면된다.

JPA에서는 이 같은 페이징 처리를 위해 `**Page**`와 `**Pageable**`을 사용한다.

```java
Page<Product>findByName(String name, Pageable pageable);
```

위와 같은 리턴 타입으로 Page를 설정하고 매개변수에는 Pageable 타입의 객체를 정의한다.
해당 메서드를 사용하기 위해서 예제 8.19와 같이 호출한다.

```java
// 8.19
Page<Product> productPage = productRepository.findByName("펜", PageRequest.of(0,2);
```

위 코드는 메서드를 호출할 때 리턴 타입으로 Page 객체를 받아야 하기 때문에 Page<Product>로 타입을 선언해서 객체를 리턴받는다.
그리고 Pageable 파라미터를 전달하기 위해 PageRequest 클래스를 사용했다.
PageRequest는 Pageable의 구현체다.

일반적으로 PageRequest는 of 메서드를 통해 PageRequest 객체를 생성한다.
of 메서드는 매개변수에 따라 다양한 형태로 오버로딩돼 있는데 다음과 같은 매개변수 조합을 지원합니다.

IMG

Page 객체를 그대로 출력하면 해당 객체의 값을 보여주지 않고 몇 번째 페이지에 해당하는지 확인할 수 있습니다.
그러나 각 페이지를 구성하는 세부적인 값을 보려면 아래와 같이 작성해야 합니다.

```java
Page<Prodcut> productPage = productRepository.findByName("Pen", PageRequest.of(0,2);
sout.(productPage.getContent());
```

getContent() 메서드를 사용해 출력한다면, 배열 형태로 값이 출려됩니다.

# `@Query` 어노테이션

db에서 값을 가져올 때는

1. 메서드의 이름만으로 쿼리 메서드를 생성
2. @Query 어노테이션을 사용해 직접 JPQL을 작성

JPQL을 사용하면 JPA 구현체에서 자동으로 쿼리 문장을 해석하고 실행하게 된다.

만약 DB를 다른 DB로 변경할 일이 없다면 직접 해당 db에 특화된 SQL을 작성할 수 있으며, 주로 튜닝된 쿼리를 사용하고자 할 때 직접 SQL을 작성한다.

```java
@Query("select p from Product as p where p.name = ?1")
List<Product> findByName(String name);
```

1번 줄과 같이 @Query 어노테이션을 사용해 JPQL 형식의 쿼리문을 작성한다(참고로 쿼리문에서 SQL 예약어에 해당하는 단어는 대/소문자 상관 x). 

1. from 뒤에서 엔티티 타입을 지정하고 별칭을 생성한다.(as 생략가능)
2. where문에서는 sql과 마찬가지로 조건을 지정한다.
3. 조건문에서 사용한 ‘`?1`’은 파라미터를 전달받기 위한 인자에 해당한다.
4. 1은 첫 번째 파라미터를 의미한다.

하지만 이 같은 방식을 사용할 경우 파라미터의 순서가 바뀌면 오류가 발생할 가능성이 있다.
→ `@Param` 어노테이션을 사용하여 예방하자.

```java
@Query("SELECT p FROM Product p WHERE p.name = :name"
List<Product> findBYNameParam(@Param("name") String name);
```

보다시피 파라미터를 바인딩하는 방식으로 메서들르 구현하면 코드의 가독성이 높아지고 유지보수가 수월해진다.

그리고 @Query를 사용하면 엔티티 타입이 아니라, 원하는 칼럼의 값만 추출할 수 있다.

```java
// 특정 칼럼만 추출하는 쿼리
@Query("SELECT p.name, p.price, p.stock FROM Product p WHERE p.name = :name")
List<Object[]> findByNameParam2(@Param("name") String name);
```

이처럼 SELECT에 가져오고자 하는 칼럼을 지정하면 된다.
이때 메서드에선 Object 배열의 리스트 형태로 리턴 타입을 지정해야한다.

# QueryDSL

문자열이 아니라 코드로 쿼리를 작성할 수 있도록 도와줍니다.

@Query 어노테이션은 직접 문자열을 입력하기 때문에 컴파일 시점에 에러를 잡지 못하고 런타임 에러가 발생할 수 있습니다.
쿼리의 문자열이 잘못된 경우에는 app이 실행된 후 로직이 실행되고 나서야 오류를 발견할 수 있습니다.

### QueryDSL 이란 ?

정적 타입을 이용해 SQL과 같은 쿼리를 생성할 수 있도록 지원하는 프레임워크입니다.

문자열이나 XML 파일을 통해 쿼리를 작성하는 대신 QueryDSL이 제공하는 플루언트 API를 활용해 쿼리를 생성할 수 있다.

### 장점

1. IDE가 제공하는 코드 자동 완성 기능 사용가능
2. 문법적으로 잘못된 쿼리 사용 X
→ 따라서 정상적으로 활용된 QeuryDSL은 문법 오류를 발생시키지 않음
3. 고정된 SQL 쿼리를 작성하지 않기 때문에 동적으로 쿼리 생성 가능
4. 코드로 작성하므로 가독성 및 생산성 향상
5. 도메인 타입과 프로퍼티를 안전하게 참조 가능

### QueryDSL을 사용하기 위한 설정

1. pom.xml 에 의존성 추가.
2. <plugins>태그에 QueryDSL을 사용하기 위한 APT 플로그인 추가.

(이렇게 설정을 마치고 나면 generated-soruce 경로에 Q 도메인 클래스가 생성된 것을 확인할 수 있다)

<aside>
📌 **APT란 ?**
Annotation Processing Tool

어노테이션으로 정의된 코드를 기반으로 새로운 코드를 생성하는 기능이다.
클래스를 컴파일하는 기능도 제공한다.

</aside>

QeuryDSL은 지금까지 작성했던 엔티티 클래스와 Q도메인이라는 쿼리 타입의 클래스를 자체적으로 생성해서 메타데이터로 사용하는데, 이를 통해 SQL과 같은 쿼리를 생성해서 제공한다.

만약 Q도메인 클래스가 제대로 생성되지 않았다면 프로젝트 폴더를 마우스 우클릭한 후 Maven → Generate Sources and Update Foldes를 선택한다.

또한 코드가 정상적으로 동작하지 않는다면 IDE의 설정을 조정해야 한다.

Ide에서 소스파일로 인식할 수 있게 설정해주면 된다.(generated-soruces에 대해 Source로 인식하도록 설정)

QueryDSL 사용법 (229~ 241page)
