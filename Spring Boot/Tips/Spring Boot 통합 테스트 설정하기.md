# Spring Boot 통합 테스트 설정하기



## 🎊 시작하기 전에..

통합 테스트는 단위 테스트 다음 단계에 이루어진다.



### 1️⃣ build.gradle

```java
testImplementation 'com.h2database:h2' 									//테스트 데이터베이스
testImplementation 'org.springframework.boot:spring-boot-starter-test'	//스프링 테스트
testImplementation 'org.springframework.security:spring-security-test'	//보안 테스트
```



### 2️⃣ BaseIntegrationTest 작성하기

```java
@SpringBootTest
@AutoConfigureMockMvc
@ActiveProfiles("test")
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
public class BaseIntegrationTest {

    protected MockMvc mockMvc;

    @Autowired
    protected ObjectMapper objectMapper;

    @BeforeEach
    void setUp(WebApplicationContext webApplicationContext) {
        this.mockMvc = MockMvcBuilders.webAppContextSetup(webApplicationContext)
                .addFilters(new CharacterEncodingFilter("UTF-8", true)) // 필터 추가
                .build();
    }
}
```

위의 코드를 차근차근 해석하면

`@SpringBootTest`는 통합테스트를 제공하는 기본적인 스프링 부트 테스트 어노테이션이다.

`@AutoConfigureMockMvc`는 **MockMvc를 생성**하는 어노테이션이다.

`@ActiveProfiles`는 테스트 시 **어떤 properteis의 값**을 **사용**할 것인지 지정해주는 어노테이션이다.

`@AutoConfigureTestDatabase`에서 기본값인 `Replace.Any`를 그대로 사용하면 내장된 Embedded DB를 사용하므로 데이터 소스를 따로 사용하고싶다면 Replace.NONE로 설정하면된다.