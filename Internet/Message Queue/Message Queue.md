# MQ(Message Queue)란?

## MQ란?

MQ란 메시지 기반의 미들웨어로 메시지를 이용하여 여러 애플리케이션, 시스템, 서비스들을 연결해주는 솔루션이다.

MOM(Message Oriented Middleware)를 구현한 솔루션으로 비동기 메시지를 사용하는 서비스들 사이에서 데이터를 교환해주는 역할을 한다.

Producer(sender)가 메시지를 큐에 전송하면 Consumer(receiver)가 처리하는 방식으로, producer와 consumer에 message 프로세스가 추가되는 것이 특징이다.

MQ를 사용하여 비동기로 요청을 처리하고 queue에 저장하여 consumer에게 병목을 줄여줄 수 있다.

### MOM (Message Oriented Middleware, 메시지 지향 미들웨어)

- 독립된 애플리케이션 간에 데이터를 주고받을 수 있도록 하는 **시스템 디자인**
  - 함수 호출, 공유메모리 등의 방식이 아닌, 메시지 교환을 이응하는 중간 계층에 대한 인프라 아키텍쳐 개념
  - 분산 컴퓨팅이 가능해지며, 서비스간 결합성이 낮아짐
- 비동기로 메시지를 전달하는 것이 특징
- Queue, Broadcast, Multicast 등의 방식으로 메시지 전달
- **Pub/Sub** 구조
  - 메시지를 발행하는 Publisher(Producer), 메시지를 소비하는 Subscribe(Consumer)로 구성

### Message BroKer

- 메시지 처리 또는 메시지 수신자에게 메시지를 전달하는 시스템이며, 일반적으로 MOM을 기반으로 구축됨.

### MQ(Message Queue)

- Message Broker와 MOM을 구현한 소프트웨어 ex) RabbitMQ, ActiveMQ, Kafka
- MOM은 메시지 전송 보장을 해야하므로 AMQP를 구현함

### AMQP(Advanced Message Queueing Protocol)

- 메시지를 안정적으로 주고받기 위한 인터넷 프로토콜

## 장점

- 서비스간의 결합성이 낮아지므로 각자의 비즈니스 로직에만 집중
- 메시지 처리 방식은 Message Broker에 위임
  - 각 서비스는 Client를 통해 메시지를 보내고 받기만 하면 됨
- 각 서비스는 비동기 방식으로 메시지를 보내기만 하면, Message Broker에서 순서 보장, 메시지 전송 보장등을 처리
- 메시징 시스템이 잠깐 다운되어도 각 서비스에는 직접적인 영향을 미치지 않는다.



## 단점

- Message Broker 구축, 예를 들면 kafka 클러스터 구축에 필요한 금전, 인적자원에 대한 비용
- 비동기의 양면성 - 정말 메시지가 잘 전달되었는가?
- 함수 호출, 공유메모리 사용 방식보다 메시징 시스템을 사용했을 때 호출 구간이 늘어나므로 네트워크 비용 발생