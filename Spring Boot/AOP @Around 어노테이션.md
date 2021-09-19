# AOP @Around 어노테이션

### 📌 정의

Advice의 한 종류로 핵심 관심사의 실패여부와 상관없이 전 후로 실행되도록 하는 Advice이다.

Advice는 실질적으로 어떤 일을 해야할지에 대한 것, 즉 실질적인 부가기능을 담은 구현체이다.

### 😉 사용법

Pointcut를 전달해주어야 한다. Pointcut는 횡단관심사(부가기능)이 적용될 joinPoint들을 정의한 것이다.

#### 1️⃣ execution

@Pointcut("execution(`접근제어자`, `반환형` `패키지포함 클래스 경로` `메소드` `파라미터`)")

- execution(* *(..))

  접근제어자, 반환형 모두 상관 X, 어떠한 경로에 존재하는 클래스도 상관하지 않고 적용.

  메소드의 파라미터가 개수 상관 X

- execution(* test.spring..*(..))

  접근제어자, 반환형 모두 상관 X, test.spring 하위 패키지, 클래스 적용, 메소드명 상관 X, 파라미터 상관 X

#### 2️⃣ within

패키지 내의 모든 메소드에 적용할 때 사용.

@Pointcut("within(`Class 경로`)")

- within(com.java.ex.*)

  com.java.ex. 하위의 모든 클래스, 모든 메소드

- within(com.java.ex..*)

  com.java.ex. 패키지의 하위 패키지를 포함한 모든 클래스, 메소드

#### 3️⃣ bean

해당 bean id를 가지고있는 bean의 모든 메소드에 적용

@Pointcut("bean(`bean id`)")

- bean(car)

  car라는 bean id를 가지고있는 bean에 적용

#### 4️⃣ 커스텀 어노테이션

@Pointcut("@annotation(`어노테이션 명`)")

