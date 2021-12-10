# Swarmpit이란?

### 📌 정의

Docker Swarm을 관리하기 쉽게 GUI형태로 확인할 수 있는 도구이다.

### ♻ Auto Deploy

Swarmpit의 Service에서 DEPLOYMENT탭을 들어가보면 **Autoredeploy**가 있는데, 이는 **주기적**으로 도커 이미지가 업데이트되었는지 확인하는 것인데, 해당 해시값과 현재 이미지의 **해시값을 비교**해서 다르면 업데이트를 진행한다.