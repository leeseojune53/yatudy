# Spotless

Spotless는 코드의 포멧을 맞춰주는 툴이다.

Gradle에 추가해서 build 시 체크할 수 있고, 포멧에 맞게 수정도 할 수 있다.

옵션은 약 30개 이상 있어서, 사용할거라면 아래 깃허브를 참고하면 좋다.

깃허브 : https://github.com/diffplug/spotless

### 예시

```groovy
    spotless {
        java {
            target("src/main/java/github/**/*.java", ...)
          
          // Import 정렬 순서
            importOrder("lombok", "org", "com", "java", "javax", "io.github")
          // 사용하지 않는 Import 제거
            removeUnusedImports()

          // Custom Rule도 생성할 수 있다.
            custom("noWildcardImports") {
                when {
                    it.contains("*;\n") -> throw Error("No wildcard imports allowed")
                    else -> it
                }
            }
        }
    }
```



