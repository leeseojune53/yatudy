# 부모 객체에서 자식 객체를 가져올 때 no Session 에러 (parallelStream)

### 🐛 문제 상황

**parallelStream**로 루프를 돌리고 있을 때 해당 루프 내에서 **자식 객체를 가져오려고 했을 때** 발생한다.

### 🏴‍☠️ 원인

parallelStream는 java 8부터 해당 Collection을 병렬 처리할 수 있게 만드는 기능인데, Hibernate의 Session은 Thread별로 생성이 된다. 따라서 여러 스레드에서 작업하는 parallelStream은 no Session이 뜨는 것이 정상적인것이다.

### ♻ 해결법

parallelStream 대신 stream을 사용하거나 parallelStream을 들어가기 전에 **EAGER와 같이 데이터를 미리 다 들고**오면 된다.