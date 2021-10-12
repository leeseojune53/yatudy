# MQ(Message Queue)란?

### 🎊 시작하기 전에..

Message Queue를 설명하려면 우선 [MOM](https://github.com/leeseojune53/yatudy/blob/main/Internet/Message%20Queue/MOM.md), [Message Broker](https://github.com/leeseojune53/yatudy/blob/main/Internet/Message%20Queue/Message%20Broker.md)를 알고있어야 합니다.

MOM과 Message Broker에 대한 설명은 따로 없는점 양해 바랍니다.

### 📌 정의

MQ란 메시지 기반의 미들웨어로 메시지를 이용하여 여러 애플리케이션, 시스템, 서비스들을 연결해주는 솔루션이다.

MOM를 구현한 솔루션으로 **비동기 메시지**를 사용하는 서비스들 사이에서 **데이터를 교환해주는 역할**을 한다.

MQ를 사용하여 **비동기로 요청을 처리**하고 **queue에 저장**하여 consumer에게 병목을 줄여줄 수 있다.

Message Broker에서 Message 값을 저장하는 역할을 한다.

MOM은 메시지 전송 보장을 해야하므로 AMQP를 구현한다.

### AMQP(Advanced Message Queueing Protocol)

- 메시지를 안정적으로 주고받기 위한 **인터넷 프로토콜**이다.

### 😊 장점

- 서비스간의 결합성이 낮아지므로 각자의 비즈니스 로직에만 집중
- 메시지 처리 방식은 **Message Broker에 위임**
  - 각 서비스는 Client를 통해 메시지를 보내고 받기만 하면 됨
- 각 서비스는 비동기 방식으로 메시지를 보내기만 하면, Message Broker에서 순서 보장, 메시지 전송 보장등을 처리
- 메시징 시스템이 **잠깐 다운되어도 각 서비스에는 직접적인 영향을 미치지 않는다.**



### 😵 단점

- Message Broker 구축, 예를 들면 kafka 클러스터 구축에 필요한 금전, 인적자원에 대한 비용
- 비동기의 양면성 - 정말 메시지가 잘 전달되었는가?
- 함수 호출, 공유메모리 사용 방식보다 메시징 시스템을 사용했을 때 호출 구간이 늘어나므로 네트워크 비용 발생