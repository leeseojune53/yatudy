# sub-project jacoco report ํฉ์น๊ธฐ

### ๐ ์์ํ๊ธฐ ์ ์...

Spring Boot ๋๋ Java/Kotlin์ผ๋ก ํ๋ก์ ํธ๋ฅผ ์งํํ  ๋ ๊ฐ ๋๋ฉ์ธ๋ณ๋ก ํ๋ก์ ํธ๋ฅผ ๋๋๋ ๊ฒฝ์ฐ ๋๋ ๋๋ฉ์ธ / ์ธํ๋ผ๋ก ํ๋ก์ ํธ๋ฅผ ๋๋๋ ๊ฒฝ์ฐ๊ฐ ์๋ค.

์ด ์ํฉ์์ jacoco๋ฅผ ์ด์ฉํด์ Code Coverage๋ฅผ ๋ถ์ํ  ๋ ๊ฐ ์๋ธ ํ๋ก์ ํธ๋ณ build ๋๋ ํ ๋ฆฌ์ Report๊ฐ ์ ์ฅ๋๋๋ฐ, ์ด๋ฌ๋ฉด github actions์์ Third Party ์๋น์ค๋ฅผ ์ด์ฉํ  ๋ Report๋ฅผ ์ ๋๋ก ์ ๋ฌํ์ง ๋ชปํ๊ฑฐ๋, ์ฌ๋ฌ ํ์ผ์ ์ ๋ฌํด์ผํ๋ ๊ฒฝ์ฐ๊ฐ ๋ฐ์ํ๋ค.

์ด ๊ธ์ ์ ์ํฉ์์ jacoco์ Test Report๋ฅผ ๋ฃจํธ build ๋๋ ํ ๋ฆฌ์ ํฉ์น๋ ๋ฐฉ๋ฒ์ ๋ํ ๊ธ์ด๋ค.

### 1๏ธโฃ ์ฒซ ๋ฒ์งธ๋ก..

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

build.gradle์์ ์์ ๊ฐ์ด ์ค์ ์ ํด์ค๋ค. ๊ฐ๋จํ๊ฒ Jacoco task๊ฐ ์กด์ฌํ๋ฉด ๋ฃจํธ์ ์ ์ฅํ๋ ๊ฒ์ด๋ค.