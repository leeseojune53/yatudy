# Kafka란?

### 📌 정의

**Topic, Pub-Sub 모델**의 MQ이다. **분산환경에 특화**되어있는 특징을 가지고 있다.

### ⭐ 개념

1. Event

   Kafka에서 Producer와 Consumer가 데이터를 주고 받는 단위, 메시지이다.

2. Producer

   Kafka에서 Topic에 이벤트를 Post하는 클라이언트 어플리케이션이다.

3. Consumer

   Kafka에서 Topic을 구독하고, Topic에서 얻은 이벤트를 처리하는 클라이언트 어플리케이션이다.

4. Topic

   이벤트가 쓰이는 곳이다.

   파일시스템의 폴더와 유사하고, 이벤트는 폴더 안의 파일과 유사하다.

   Topic에 저장된 이벤트는 **필요한 만큼 다시 읽을 수 있다**.

