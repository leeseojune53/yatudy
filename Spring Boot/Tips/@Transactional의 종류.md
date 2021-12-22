# @Transactional의 종류

### 🎊 시작하기전에...

Transaction에 대해서 기본적인 지식이 있어야 수월하게 읽을 수 있습니다.

Transactional 어노테이션은 Springframework와 javax에서 제공해줍니다.

### 😵 차이점

Springframework의 transactional은 Springframework에서 제공한다.

하지만, Javax의 transactional은 **자바 표준 확장 패키지**에서 제공한다. 즉 자바에서 기본 제공하는 것이다.

또, Springframework의 Transactional에 더 많은 옵션(readOnly, timeout 등)이 존재한다.

### 💡 결론

@Transactional 어노테이션의 옵션을 많이 사용한다면 Spring framework의 어노테이션을 사용하는 것이 좋다. 하지만, 옵션을 사용하지 않고, Spring version이 4.0 이상이라면 Spring에서 파싱해서 처리하기 때문에 두 개 다 상관 없다.