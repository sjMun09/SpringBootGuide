외부의 요청을 받아 응답하는 기능을 구현해서 컨트롤러가 어떻게 구성되는지 알아보자.

## GET API

웹 app 서버에서 값을 가져올 때 사용하는 api이다.
실무에서는 http 메서드에 따라 컨트롤러 클래스를 구분하지 않는다.

→ 보통 `@GetMapping(value=”/board/admin”)` 이런식 Url을 설정해서 사용함.

`@RequestMapping` 어노테이션은 별다른 설정 없이 선언하면 Http의 모든 요청을 받는다.
그러나 get 형식의 요청만 받기 위해서는 어노테이션에 별도 설정이 필요하다.

예제 5-2) method 요소의 값을 RequestMethod.GET으로 설정하면 요청 형식을 GET으로만 설정할 수 있다.

```java
@RestController
@RequestMapping("/api/v1/get-api")
class GetController{
	@RequestMapping(value ="/hello", method = RequestMethod.GET)
	public String getHello(){
		return "hello world";
		}
}
```

이렇게 작성할 수도 있는데 스프링 4.3 이후로는 새로 나온 아래의 어노테이션을 사용하기 때문에 `@RequestMapping` 어노테이션을 사용하지 않습니다.

- GetMapping
- PostMapping
- PutMapping
- DeleteMapping

이 4개의 어노테이션이 대표적입니다.

### 예제 5-3) 매개변수가 없는 GET 메서드 구현

```java
//http://localhost:8080/api/v1/get-api/name
@GetMapping(value="/name")
public String getName(){
	return "flature";
}
```

매개변수가 없는 요청은 위 예제 코드의 
`http://localhost:8080/api/v1/get-api/name` 이 URL을 그대로 입력하고 요청할 때 스프링 부트 애플리케이션이 정해진 응답을 반환하게 됩니다.

Q. 왜 해당 url을 입력해도 위의 메서드가 호출되면서 return 값이 화면에 출력되는지 알 수 있나요 ?

A. 

매개변수가 없는 GET 메서드의 구현 예제인 코드를 살펴보면, 스프링 부트 애플리케이션은 해당 URL(**`http://localhost:8080/api/v1/get-api/name`**)에 대한 요청을 받았을 때 어떻게 작동하는지 알 수 있습니다. 여기서 중요한 역할을 하는 것은 스프링 프레임워크의 컨트롤러와 **`@GetMapping`** 어노테이션입니다.

1. **컨트롤러 및 매핑**: **`@GetMapping(value="/name")`**이 붙은 메서드는 스프링의 컨트롤러 내에 정의됩니다. 컨트롤러는 클라이언트의 HTTP 요청을 처리하는 클래스로, 특정 URL 경로에 대한 요청을 처리할 메서드를 매핑하는 어노테이션을 사용합니다. 여기서 **`@GetMapping`**은 HTTP GET 요청을 해당 메서드로 라우팅하는 데 사용됩니다. **`value="/name"`**은 이 메서드가 **`/api/v1/get-api/name`** 이라는 URL 패턴에 반응하도록 지정합니다.
2. **메서드 실행**: 클라이언트가 **`http://localhost:8080/api/v1/get-api/name`** URL로 GET 요청을 보내면, 스프링 부트 애플리케이션은 **`getName()`** 메서드를 실행합니다. 이 메서드는 간단히 문자열 **`"flature"`**를 반환합니다.
3. **HTTP 응답 반환**: 반환된 **`"flature"`** 문자열은 스프링 부트에 의해 HTTP 응답의 본문으로 설정되고 클라이언트로 전송됩니다. 스프링 부트는 내부적으로 반환 값으로부터 HTTP 응답을 구성합니다. 일반적으로, 반환 타입이 단순한 문자열일 경우 **`text/plain`** 타입의 응답으로 처리되어 클라이언트의 브라우저나 HTTP 클라이언트에 표시됩니다.

이 과정을 통해, 매개변수 없는 GET 메서드가 어떻게 간단한 문자열을 클라이언트에 전송하는지를 알 수 있습니다. 클라이언트는 해당 URL에 접속만 하면 서버로부터 "flature"라는 응답을 받게 되는 것입니다.

Q. http://localhost:8080/name 으로 접속하게 되면 똑같이 getName 메서드를 호출하게 되는거야 ?

A. 

**`http://localhost:8080/name`** URL로 접속했을 때 **`getName`** 메서드가 호출되는지 여부는 URL 매핑 설정에 달려 있습니다. 제공해 주신 예제 코드에서는 **`@GetMapping(value="/name")`** 어노테이션이 사용된 메서드가 **`/api/v1/get-api/name`** 경로에 매핑되어 있습니다. 이는 스프링 부트 애플리케이션이 **`/api/v1/get-api/name`** 경로에 대한 GET 요청을 해당 메서드로 라우팅하도록 설정되어 있음을 의미합니다.

따라서, **`http://localhost:8080/name`**으로 접속하면, 이 URL은 **`getName`** 메서드에 매핑되어 있지 않기 때문에 스프링 부트는 해당 메서드를 호출하지 않습니다. 대신, 이 경로에 대한 별도의 처리가 설정되어 있지 않다면 스프링 부트는 404 Not Found 오류를 반환할 것입니다. 이는 요청된 URL에 해당하는 리소스가 서버에서 찾을 수 없음을 나타냅니다.

**`http://localhost:8080/name`** 경로로 접속했을 때 **`getName`** 메서드를 호출하려면, 스프링 컨트롤러 내에서 해당 경로를 **`getName`** 메서드 또는 다른 메서드에 매핑해야 합니다. 예를 들어 다음과 같이 설정할 수 있습니다:

```java
@GetMapping(value="/name")
public String getNameSimple() {
    return "Simple name";
}

@GetMapping(value="/api/v1/get-api/name")
public String getName() {
    return "flature";
}
```

이렇게 하면 **`/name`** 경로는 **`getNameSimple`** 메서드로 매핑되고, **`/api/v1/get-api/name`**은 **`getName`** 메서드로 매핑됩니다.

> 이해하기 쉽게 정리하겠습니다.
매개변수가 없는 GET 메서드 구현할 때, getName 메서드를 호출하면서 리턴 값이 나온 이유는
해당 메서드가 GetController 안에 있기 때문입니다.
> 

### @PathVariable을 활용한 GET 메서드

실무에선 매개변수를 받지 않는 메서드는 거의 쓰이지 않는다.
웹 통신의 기본 목적은 데이터를 주고 받는 것이기 때문에 대부분 매개변수를 받는 메서드를 작성하게 됩니다.
매개변수를 받을 때 자주 쓰이는 방법 중 하나는 URL 자체에 값을 담아 요청하는 것입니다.

예제 5-4) URL에 값을 담아 전달되는 요청을 처리하는 방법(@PathVariable을 활용한 GET 메서드)

```java
// http://localhost:8080/api/v1/get-api/variable1/{String 값}
@GetMapping(value="/variable1/{variable}")
public String getvariable1(@PathVariable String variable){
	return variable;
}
```

`http://localhost:8080/api/v1/get-api/variable1/{String 값}` 요청 예시 url을 보면
이 메서드는 중괄호`{ }` 로 표시된 위치의 값을 받아 요청하는 것을 알 수 있다.
(실제 요청 시 중괄호는 들어가지 않으며 값만 존재)

값을 간단히 전달할 때 주로 사용하는 방법이며, GET 요청에서 많이 사용된다.

이러한 방식으로 코드를 작성할 때는 몇 가지 지켜야 할 규칙이 있다.
@GetMapping 어노테이션의 값으로 URL을 입력할 때 중괄호를 사용해 어느 위치에서 값을 받을지 지정해야 한다.
또한 메서드의 매개변수와 그 값을 연결하기 위해 @PathVariable을 명시하며, @GetMapping 어노테이션과 @PathVariable에 지정된 변수의 이름을 동일하게 맞춰야 합니다.

만약 @GetMapping 어노테이션에 지정한 변수의 이름과 메서드 매개변수의 이름을 동일하게 맞추기 어렵다면
@PathVariable 뒤에 괄호를 열어 @GetMapping 어노테이션의 변수명을 지정하면 된다.

예제 5-5) @PathVariable에 변수명을 매핑하는 방법

```java
// http://localhost:8080/api/v1/get-api/variable2/{String 값}
@GetMapping(value = "/variable2/{variable}")
public String getVariable2(@PathVariable("variable")String var){
	return var;
}
```

2번째 줄에 적혀 있는 변수명인 variable과 3번 줄에 적힌 매개변수명인 var가 서로 일치하지 않는 상황에서 두 값을 매핑하는 방법을 보여준다. @PathVariable에는 변수의 이름을 특정할 수 있는 value 요소가 존재하며, 이 위치에 변수 이름을 정의하면 매개변수와 매핑할 수 있다.
3번 줄의 @PathVariable 사용법을 좀 더 풀어쓰면 다음과 같다.

```java
public String getVariable2(@PathVariable(value="variable") String var){
```

## @RequestParam을 활용한 GET 메서드

GET요청을 구현할 때 url 경로에 값을 담아 요청을 보내는 방법 외에도 쿼리 형식으로 값을 전달할 수도 있습니다.

즉, uri에서 ‘?’를 기준으로 우측에 ‘{key}={value}’ 형태로 구성된 요청을 전송하는 방법입니다.

애플리케이션에서 이 같은 형식을 처리하려면 @RequestParam을 활용하면 된다.
아래와 같이 매개변수 부분에 @RequestParam 어노테이션을 명시해 쿼리 값과 매핑하면 됩니다.

예제 5-6) @RequestParam을 활용한 GET 메서드 구현

```java
// http://localhost:8080/api/v1/get-api/request1?name=value&email=value2&oraganication=value3
@GetMapping(value ="/request1")
public String getRequestParam1(
	@RequestParam String name,
	@RequestParam String email,
	@RequestParam String organization){
	return name + " " + email + " "+organization;
}
```

1번 줄을 보면 ‘?’ 오른쪽에 쿼리스트링(query string)이 명시돼 있습니다.
쿼리스트링에는 키(변수의 이름)가 있기 때문에 이 값을 기준으로 메서드의 매개변수에 이름을 매핑하면 값을 가져올 수 있습니다.
키와 @RequestParam 뒤에 적는 이름을 동일하게 설정하기 어렵다면 @PathVariable 에제에서 사용한 방법처럼 value 요소로 매핑합니다.

만약 쿼리스트링에 어떤 값이 들어올지 모른다면 아래와 같이 Map 객체를 활용할 수도 있습니다.

예제 5-7) @RequestParam과 Map을 조합한 GET method

```java
//http://localhost:8080/api/v1/get-api/request2?key1=value1&key2=value2
@GetMapping(value= "/request2")
public String getRequestParam2(@RequestParam Map<String, String>param){
	StringBuilder sb = new StringBuiler();
	
	param.entrySet().forEach(map -> {
		sb.append(map.getKey() + " : " + map.getValue() + "\n");
		});

		return sb.toString();
}
```

위의 형태로 코드를 작성하면 값에 상관없이 요청을 받을 수 있습니다.
예를 들면, 회원 가입 관련 API에서 사용자는 회원 가입을 하면서 ID 같은 필수 항목이 아닌 취미 같은 선택 항목에 대해서는 값을 기입하지 않는 경우가 있습니다.
이러한 경우에는 매개변수의 항목이 일정하지 않을 수 있어 Map 객체로 받는 것이 효율적입니다.

<aside>
📌 ***URI와 URL의 차이***
URL은 우리가 흔히 말하는 웹 주소를 의미하며,
리소스가 어디에 있는지 알려주기 위한 경로를 의미합니다.
반면 URI는 특정 리소스를 식별할 수 있는 식별자를 의미합니다.
웹에서는 URL을 통해 리소스가 어느 서버에 위치해 있는지 알 수 있으며, 그 서버에 접근해서
리소스에 접근하기 위해서는 대부분 URI가 필요합니다.

</aside>

## DTO 객체를 활용한 GET 메서드

### DTO란 ?

DTO는 Data Transfer Object의 약자로, 다른 레이어 간의 데이터 교환에 활용된다.
간략하게 설명하면 각 클래스 및 인터페이스를 호출하면서 전달하는 매개변수로 사용되는 데이터 객체입니다.

DTO는 데이터를 교환하는 용도로만 사용하는 객체이기 때문에 DTO에는 별도의 로직이 포함되지 않습니다.
ch06에선 DTO 클래스의 역할을 좀 더 자세히 다룰 예정입니다.

<aside>
📌 ***DTO와 VO***
DTO와 VO(value object)의 역할을 서로 엄밀하게 구분하지 않고 상요할 떄가 많습니다.
이렇게 해도 대부분의 상황에서는 큰 문제가 발생하지 않지만 정확하게 구분하자면 역할과 사용법에서 차이가 있다.
먼저 VO는 데이터 그 자체로 의미가 있는 객체를 의미한다.
VO의 가장 특징적인 부분은 읽기전용(read-only)으로 설계한다는 점이다.
즉, VO는 값을 변경할 수 없게 만들어 데이터의 신뢰성을 유지해야 한다는 것입니다.
DTO는 데이터 전송을 위해 사용되는 데이터 컨테이너입니다.
즉, 같은 애플리케이션 내부에서 사용되는 것이 아니라 다른 서버(시스템)로 전달하는 경우에 사용됩니다
본문에서는 DTO가 다른 레이어 간 데이터 교환에 활용된다고 이 전에 말했습니다.
여기서 레이어는 애플리케이션 내부에 정의된 레이어일 수도 있고 인프라 관점에서의 서버 아키텍처 상의 레이어일 수도 있습니다. 이러한 개념의 혼용이 DTO와 VO의 차이를 흐리게 만듭니다.
이 같은 용어와 개념을 정확하게 사용하는 것도 중요하지만 팀 내부적으로 용어나 개념의 역할 범위를 설정하고 합의해서 사용 한다면 업무를 효율적으로 처리하는 데 도움이 됩니다.

</aside>

예제 5-8) DTO 객체는 아래와 같이 작성할 수 있습니다.

```java
pulbic class MemberDto
	private String name;
	private String email;
	private String organization;
	
	public String getName(){
		return name;
		}
	public void setName(String name){
		this.name = name;
		}
	public String getEmail(){
		return email;
		}
	public void setEmail(String email){
		this.email =email;
		}
		public String getOrganization(){
			return organization;
		}
		public void setOrganization(String organization)
				this.organization = organization;
		}
		
		@Override
		public String toString(){
			return "MemberDto("+
				"name='"+name+'\''+
				", email='" + email + '\'' +
				", organization='" + organization +'\'' +
				'}';
		}
}
```

DTO 클래스에는 전달하고자 하는 필드 객체를 선언하고 getter/setter 메서드를 구현합니다.
DTO클래스에 선언된 필드는 컨트롤러의 메서드에서 쿼리 파라미터의 키와 매핑됩니다.
즉, 쿼리스트링의 키가 정해져 있지만 받아야 할 파라미터가 많을 겨웅에는 아래와 같이 DTO 객체를 활용해 코드의 가독성을 높일 수 있습니다.

예제 5-9) DTO객체를 활용한 GET method

```java
//http://localhost:8080/api/v1/get-api/request3?name=value1&email=value1&email=value2&organization=value3
@GetMapping(value="/reqeust3")
public String getRequestParam3(MemberDto memberDto){
	//return memberDto.getName() + " " +memberDto.getEmail() + " " + memberDto.getOrganization();
	return memberDto.toString();
}	
```

주석문은 예제 5.6과 동일한 쿼리스트링을 가집니다.
다만 3번 줄과 같이 예제 5.6에 비해 코드의 양을 줄일 수 있습니다.

# POST API

웹 애플리케이션을 통해 db 등의 저장소에 리소스를 저장할 때 사용되는 api입니다.
앞에서 살펴본 GET API에서는 URL의 경로나 파라미터에 변수를 넣어 요청을 보냈지만 POST API에서는 저장하고자 하는 리소스나 값을 HTTP body에 담아 서버에 전달합니다.
그래서 URI가 GET API에 비해 간단합니다.

POST API를 만들기 위해 우선 @RequestMapping 어노테이션을 이용해 공동 URL을 설정해야 합니다.

예제 5-10) 컨트롤러 클래스에서 공통 URL tjfwjd

```java
@RestController
@RequestMapping("/api/v1/post-api")
public class PostController{
}
```

### @RequestMapping

POST API에서 @RequestMapping을 사용하는 방법은 GET API와 크게 다르지 않는다.
요청 처리 메서드를 정의할 때 아래와 같이 method 요소를 RequestMethod.POST로 설정하는 부분을 제외하면 GET API와 동일합니다.

예제 5-11) @RequestMapping 사용 예

```java
@RequestMapping(value = "/doamin", method = RequestMethod.POST)
public String postExample(){
	return "Hello Post API";
}
```

### @RequestBody를 활용한 POST 메서드 구현

예제 5-11) 에서는 별도의 리소스를 받지 않고 단지 POST 요청만 받는 메서드를 구현했다.
일반적으로 POST 형식의 욫엉은 클라이언트가 서버에 리소스를 저장하는 데 사용합니다.
그러므로 클라이언트의 요청 트래픽에 값이 포함돼 있습니다.
즉, POST 요청에서는 리소를 담기 위해 HTTP Body에 값을 넣어 전송합니다.

Body 영역에 작성되는 값은 일정한 형태를 취합니다.
일반적으로 JSON 형식으로 전송되며, 이 책에서도 가장 대중적으로 사용되는 JSON 형식으로 값을 주고 받을 예정입니다. 이렇게 서버에 들어온 요청은 아래와 같이 처리할 수 있습니다.

예제 5-12) @RequestBody와 Map을 활용한 POST API 구현

```java
// http://localhost:8080/api/v1/post-api/member
@PostMapping(value= "/member")
public String postMember(@RequestBody Map<String, Object>postData){
	StringBuilder sb = new StringBuilder();
	
	postData.entrySet().forEach(map -> {
		sb.append(map.getKey() + " : " + map.getvalue() + "\n");
	});
	return sb.toString();
	}
```

2번 줄을 보면 @RequestMapping 대신 @PostMapping을 사용했다.
이 어노테이션을 사용하면 method 요소를 정의하지 않아도 되기 때문이다.
그리고 3번 줄에선 @RequestBody라는 어노테이션을 사용헀는데,
@RequestBody는 HTTP 의  Body 내용을 해당 어노테이션이 지정된 객체에 매핑하는 역할을 합니다.

<aside>
📌 ***JSON 이란?*** (Java Script Object Notation)
자바스크립트의 객체 문법을 따르는 문자 기반의 데이터 포맷입니다.
현재는 자바스크립트 외에도 다수의 프로그래밍 환경에서 사용합니다.
대체로 네트워크를 통해 데이터를 전달할 때 사용하며, 문자열 형태로 작성되기 때문에 파싱하기도 쉽다는 장점이 잇습니다.

</aside>

Map 객체는 요청을 통해 어떤 값이 들어오게 될지 특정하기 어려울 때 주로 사용합니다.
요청 메시지에 들어갈 값이 정해져 있다면 예제 5.13과 같이 DTO객체를 매개변수로 삼아 작성할 수 있습니다.
이 예제의 DTO 객체는 예제5.8에서 사용한 것과 동일합니다.

```java
// http://localhost:8080/api/v1/post-api/member2
@PostMapping(value = "/member2")
public String postMemberDto(@RequestBody MemberDto memberDto){
	return memberDto.toString();
}
```

위와 같이 작성하면 MemberDto의 멤버 변수를 요청 메시지의 키와 매핑해 값을 가져옵니다.

회원가입하면서 id 같은 필수 항목이 아닌 취미같은 선택 항목에 대해서는 값을 기입하지 않는 경우가 존재합니다.
이러한 경우에는 매개변수의 항목이 일정하지 않을 수 있어 Map객체로 받는 것이 더 효율적이다.

### → 이를 코드로 나타낸다면 ?

스프링 부트에서는 주로 **`@RequestParam`** 어노테이션을 사용하여 HTTP 요청 파라미터를 컨트롤러 메서드의 매개변수로 받을 수 있습니다. 선택적인 매개변수들을 처리할 때는 **`Map<String, String>`** 타입을 사용하여 필수가 아닌 매개변수들을 유연하게 관리할 수 있습니다. 아래 예시에서는 회원 가입 시 ID는 필수 항목이지만, 취미는 선택적 항목으로 처리하는 방법을 보여줍니다.

스프링 부트 컨트롤러 예시

```java
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import java.util.Map;

@RestController
public class RegistrationController {

    @PostMapping("/register")
    public String registerUser(@RequestParam("id") String id, @RequestParam Map<String, String> optionalParams) {
        // 필수 파라미터 처리
        System.out.println("ID: " + id);

        // 선택적 파라미터 처리
        if (optionalParams.containsKey("hobby")) {
            System.out.println("Hobby: " + optionalParams.get("hobby"));
        }

        // 회원가입 로직 추가...

        return "Registration successful!";
    }
}

```

위의 코드에서 **`@RequestParam("id") String id`**는 필수 파라미터 'id'를 처리하고, **`@RequestParam Map<String, String> optionalParams`**는 모든 나머지 선택적 파라미터들을 **`Map`** 객체에 저장합니다. 이 **`Map`**은 키-값 쌍으로 구성되어 있으며, 사용자가 제공하는 선택적 파라미터들을 동적으로 처리할 수 있습니다. 이렇게 하면 어떤 선택적 파라미터가 들어올지 모르는 상황에서도 유연하게 대응할 수 있습니다.

## REST API 명세를 문서화 - Swagger

api를 개발하면 명세를 관리해야 한다.
명세란 해당 api가 어떤 로직을 수행하는지 설명하고 이 로직을 수행하기 위해 어떤 값을 요청하며,
이에 따른 응답 값으로는 무엇을 받을 수 있는지를 정리한 자료이다.

### 그렇다면 Swagger란 ?

api는 개발 과정에서 계속 변경되므로 작성한 명세  문서로 주기적인 업데이트가 필요하다.
또한 명세 작업은 번거롭고 시간소요가 많이 걸리기 때문에 스웨거를 적극 사용하는 추세다.

스웨거를 사용하기 위해서는 의존성을 추가해야한다.

(메이븐의 경우 pom.xml 파일에 의존성 추가) (gradle 에선 build.gradle 에 의존성 추가)

스웨거를 사용하려면 코드를 설정해야 한다.
→ 왜 ??? , 왜 설정해야 하는거지? → 기본설정이 완료되면 localohst:포트번호/swagger-ui-html 로 접속하면 된다.

스웨거를 더 잘 활용하기 위해

@RequestParam을 활용한 GET 메서드

그렇다면, 스웨거를 사용한다면 모든 DTO 클래스의 메서드를 매개변수로 활용하기 위해 @RequestParam 어노테이션을 써야하는 것일까 ?

## 정리

### **1. 왜 스웨거를 설정해야 하는가?**

스웨거는 API 문서를 자동으로 생성해주는 도구로, 개발자가 API의 구조를 더 쉽게 이해하고, 테스트할 수 있도록 도와줍니다. 스웨거를 사용하려면 몇 가지 설정을 해야 하는데, 이는 스웨거가 프로젝트의 API 엔드포인트들을 인식하고 문서화하기 위해 필요한 메타데이터를 수집할 수 있도록 하기 위함입니다. 스웨거 설정을 통해 API 개발의 투명성과 효율성을 높일 수 있습니다.

스웨거를 설정하면 API에 대한 정보(예: 경로, 매개변수, HTTP 메서드, 응답 등)를 자동으로 파악하여 사용자가 쉽게 API를 테스트하고 문서화할 수 있게 해줍니다. 설정이 완료되면 기본적으로 **`localhost:포트번호/swagger-ui.html`** 주소로 접속하여 생성된 API 문서를 볼 수 있습니다.

### **2. 스웨거를 더 잘 활용하기 위한 방법**

스웨거를 통해 API를 더 잘 문서화하고 사용하기 위해서는 다음과 같은 방법을 사용할 수 있습니다:

- **API 메서드에 적절한 어노테이션 사용**: **`@ApiOperation`**, **`@ApiParam`** 등 스웨거 전용 어노테이션을 사용하여 메서드나 매개변수의 설명을 추가할 수 있습니다. 이를 통해 생성되는 문서의 가독성과 유용성을 높일 수 있습니다.
- **응답 및 요청 객체 모델링**: 요청과 응답에 사용되는 모델(DTO)의 필드에 **`@ApiModelProperty`** 어노테이션을 추가하여, 각 필드에 대한 설명을 제공할 수 있습니다.

### **3. 모든 DTO 클래스의 메서드를 매개변수로 활용하기 위해 @RequestParam 어노테이션을 사용해야 하나?**

**`@RequestParam`**은 주로 간단한 쿼리 파라미터를 처리할 때 사용합니다. 반면, 복잡한 데이터 구조를 전송해야 할 때는 DTO(Data Transfer Object) 클래스를 사용하는 것이 좋습니다. DTO를 사용하면 여러 데이터 필드를 하나의 객체로 그룹화하여 API 요청 및 응답을 더 체계적으로 관리할 수 있습니다.

스프링 부트에서는 **`@ModelAttribute`**나 **`@RequestBody`** 어노테이션을 이용하여 DTO 객체를 메서드의 매개변수로 사용할 수 있습니다. **`@RequestBody`**는 주로 JSON이나 XML과 같은 복잡한 데이터 구조를 받을 때 사용하고, **`@ModelAttribute`**는 주로 폼 데이터를 받을 때 사용됩니다.

스웨거를 사용할 때는 이런 DTO 클래스를 사용하여 API의 매개변수를 정의하는 것이 좋으며, 스웨거에서는 이러한 DTO를 통해 자동으로 문서의 요청 및 응답 부분을 적절히 표현할 수 있습니다.

따라서, 모든 메서드에서 **`@RequestParam`**을 사용할 필요는 없으며, 요청의 복잡성과 성격에 따라 적절한 어노테이션을 선택하는 것이 중요합니다.

스웨거는 보통 설정(Configuration)에 관한 클래스로 config라는 패키지 생성한 후에 그 안에서 생성하는 것이 좋다.

```java
package com.springboot.api.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

// 예제 5.22 -> 스웨거 설정 코드
@Configuration
@EnableSwagger2
public class SwaggerConfiguration {

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
            .apiInfo(apiInfo())
            .select()
            .apis(RequestHandlerSelectors.basePackage("com.springboot.api"))
            .paths(PathSelectors.any())
            .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
            .title("Spring Boot Open API Test with Swagger")
            .description("설명 부분")
            .version("1.0.0")
            .build();
    }
}
```

해당 챕터를 읽을때 아래와 같은 개념을 모르셨다면 자바의 정석으로 공부하고 오시기를 권장드립니다.

1. Map 이란 ?
2. Map 에서 entrySet() 메서드는 ?
3. forEach() 는 무엇?
4. return 할 때 stringBuilder는 왜 toSring()의 메서드를 사용하는 것일까 ?

## 정답

### **1. Map이란?**

Map은 키(key)와 값(value) 쌍으로 데이터를 저장하는 자료구조입니다. Java에서 **`Map`**은 인터페이스로, 이를 구현한 여러 클래스가 있으며, 가장 흔히 사용되는 구현체는 **`HashMap`**, **`TreeMap`**, **`LinkedHashMap`** 등이 있습니다. Map의 특징은 각 키는 유일해야 하며, 각 키는 하나의 값을 가질 수 있습니다. 이를 통해 데이터를 효율적으로 저장하고 빠르게 검색할 수 있습니다.

### **2. Map에서 entrySet() 메서드는?**

**`entrySet()`** 메서드는 Map 내의 모든 키-값 쌍을 **`Set`** 형태로 반환합니다. 이 반환된 **`Set`**은 **`Map.Entry<K, V>`** 객체들의 집합이며, 여기서 **`K`**와 **`V`**는 각각 키와 값의 타입을 나타냅니다. **`entrySet()`**을 사용하면 Map에 저장된 모든 데이터에 대해 반복(iterate)할 수 있어서 데이터를 검색하거나 조작할 때 유용합니다.

### **3. forEach()는 무엇인가?**

**`forEach()`**는 Java 8에서 도입된 메서드로, 주로 컬렉션(Collection)이나 스트림(Stream)에 있는 각 요소에 대해 반복적으로 작업을 수행할 때 사용됩니다. **`forEach()`**는 람다 표현식을 매개변수로 받아 컬렉션의 각 요소에 이 람다를 적용합니다. 이 메서드는 내부적으로는 Iterator를 사용하여 컬렉션의 각 요소를 처리하며, 코드를 간결하게 만들어 줍니다.

예시

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 100);
map.put("banana", 200);

map.forEach((key, value) -> System.out.println(key + " has price " + value));
```

이 코드는 각 키와 값을 출력합니다.

### **4. return 할 때 stringBuilder는 왜 toString()의 메서드를 사용하는 것일까?**

**`StringBuilder`**는 문자열을 효율적으로 조합하고 수정할 수 있는 클래스입니다. 문자열 연산(특히 반복적인 수정이나 추가)이 많은 경우, **`String`** 대신 **`StringBuilder`**를 사용하면 성능이 향상됩니다. 이는 **`StringBuilder`**가 내부적으로 문자 배열을 사용하여 동적으로 확장하기 때문입니다.

**`toString()`** 메서드는 **`StringBuilder`**에 축적된 모든 문자를 하나의 **`String`**으로 결합하여 반환합니다. **`return`**에서 **`toString()`**을 사용하는 이유는, 일반적으로 메서드 외부에서는 **`String`** 형태의 데이터가 필요하기 때문입니다. 즉, **`StringBuilder`**의 수정 가능한 특성은 내부에서만 유용하고, 외부로 데이터를 전달할 때는 불변의 **`String`** 형태로 전환하여 안정성과 사용 편의성을 보장합니다.

# 로깅 라이브러리- Logback

로깅이란 app이 동작하는 동안 시스템의 상태나 동작 정보를 시간순으로 기록하는 것을 의미합니다.

로깅은 비기능 요구사항에 속합니다.
즉, 사용자나 고객에게 필요한 기능은 아니라는 의미이다.
하지만 로깅은 디버깅하거나 개발 이후 발생한 문제를 해결할 때 원인을 분석하는 데 꼭 필요한 요소입니다.

그렇다면 LogBack의 특징은 ?

<aside>
👉 자바에서는 문자열을 합치는 방법은 크게 4가지가 있다.
1. + 연산자
2. String  클래스의 { concat(), append(), format() } 메서드 활용 이렇게 총 4가지가 있다.

</aside>

로깅 설정 및 출력에 배웠다.
로그에 어떤 내용을 포함할지에 관해, → app 마다 출력해야 할 로그가 다르기 떄문이다.

Controller를 통해 값을 받는 방법을 알아봤다.
+ 컨트롤러를 작성해서 외부에 인터페이스를 노출하는 방법도 알아봤다.
