# MOM(Message Oriented Middleware)이란?

### 📌 정의

독립된 애플리케이션 간에 데이터를 주고받을 수 있도록 하는 **시스템 디자인**

**비동기**로 메시지를 **교환**할 수 있게해서 **서비스간 결합성을 낮춘**다.

### 🔀 여러가지 메세지 전달 방식들

#### 📋 Topic 방식

Pub/Sub 구조라고 말한다.

메시지를 발행하는 Publisher(Producer), 메시지를 소비하는 Subscribe(Consumer)로 구성되어있다.

Message를 Publish한 후, 해당 Message를 누가 얼마나 사용하는지 신경쓰지 않는다.

많은 Consumer가 붙어서 **동시에 해당 데이터를 소비**할 수 있다.

#### 🎞 Queue 방식

point-to-point 방식이라고도 말한다.

메시지 큐에 넣어둔 메시지를 한번 **consume**하면 **queue에서 삭제**된다.