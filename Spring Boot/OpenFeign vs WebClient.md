# OpenFeign vs WebClient

### 😁 공통점

Spring Boot에서 Api 요청을 할 수 있는 Client 역할을 한다.

### 😵 차이점

가장 큰 차이점은 Blocking과 Non-Blocking이다.

WebClient는 reactor를 사용하는 WebFlux에 속해있다.

OpenFeign도 3th party Library를 통해서 Non-Blocking으로 사용할 수 있지만, 프로젝트의 기본 뼈대가 Non-Blocking이라면 WebClient를 추천한다.