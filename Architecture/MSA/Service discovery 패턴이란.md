# Service discovery 패턴이란?

### 📌 정의

클라이언트가 서비스를 호출할 때 서비스의 위치를 알아낼 수 있는 기능을 말한다.

### ❓ Service Registery

각 서비스의 정보(IP, 포트번호)를 가지고있는 Registery이다.

Spring Cloud의 Eureka나 Consul와 같은 서비스이다.

### 1️⃣ Client side discovery

<img src="https://t1.daumcdn.net/cfile/tistory/99E912455AD610DE09" alt="img" style="zoom:50%;" />

클라이언트가 Service registery를 호출해서 서비스의 위치를 확인하고 호출하는 것.

### 2️⃣ Server side discovery

<img src="https://t1.daumcdn.net/cfile/tistory/99AF813E5AD610DE03" alt="img" style="zoom:50%;" />

서비스 앞에 로드밸런서를 배치함으로써 클라이언트가 서비스를 호출하면 로드밸런서가 Service registery를 호출하고, 해당 정보로 라우팅하는 방식이다.



## Reference

[MSA에서 Service discovery 패턴](https://bcho.tistory.com/1252)