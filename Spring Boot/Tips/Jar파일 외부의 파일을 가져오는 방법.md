# Jar파일 외부의 파일을 가져오는 방법

### 🐛 문제 상황

Docker Container에서 Spring Boot 프로젝트를 실행시키는데 `ClassPathResource ` 를 사용하면 `class path resource [파일명] cannot be opened because it does not exist` 예외가 발생함.

### 🏴‍☠️ 원인

Jar내부의 파일이 아닌 Jar파일 외부에 위치하는 파일을 가져와야 하므로 발생함.

### ♻ 해결법

가져와야하는 파일의 경로 앞에 `file:///`를 붙여준다.

### 😎 예시

#### 기존 코드

```java
new ClassPathResource("/root/filename.json")
```

#### 해결 코드

```java
new ClassPathResource("file:///root/filename.json")
```

