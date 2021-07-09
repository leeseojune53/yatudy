# SNS(Simple Notification Service)

구독 중인 엔드포인트 또는 클라이언트에 메시지 전달을 조성 및 관리하는 웹 서비스이다.

MOM을 구현한 Message Broker라고 보면 될 것 같다.

이벤트를 생산하는 쪽을 게시자(Publisher)라고 하고, 이벤트를 구독하는 쪽을 구독자(Subscriber)라고한다.

![img](https://t1.daumcdn.net/cfile/tistory/996BE4415C2362ED20)

AWS SNS는 구독자(SQS / Lambda 등)에게 전송된 메시지 전달 상태에 log를 남겨준다.

