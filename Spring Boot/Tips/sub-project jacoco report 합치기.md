# sub-project jacoco report 합치기

### 🎊 시작하기 전에...

Spring Boot 또는 Java/Kotlin으로 프로젝트를 진행할 때 각 도메인별로 프로젝트를 나누는 경우 또는 도메인 / 인프라로 프로젝트를 나누는 경우가 있다.

이 상황에서 jacoco를 이용해서 Code Coverage를 분석할 때 각 서브 프로젝트별 build 디렉토리에 Report가 저장되는데, 이러면 github actions에서 Third Party 서비스를 이용할 때 Report를 제대로 전달하지 못하거나, 여러 파일을 전달해야하는 경우가 발생한다.

이 글은 위 상황에서 jacoco의 Test Report를 루트 build 디렉토리에 합치는 방법에 대한 글이다.

### 1️⃣ 첫 번째로..

```kotlin
// build.gradle.kts
...

allprojects {
		...
    apply(plugin = "jacoco")
		...
}

...

tasks.register<JacocoReport>("jacocoRootReport") {
    subprojects {
        this@subprojects.plugins.withType<JacocoPlugin>().configureEach {
            this@subprojects.tasks.matching {
                it.extensions.findByType<JacocoTaskExtension>() != null }
                .configureEach {
                    sourceSets(this@subprojects.the<SourceSetContainer>().named("main").get())
                    executionData(this)
                }
        }
    }

    reports {
        xml.outputLocation.set(File("${buildDir}/reports/jacoco/test/jacocoTestReport.xml"))
        xml.required.set(true)
        html.required.set(false)
    }
}
```

build.gradle에서 위와 같이 설정을 해준다. 간단하게 Jacoco task가 존재하면 루트에 저장하는 것이다.