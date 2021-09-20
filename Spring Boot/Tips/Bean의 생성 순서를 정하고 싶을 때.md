# Bean의 생성 순서를 정하고 싶을 때

### 🐛 문제 상황

AUtil이라는 Bean에서 생성해주는 Object를 B라는 Bean에서 사용하는데 NPE가 발생했다.

### 🏴‍☠️ 원인

AUtil이 생성된 후 B가 생성되어야하는데 순서가 B -> AUtil로 되어서

### ♻ 해결법

`@DependsOn(value = {"먼저 생성되야하는 Bean 이름"})`을 사용한다.

