

# QueryDSL Transform Connection Leak

회사에서 코드를 작성하다 쿼리 결과의 일부 구조 자체를 수정해야하는 상황이 생겨서 QueryDSL의 구조 변환 함수인 `transform`을 사용하였다.

참고로 `transform`은 쿼리 결과값을 가져온 이후, Application에서 메모리를 사용하여 변환하는 함수이다.

해당 코드가 개발 환경에 배포된 이후 Java Application의 속도가 느려지다 멈추는 현상이 발생하였다. 약 4건이 해당 시간대에 개발 환경에 배포되어서 다 찾아봤는데 따로 의심가는 코드가 없었다.

HikariCP Leak을 검색하니 Querydsl의 transform이 Connection leak을 발생시킨다는 아티클이 있어서 조금 더 파보았고, 실제로 해당 method 상단에 @Transactional 어노테이션을 붙이니 문제가 해결되었다.

### OSIV의 영향?

몇일 후 조금 더 DeepDive하려고 `@Transactional`을 제거하고 10(Connection Pool size)번 호출 하였는데, Application이 멈추지 않았다.

약 2일간 변경된 파일은 180개였는데, 관련있어보이는 파일들을 찾아보다 application 설정에 OSIV(open-in-view property)가 false로 설정되었던게 삭제(기본값 true)로 변경된 것이었다.

OSIV가 켜짐으로써 Persistence Context의 생존 범위가 Dispatcher Servlet까지 늘어나게 되는데, OSIV가 없었을 때는 Service layer를 벗어나도 Connection(Entity Manager)를 Spring Boot가 건드리지 않았는데, OSIV가 켜지면서 Dispatcher Servlet에서 빠져나갈 때 EntityManager를 close하면서 DB Connection도 Close되는 걸로 추측한다.

`OpenEntityManagerInViewInterceptor`의 `afterCompletion`함수에 EntityManager를 close하는 부분이 있다.

![querydsl-leak-1.png](https://github.com/leeseojune53/yatudy/blob/main/images/querydsl-leak-1.png?raw=true)

또, `closeEntityManager`를 5단계 정도 타고 들어가면 `AbstractSharedSessionContract`라는 class가 있다. 여기서 `close()`라는 함수에서 jdbcCoordinator를 Close한다.

해당 함수의 Javadoc에는 아래와 같이 적혀있다.

![querydsl-leak-2.png](https://github.com/leeseojune53/yatudy/blob/main/images/querydsl-leak-2.png?raw=true)

`Close this coordinator and release and resources.` (해당 코디네이터를 닫고, 릴리즈 리소스를 닫는다.)

따라서 OSIV를 키면 EntityManager를 close하는 과정에서 DB Connection도 release되는 사이드이펙트로 transform의 Connection leak이 발생하지 않는다.