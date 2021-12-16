# Spring Boot Admin 구축하기

### 🎊 시작하기전에...

Spring Boot Admin에 대해서 대략적으로 파악하는게 좋을 것 같다.

또, Spring Boot Admin을 구축하려면 하나 이상의 Client가 존재해야한다.

**현재 Kotlin에서는 작동하지 않는것으로 보인다.**

### 1️⃣ 가장 먼저

Spring Boot Admin 프로젝트를 만들어주고, 밑의 의존성을 추가해준다.

`implementation 'de.codecentric:spring-boot-admin-server:${version}'`

그리고, Client 프로젝트에 아래의 의존성을 추가해준다.

`implementation 'de.codecentric:spring-boot-admin-client:${version}'`

### 2️⃣ 두 번째로

Spring Boot Admin의 yml을 아래와 같이 설정한다.

```yaml
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

Spring Boot Client는 아래와같이 설정한다.

```yaml
spring:
  boot:
    admin:
      client:
        url: http://localhost:8000

management:
  endpoints:
    web:
      exposure:
        include: "*"
server:
  port: 18080
```



### ✅ 끝으로

위의 코드는 정말 Spring Boot Admin을 맛보는 코드이다. 실제로는 Spring Security를 사용해서 정보 접근을 제한해야한다.