# List안의 값을 Validation 하는 법

### 🐛 문제 상황

List 안의 value를 검증해야하는 상황인데 검증을 못함.

### 🏴‍☠️ 원인

List 변수 위에 validation 어노테이션을 붙이면 그 List에 대한 것을 검증하기 때문이다.

### ♻ 해결법

Java 8부터 지원된 어노테이션을 타입에 사용할 수 있는점을 활용하면 된다.

예를 들어 `List<String> `자료형이라면 `List<@Size(max = 4) String>`와 같이 사용하면 된다.