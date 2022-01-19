# JPA에서 커스텀 delete... 함수를 사용할 때 발생하는 예외 해결법

### 🐛 문제 상황

delete... 메소드를 Repository에 선언해서 사용하는데 `No EntityManager with actual transaction available for current thread - cannot reliably process 'remove' call` 예외가 발생한다.

### 🏴‍☠️ 원인

JpaRepository의 구현체인 SimpleJpaRepository를 들어가보면 delete 함수에는 `@Transactional` 어노테이션이 붙어있지만, **커스텀 메소드는 찾아볼 수 없다.**

### ♻ 해결법

Repository에서 **커스텀** delete... 메소드 위에 `@Transactional`을 부착해주거나 사용하는 usecase의 메소드에서 `@Transactional`를 부착해주면 된다.