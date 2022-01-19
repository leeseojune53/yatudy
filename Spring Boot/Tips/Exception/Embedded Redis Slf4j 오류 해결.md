# Embedded Redis Slf4j 오류 해결

### 🐛 문제 상황

```LoggerFactory is not a Logback LoggerContext but Logback is on the classpath```

build.gradle 에 ```testImplementation 'it.ozimov:embedded-redis:0.7.3'```를 추가하고 실행하게되면 위의 에러가 발생한다.

### 🏴‍☠️ 원인

embedded-redis 0.7.3 버전을 뜯어보면 Slf4j가 추가되어있다.

Spring Boot 내중 Slf4j과 충돌해서 발생한 것이다.

### ♻ 해결법

embedded-redis를 다운그레이드하는 것이다.

```testImplementation 'it.ozimov:embedded-redis:0.7.2'``` 로 변경해주면 Slf4j가 없어지면서 해결된다.