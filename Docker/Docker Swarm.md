# Docker Swarm이란?

### 📌 정의

도커가 공식적으로 만든 오케스트레이션 툴.

도커 컨테이너를 위한 클러스터링, 스케줄링 툴이다. 스웜을 이용해서 여러 개의 서버와 컨테이너 관리를 쉽게 할 수 있다.

또한 **매니저 노드**와 **작업자 노드**가 존재한다.

### 👨‍🎓 매니저 노드

매니저 노드는 아래의 업무를 통해 도커 클러스터를 관리한다.

**매니저 노드 역시 작업자에 속하긴 한다.**

- 클러스터의 상태를 유지 : 뗏목 알고리즘 사용한다.
- 스케줄링 서비스 : 작업자 노드에게 컨테이너를 배포한다. 특정 노드에게만 배포하거나, 모든 노드에 하나씩 배포할 수도 있다.
- 스웜 모드 제공

### 👷‍♂️ 작업자 노드

도커에서 일반적으로 컨테이너를 실행하는 노드를 작업자 노드라고 한다.

작업자 노드들의 클러스터는 반드시 하나 이상의 관리자 노드를 가져야 한다. 