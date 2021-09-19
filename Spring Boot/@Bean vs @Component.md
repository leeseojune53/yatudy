# @Bean vs @Component

### 😁 공통점

Bean을 생성하는 어노테이션.

### 😵 차이점

@Bean 어노테이션의 docs를 가보면 `ElementType.METHOD, ElementType.ANNOTATION_TYPE`가 되어있고, @Component 어노테이션의 docs를 가보면 `ElementType.TYPE`으로 되어있다.

따라서 개발자가 직접 **수정**이 **가능**한 클래스에는 @Component를 사용하고, 라이브러리를 사용할 경우에는 해당 **인스턴스**를 생성하는 메소드 위에 @Bean을 사용한다.

간단하게 @Bean 어노테이션은 **method** 위에 붙일 수 있고, @Component 어노테이션은 **class**위에 붙일 수 있다.

