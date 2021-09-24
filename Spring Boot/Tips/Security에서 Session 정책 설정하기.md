# Security에서 Session 정책 설정하기

### 🎊 시작하기 전에..

필자는 Spring Boot Security의 기본적인 구조를 이해한 후 이 글을 읽는것을 추천한다.

### 📌 세션의 정의

세션은 일정 시간동안 유저(하나의 브라우저)로 들어오는 요청을 하나의 상태(연결형)로 생각하고, 상태를 일정하게 유지하는 기술이다. 

### 📌 Security Session Policy

- ALWAYS : 항상 세션 필요
- IF_REQUIRED : 필요시 생성
- NEVER : 생성하지 않음(있을 시 사용)
- **STATELESS** : 생성 및 사용하지 않음(JWT와 같은 토큰 인증방식 사용 시 설정)