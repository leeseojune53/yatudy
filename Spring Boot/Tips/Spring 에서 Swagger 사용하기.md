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

Config 파일에서 Swagger 창에서 띄워줄 정보(제목, 설명, 버전 등)을 설정한다.

### 3️⃣ Controller 설정

Controller 설정은 크게 두 가지로 나뉘어 진다.

- Tags: Controller 묶음의 대략적인 설명
- Operation: 개별 Api의 설명

아래 코드와 같이 작성한다.

```kotlin
package io.github.v1servicenotification.domain.detail.presentation

import io.github.v1servicenotification.detail.api.DetailApi
import io.github.v1servicenotification.detail.api.dto.response.DetailResponse
import io.github.v1servicenotification.global.extension.getUserId
import io.swagger.v3.oas.annotations.Operation
import io.swagger.v3.oas.annotations.tags.Tag
import org.springframework.web.bind.annotation.GetMapping
import org.springframework.web.bind.annotation.PathVariable
import org.springframework.web.bind.annotation.PostMapping
import org.springframework.web.bind.annotation.RestController
import java.util.*

@Tag(name = "알림 상세", description = "알림 상세 Api입니다.")
@RestController
class DetailController(
    private val detailApi: DetailApi
) {

    @Operation(summary = "전송된 알림 목록")
    @GetMapping("/")
    fun queryNotificationDetailList(): DetailResponse {
        return detailApi.queryNotificationDetail(getUserId())
    }

    @Operation(summary = "전송된 알림 목록")
    @PostMapping("/{notification-uuid}")
    fun checkNotification(@PathVariable("notification-uuid") notificationId: UUID) {
        detailApi.checkNotification(getUserId(), notificationId)
    }

}
```

Controller에서 사용되는 Annotation을 아래에 정리했다.

#### @Tag

- Api **그룹** 설정을 위한 어노테이션.
- name 속성으로 태그의 이름을 설정할 수 있다.
- description 속성으로 태그에 대한 설명을 추가할 수 있다.

#### @Operation

- Api 상세 정보 설정을 위한 어노테이션.
- summary 속성으로 api의 간략한 설명을 적을 수 있다.
- description 속성으로 api에 대한 상세 설명을 적을 수 있다.
- responses, parameters 속성을 추가로 설정할 수 있다.

#### @ApiResponses

- 여러 개의 @ApiResponse를 묶는 어노테이션이다.

#### @ApiResponse

- Api의 Response 설정을 위한 어노테이션.
- responseCode 속성으로 Http status code를 설정할 수 있다.
- description 속성으로 response에 대한 설명을 추가할 수 맀다.
- **기본적으로 반환되는 값**은 **Swagger가 코드를 분석해서 등록한다.**

#### @Parameter

- Api의 파라미터 설정을 위한 어노테이션.
- name 속성으로 파라미터의 이름을 설정할 수 있다.
- description 속성으로 파라미터에 대한 설명을 추가할 수 있다.
- in 속성으로 파라미터의 위치(Header, Query, Path, Cookie 등)를 설정할 수 있다.

### 4️⃣ Spring Security 설정

Spring Security를 같이 사용한다면, permitAll 설정을 해줘야하는 엔드포인트가 존재한다.

- `/swagger-ui/**`
- `/v3/api-docs/**`

**위 두 엔드포인트는 꼭 permitAll을 해줘야한다.**

### 마지막으로..

Spring doc의 default ui path는 `BASE_PATH/swagger-ui/index.html` 이다.
