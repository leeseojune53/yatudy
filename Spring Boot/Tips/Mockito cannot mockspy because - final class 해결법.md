# Mockito cannot mock/spy because - final class 해결법

### 🐛 문제상황

**Test 구동 시** `Mockito cannot mock/spy because - final class` 예외가 발생한다.

### 🏴‍☠️ 원인

Mockito가 **final class를 Mocking**하려고해서 발생

### ♻ 해결법

build.gradle.kts에 `testImplementation("org.mockito:mockito-inline:2.13.0")` 추가
