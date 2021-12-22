# Kopring 시작 설정하기

### 🎊 시작하기전에..

이 글은 코프링(Kotlin + Spring)을 처음 접할 때 제가 어려움을 겪어서 처음 접하시는 분들은 조금 더 쉽게 시작하기 위해 작성한 글입니다.

먼저, **코프링에는 Lombok을 사용하지 않습니다.**



### ⚙ 설정

build.gradle.kts 파일이 Java에서의 build.gradle과 같은 파일입니다.

JPA를 사용한다면 build.gradle.kts파일에서 아래의 코드를 추가해줘야합니다.

```kotlin
plugins {
    ~~~
	kotlin("plugin.jpa") version "1.6.0"
}

~~~

dependencies {
    ~~~
    implementation("org.springframework.boot:spring-boot-starter-data-jpa")
	~~~
}

noArg {
    annotation("javax.persistence.Entity")
    annotation("javax.persistence.MappedSuperclass")
    annotation("javax.persistence.Embeddable")
}

allOpen {
    annotation("javax.persistence.Entity")
    annotation("javax.persistence.MappedSuperclass")
    annotation("javax.persistence.Embeddable")
}
```

위에서 jpa plugin은 **noArg를 사용하기 위해서 추가**하였고, **Kotlin에서 기본 클래스는 final class**이므로 **Entity 등 어노테이션을 allOpen(Open class로 변경)으로해서 상속이 가능**하게 했다.