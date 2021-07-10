# SNS vs SQS

## SNS(Simple Notification Service)

1. publisher(게시자)가 Subscriber(구독자)에게 메세지를 전송하는 관리형 서비스

2. Publisher는 Topic(주제)에 메세지를 발행한다. Topic은 수많은 Subscribers에게 전달될 수 있다.(fan out) 이때 전달 방식은 Lambda, SQS, Email 등 여러가지가 있다.

3. 다른 시스템들이 이벤트에 신경쓰는가? 

   Topic에 메세지를 publish하고싶어하고, 사람들에게 발행됐다고 알리고 싶을 때

## SQS(Simple Queue Service)

1. 마이크로서비스, 분산 시스템 및 서버리스 애플리케이션을 쉽게 분리하고 확장할 수 있도록 지원하는 완전관리형 메세지 대기열 서비스

2. 시스템은 Queue로 부터 새로운 이벤트를 실시할 수 있다. Queue에 있는 메세지들은 한 명의 consumer 또는 하나의 서비스에서 실행된다.

3. 이 시스템이 이벤트에 신경쓰는가?

   내가 이벤트의 수신자일 때

