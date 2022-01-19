# Embedded Redis 사용 시 에러

### 🐛 문제 상황

실행 시 ```Can't start redis server. Check logs for details.``` 가 발생함.

### 🏴‍☠️ 원인

localhost에서 실행되고 있는 redis의 port와 embedded-redis가 겹쳐서 발생.

### ♻ 해결법

embedded-redis의 port를 localhost에서 사용중인 port외 다른 port로 교체한다.