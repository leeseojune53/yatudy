# RestTemplate vs WebClient

### 😁 공통점

Spring Boot에서 Api 요청을 할 수 있는 Client 역할을 한다.

### 😵 차이점

가장 큰 차이점은 Blocking과 Non-Blocking이다.

WebClient는 reactor를 사용하는 WebFlux에 속해있다. 그에반해 RestTemplate은 Blocking이다.



OpenFeign은 OkHTTP와 같은 Client를 Binding해주는 라이브러리입니다.

