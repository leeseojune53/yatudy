# @Value값이 null로 들어갈 때 해결법

### 🐛 문제 상황

@Value를 이용해서 값을 넣은 field를 이용해서 생성하는 Object가 있는데, getter를 사용해서 값을 확인해보니 null값이 들어가있음.

### 🏴‍☠️ 원인

@Value의 값이 들어가기 전에 Object가 생성되었기 때문

### ♻ 해결법

`@PostConstruct`를 사용하면 된다.

`@PostConstruct`는 의존성 주입이 이루어진 후 초기화를 수행하는 메소드이다.

