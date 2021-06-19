# GC(Garbage Collector) 구동원리

## GC란?

**유효하지 않은 메모리**인 가비지가 발생하게 된다. C언어를 이용하면 free()라는 함수를 이용해서 직접 메모리를 해제해주어야한다. 하지만 Java를 이용해 개발을 하면 개발자가 메모리를 직접 해제해주는 일이 없다. **JVM의 가비지 컬렉터가 불필요한 메모리를 알아서 정리**해주기 때문이다. Java에서 명시적으로 불필요한 데이터를 표현하기 위해서 일반적으로 null을 선언해준다.

```java
Test test = new Test();		(1)
test = null;

//Garbage
test = new Test();			(2)
```

1번에서 생성된 test 객체는 더이상 참조를 하지 않고 사용이 되지 않아서 Garbage가 되었다. Java에서는 이러한 메모리 누수를 방지하기 위해서 GC가 주기적으로 검사하여 메모리를 청소해준다.

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

