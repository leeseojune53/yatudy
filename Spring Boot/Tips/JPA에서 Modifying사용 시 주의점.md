# JPA에서 Modifying사용 시 주의점

### 📌 정의

Modifying은 **조회 쿼리를 제외**한 INSERT, UPDATE, DELETE쿼리에서 사용한다.

**Persistence Context를 무시**하고 바로 **데이터베이스에 접근**한다.

### ⚠️ 주의점

JPA와의 싱크 문제가 발생할 수 있다.

JPA의 Persistence Context에 저장되어있는 객체가 Modifying사용으로 DB에서 변경된 후, 해당 객체를 조회하면 변경된 객체가 아닌 Persistence Context에 캐시되어있는 객체를 반환함으로써 싱크 문제가 발생한다.

### ♻️ 해결방법

Modifying 어노테이션에는 clearAutomatically라는 옵션이 존재하는데, 해당 옵션은 Modifying이 붙어있는 메소드 실행 후 Persistence Context를 Clear해줌으로써 추후에 조회했을 때 DB에 다시 접근하게 된다.