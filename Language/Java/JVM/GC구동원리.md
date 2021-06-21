# GC(Garbage Collector) 구동원리

[TOC]

## GC란?

**유효하지 않은 메모리**인 가비지가 발생하게 된다. C언어를 이용하면 free()라는 함수를 이용해서 직접 메모리를 해제해주어야한다. 하지만 Java를 이용해 개발을 하면 개발자가 메모리를 직접 해제해주는 일이 없다. **JVM의 가비지 컬렉터가 불필요한 메모리를 알아서 정리**해주기 때문이다. Java에서 명시적으로 불필요한 데이터를 표현하기 위해서 일반적으로 null을 선언해준다.

```java
Test test = new Test();		(1)
test = null;

//Garbage
test = new Test();			(2)
```

1번에서 생성된 test 객체는 더이상 참조를 하지 않고 사용이 되지 않아서 Garbage가 되었다. Java에서는 이러한 메모리 누수를 방지하기 위해서 GC가 주기적으로 검사하여 메모리를 청소해준다.

## Root Set과 Garbage

![img](https://blog.kakaocdn.net/dn/9ztB2/btqw6nLczwh/FA6H5Kh9wcDHylRMxP3sA0/img.png)

Object의 사용여부는 Root Set과의 관계로 판단하게 되는데 Root Set에서 어떤 식으로든 참조 관계가 있다면 Reachable Object라고 하며 이를 현재 사용하고 있는 Object로 간주하며, 그 밖의 Object는 unreachable Object가 된다.

### Root Set의 Reachable Object 판별법

1. Local variable(지역 변수) Section, Operand Stack에 Object의 참조 정보가 있다면 Reachable Object이다.
2. Method Area에 로딩된 클래스 중 constant pool에 있는 참조 정보를 토대로 Thread에서 직접 참조하지 않지만 constant pool을 통해 간접으로 참조되고 있는 Object는 Reachable Object이다.
3. 아직 메모리에 남아 있으며 native Method Area로 넘겨진 Object의 참조가 JNI 형태로 참조 관계가 있는 Object는 Reachable Object이다.

## Minor GC와 major GC

JVM의 Heap영역은 처음 설계될 때 다음의 2가지를 전제로 설계되었다.

- 대부분의 객체는 금방 접근 불가능한 상태(Unreachable)가 된다.
- 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.

즉, 객체는 대부분 **일회성**되며, 메모리에 오랫동안 남아있는 경우는 **드물다**는 것이다. 객체의 생존 기간에 따라 물리적인 Heap 영역을 나누게 됐는데, 이에 따라 young, Old 총 2가지 영역으로 설계되었다.

![img](https://blog.kakaocdn.net/dn/va8qQ/btqUSpSocbS/kxTvtnmrdhf4bnVPXth0UK/img.png)

GC 영역 & 흐름

### Young 영역(Young Generation)

- 새롭게 생성된 객체가 할당(Allocation)되는 영역
- 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 많은 객체가 Young 영역에 생성되었다가 사라진다.
- Young 영역에 대한 가비지 컬렉션을 Minor GC라고 부른다.

### Old 영역(Old Generation)

- Young 영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역
- 복사되는 과정에서 대부분 Young 영역보다 크게 할당되며, 크기가 큰 만큼 가비지는 적게 발생한다.
- Old 영역에 대한 가비지 컬렉션을 Major GC 또는 Full GC라고 부른다.



예외적인 상황으로 Old 영역에 있는 객체가 Young 영역의 객체를 참조하는 경우도 존재할 것이다. 이러한 경우를 대비하여 Old 영역에는 512 bytes의 덩어리로 되어 있는 카드 테이블이 존재한다.

![img](https://blog.kakaocdn.net/dn/FOLU3/btqUOBF35cJ/BMKuD1iqfq6R0lAqMlfkC0/img.png)

카드 테이블에는 Old 영역에 있는 객체가 young 영역의 객체를 참조할 때 마다 그에 대한 정보가 표시된다. 카드 테이블이 도입된 이유는 Young 영역에서 Minor GC가 실행될 때 모은 Old 영역에 존재하는 객체들을 검사하여 참조되지 않은 Young 영역의 객체를 식별하는 것이 비효율적이기 때문이다. 그렇기 때문에 Young 영역에서 GC가 진행될 때 카드 테이블만 조회하여 GC의 대상인지 식별할 수 있도록 하고있다.

## GC의 동작 방식

Young 영역과 Old 영역은 서로 다른 메모리 구조로 되어있기 때문에, 세부적인 동작 방식은 다르다. 하지만 기본적으로 가비지 컬렉션이 실해오딘다고 하면 다음의 2가지 공통적인 단계를 따르게 된다.

### 1. Stop The World

Stop The World는 GC를 실행하기 위해 **JVM이 애플리케이션의 실행을 멈추는 작업**이다. GC가 실행될 때는 **GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단**되고, GC가 완료되면 작업이 재개된다. 당연히 모든 쓰레드들의 작업이 중단되면 애플리케이션이 멈추기 때문에, GC의 성능 개선을 위해 튜닝을 한다고 하면 보통 stop-the-time의 시간을 줄이는 작업을 하는 것이다. 

### 2. Mark and Sweep

- Mark : 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업
- Sweep : Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업

Stop The World를 통해 모든 작업을 중단시키면, GC는 스택의 모든 변수 또는 Reachable 객체를 스캔하면서 각각이 어떤 객체를 참고하고 있는지를 탐색하게 된다. 그리고 **사용되고 있는 메모리를 식별**하는데, 이러한 과정을 Mark라고 한다. 이후에 **Mark가 되지 않은 객체들을 메모리에서 제거**하는데, 이러한 과정을 Sweep라고 한다.

## Minor GC의 동작 방식

Minor GC를 정확히 이해하기 위해서는 Young 영역의 구조에 대해 이해를 해야한다. Young 영역은 1개의 Eden 영역과 2개의 Survivor 영역, 총 3가지로 나뉘어진다.

- Eden 영역 : 새로 생성된 객체가 할당되는 영역
- Survivor 영역  : 최소 1번의 GC 이상 살아남은 객체가 존재하는 영역

객체가 새롭게 생성되면 Young 영역 중에서도 Eden 영역에 할당된다. 그리고 Eden 영역이 꽉 차면 **Minor GC가 발생**하게 되는데, 사용되지 않는 메모리는 해제되고 Eden 영역에서 사용중인 메모리는 Survivor 영역으로 옮겨지게 된다. Survivor 영역은 총 2개이지만 반드시 1개의 영역에만 데이터가 존재해야 한다.

1. 새로 생성된 객체가 Eden 영역에 할당된다.
2. 객체가 계속 생성되어 Eden 영역이 꽉차게 되고 Minor GC가 실행된다.
   1. Eden 영역에서 사용되지 않는 객체의 메모리가 해제된다.
   2. Eden 영역에서 살아남은 객체는 1개의 Survivor 영역으로 이동된다.
3. 1~2번의 과정이 반복되다가 Survivor 영역이 가득 차게 되면 Survivor 영역의 살아남은 객체를 다른 Survivor 영역으로 이동시킨다. (1개의 Survivor 영역은 반드시 빈 상태가 된다.)
4. 이러한 과정을 반복하여 계속해서 살아남은 객체는 Old 영역으로 이동된다.

객체의 생존 횟수를 카운트 하기 위해 Minor GC에서 객체가 살아남은 횟수를 의미하는 age를 object Header에 기록한다. 그리고 Minor GC 때 Object Header에 기록된 age를 보고 Promotion 여부를 결정한다. 또한 Survivor 영역 중 1개는 반드시 사용이 되어야 한다. 만약 두 Survivor 영역에 모두 데이터가 존재하거나, 모두 사용량이 0이라면 현재 시스템이 정상적인 상황이 아님을 파악할 수 있다.

![img](https://blog.kakaocdn.net/dn/Cyho2/btqURvZRql6/4a7u6mMGofkpuURKQz0RT1/img.png)

HotSpot JVM에서는 Eden 영역에 객체를 빠르게 할당(Allocation)하기 위해 bump the pointer와 TLABs(Thread-Local Allocation Buffers)라는 기술을 사용하고 있다. bump the pointer란 **Eden 영역에 마지막으로 할당된 객체의 주소를 캐싱해두는 것**이다. bump the pointer를 통해 새로운 객체를 위해 유효한 메모리를 탐색할 필요 없이 마지막 주소의 다음 주소를 사용하게 함으로써 속도를 높이고 있다. 이를 통해 새로운 객체를 할당할 때 객체의 크기가 Eden 영역에 적합한지만 판별하면 되므로 빠르게 메모리 할당을 할 수 있다.

--추가

## Major GC의 동작 방식

Young 영역에서 오래 살아남은 객체는 Old 영역으로 Promotion됨을 확인할 수 있었다. 그리고 Major GC는 **객체들이 계속 Promotion되어 Old 영역의 메모리가 부족해지면**발생하게 된다. Young 영역은 일반적으로 Old 영역보다 크기가 작기 때문에 GC가 보통 0.5초에서 1초 사이에 끝난다. 그렇기 때문에 Minor GC는 애플리케이션에 크게 영향을 주지 않는다. 하지만 Old 영역은 Young  영역보다 크며 Young 영역을 참조할 수도 있다. 그렇기 때문에 Major GC는 일반적으로 Minor GC보다 시간이 오래걸리며, 10배 이상의 시간을 사용한다.
