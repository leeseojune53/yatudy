# AWS SQS란?

### 📌 정의

- SQS는 Simple Queue Service의 약자
- 애플리케이션 간의 메시지를 저달하기 위한 아주 **간단한** Queue 라고 생각하면 된다.
- 지속성이 우수하고, 사용 가능한 보안 호스팅 대기열을 제공하며, dead-letter queue, 표준대기열, FIFO 대기열을 지원하고 있다.

## SQS와 MQ의 차이점

- SQS는 이름 그대로 '간단한' 큐 서비스이다. 다른 MQ에 존재하는 message routin, fan-out, distribution lists를 지원하지 않는다. 메시지 생산자가 만들어낸 메시지를 메시지 소비자가 가져갈 수 있게 해주는 것이 전부니다.
- Amazon MQ는 AMQP나 MQTT처럼 표준화된 여러 broadcast 프로토콜을 완벽히 지원하는 fully managed 서비스이다. 복잡한 요구사항을 구현할 때 유용하며, AWS 외부에 있는 메시지 브로커를 AWS로 마이그레이션 할 때 유용하다.