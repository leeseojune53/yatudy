# Spring Cloud OpenFeign

> Netflix에서 만든 선언적 Rest Client 라이브러리. Rest Template를 **대체**할 수 있다.



## Rest Template란?

Spring 3.0부터 지원하고있고, 스프링이 제공하는 HTTP통신에 사용할 수 있는 **템플릿**, HTTP 서버와의 통신을 단순화하고 RESTful 원칙을 지킴.

- 기계적이고, 반복적인 코드를 최대한 줄여준다.
- RESTful 형식에 맞춘다
- json, xml을 쉽게 응답받는다.

하지만 위에서 *기계적이고, 반복적인 코드를 최대한 줄여준다.* 라고 기술하였는데 RestTemplate는 시간이 지나고, 코드가 쌓일수록 유지보수하기 어렵다.



## OpenFeign

의존성 추가

```
implementation 'org.springframework.cloud:spring-cloud-starter-openfeign:3.0.3'
```



### @EnableFeignClients

최상위 package에 선언한다. @EnableFeignClients 어노테이션을 붙이면, @FeignClient를 선언한 인터페이스를 찾아서 구현체를 자동으로 만들어준다.

### @FeignClient

FeignClient를 선언해줄 때 value는 클라이언트의 이름이 된다. url은 baseUrl을 나타낸다. 

## FeignClient Interface

Spring Web의 어노테이션을 그대로 사용하면 된다

example : @GetMapping, @PostMapping, @RequestHeader, @RequestParam 등을 사용할 수 있다.

### 실제 서비스에서 사용

@FeignClient가 붙어있는 Interface는 자동으로 구현체가 만들어지고, Bean으로 생성되기때문에 서비스에서

```java
@RequiredArgsConstructor
public class TestService {
	private final TestClient testclient;
}
```

위와 같은방법으로 DI받고, 서비스에서 사용하면 된다.

