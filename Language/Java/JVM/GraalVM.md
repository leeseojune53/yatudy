# GraalVM이란?

### 장점

![Graalvm 엔터프라이즈 네이티브](https://www.oracle.com/a/ocom/img/rc24-graalvm-enterprise-native.jpeg)

일반 JVM에 비해 실행속도와 사용 메모리가 굉장히 효율적이다.



GraalVM은 컴파일 모드(Compiler Operating Mode)로 JIT Compiler와 AoT Compiler를 사용할 수 있다.

기본적으로 AoT(libgraal)를 사용하며, Command Line에 `-XX:-UseJVMCINativeLibrary`를 사용해서 JIT(jargraal)을 적용할 수 있다.

클라우드 환경에서 AoT(Ahead-of-Time)를 활용해서 Warm-Up 시간을 줄일 수 있다.