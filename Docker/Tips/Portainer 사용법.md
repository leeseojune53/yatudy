# Portainer 사용법

### 1️⃣ 가장 먼저

우선 도커 볼륨을 생성한다.

`docker volume create portainer_data`

### 2️⃣ 두 번째로

Portainer 이미지를 다운로드하고, 실행한다.

`docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data --restart=always portainer/portainer`

9000번 포트를 host와 연결해주고, restart option을 이용해서 데몬 시작 시 같이 실행되도록 설정한다.