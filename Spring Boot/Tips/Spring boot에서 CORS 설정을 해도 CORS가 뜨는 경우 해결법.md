# Spring boot에서 CORS 설정을 해도 CORS가 뜨는 경우 해결법

### 🐛 문제 상황

`WebMvcConfigurer`를 구현한 Config class에서 addCorsMapping를 해줬지만, 그대로 CORS가 뜨는 경우이다.

### 🏴‍☠️ 원인

결론부터 말하면, Options 메소드를 이용해 preflight를 하는데, 서버에 해당 요청을 보내도 되는지 확인하는 작업을 하는데, Spring Security에서 Options도 막았기 때문이다.

### ♻ 해결법

`WebSecurityConfigurerAdapter`을 구현한 Config class에서 Options Method에 대해 모든 path를 허용해주면 된다.

```java
~~
.antMatchers(HttpMethod.OPTIONS, "/**").permitAll()
~~
```

