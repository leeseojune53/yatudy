# Spring 에서 Swagger 사용하기

### 🎊 시작하기 전에...

이 글은 Spring-doc, Kotlin을 사용한다.

### 1️⃣ 의존성 설정

build.gradle.kts에 dependencies에 아래 코드를 추가한다.

```
implementation(org.springdoc:springdoc-openapi-ui:${VERSION})
```

### 2️⃣ Swagger 설정

```kotlin
import io.swagger.v3.oas.models.OpenAPI
import io.swagger.v3.oas.models.info.Info
import org.springdoc.core.GroupedOpenApi
import org.springframework.context.annotation.Bean
import org.springframework.context.annotation.Configuration

@Configuration
class SwaggerConfig {

    @Bean
    fun publicApi(): GroupedOpenApi = GroupedOpenApi.builder()
        .group("v1-notification-service")
        .pathsToMatch("/notifications/*")
        .build()

    @Bean
    fun springShopOpenApi(): OpenAPI {
        return OpenAPI()
            .info(
                Info().title("XQUARE-Notification API")
                    .description("XQUARE Notification의 Api 명세서입니다.")
                    .version("v0.0.0")
            )
    }

}
```

Config 파일에 group, api prefix(path) 등 설정을 하고, Swagger 창에서 띄워줄 정보(제목, 설명, 버전 등)을 설정한다.
