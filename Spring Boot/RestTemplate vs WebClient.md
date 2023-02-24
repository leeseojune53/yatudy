# RestTemplate vs WebClient

### 😁 공통점

Http 호출을 할 수 있는 Client 역할을 합니다.

### 😵 차이점

가장 큰 차이점은 Blocking과 Non-Blocking입니다.

WebClient는 reactor를 사용하는 WebFlux에 속해있습니다. 그에반해서 RestTemplate은 Blocking입니다.



OpenFeign은 OkHTTP와 같은 Client를 Binding해주는 라이브러리입니다.

