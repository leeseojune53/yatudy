# @Query로 변경, 삭제 메소드를 사용했을 때 발생하는 에러 해결법

### 🐛 문제 상황

`@Query`를 사용해서 Update, Delete 등 변경, 삭제 메소드를 사용했을 때 `Not supported for DMS operations ~~` 에러가 발생한다.

### 🏴‍☠️ 원인

`@Query`를 사용할 때 변경, 삭제가 일어나는 메소드에는 `@Modifying` 어노테이션을 붙여줘야 하기 때문이다.

### ♻ 해결법

`@Query`가 붙어있는 메소드 위에 `@Modifying` 어노테이션을 붙여주면 잘 작동한다.