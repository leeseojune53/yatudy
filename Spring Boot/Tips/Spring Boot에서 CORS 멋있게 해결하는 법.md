# Spring Boot에서 CORS 멋있게 해결하는 법

### 🎊 시작하기 전에..

이 글은 필자가 Spring Boot의 CORS 해결법에 대해서 구글링을 했지만, 레퍼런스별로 달라서 불편함을 느껴 정리한 글이다.

### ♻ 해결법

제일 먼저 **WebMvnConfigurer**를 구현하는 객체 하나를 만들고 **addCorsMapping**을 해줘야 한다.

```java
	@Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
				.allowedMethods("GET", "POST", "DELETE", "PATCH", "PUT")
				.allowedHeaders("*")
                .allowedOrigins("https://google.com");
    }
```

위 방법을 사용하면 일반적인 CORS 허용은 해결된 것인데, **Spring Security**를 사용한다면 아래 방법 또한 적용해야한다.

우선, **WebSecurityConfigurerAdapter** 를 상속받는 객체 하나를 만들고, **configure**에서 cors관련 설정을 해야한다.

```java
http
    .cors().and()
    .authorizeRequests()
    .requestMatchers(CorsUtils::isPreflightRequest).permitAll()
```

위 코드에서 .cors()는 **CorsFilter**를 등록한다. 그 아래에 있는 CorsUtils에서는 **PrefilghtReqeust인 경우**에는 **permitAll**을 해주는 것이다.

PreflightRequest를 **허용해주지 않으면 Options요청에 403이 response**돼서 **Preflight가 실패**한다.

