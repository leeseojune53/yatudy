# ELK Stack을 도커로 구축해보자!

### 🎊 시작하기 전에..

이 글을 읽기 전에 Docker의 기본적인 작동원리, Docker Network에 대한 지식이 있어야합니다.

또한 Docker가 Server에 install되어있어야합니다.

### 1️⃣ 가장 먼저

https://github.com/EntryDSM/BELK

위의 링크를 server에 clone 받습니다.

`git clone https://github.com/EntryDSM/BELK`

### 2️⃣ 두 번째로

해당 디렉토리 내부로 들어가서 `docker-compose.yml` 파일을 엽니다.

`ELASTIC_PASSWORD:`를 설정해줍니다. 기본값은 `changeme` 입니다.

yml 파일 내부의 filebeat설정에서 두 번째 volume설정에서 `source`는 host에서 로그의 위치, `target`는 container에서 로그의 위치를 설정합니다.

### 3️⃣ 세 번째로

logstash/config로 들어가서 `logstash.yml`에서 `xpack.monitoring.elasticsearch.password`를 위에서 설정했던 비밀번호로 교체해줍니다.

kibana/config에서도 동일합니다.

### 4️⃣ 네 번째로

filebeat/config로 들어가서 `filebeat.yml`에서 `paths`를 설정해줍니다. paths의 값은 두 번째에서 설정해준 target의 위치를 설정해주면 됩니다.

### 5️⃣ 다섯 번째로

logstash/pipeline에서 `logstash.conf`에서 filter 내부는 로그 파일의 형식에 맞게 작성하면 됩니다.

`output`에서 `password`는 두 번째에서 설정했던 password를 기입해주면 됩니다.

### ✅ 끝으로

위에서 구축한 ELK Stack의 도식도이다.

![img](https://raw.githubusercontent.com/leeseojune53/yatudy/bfa3147e122d1f803d0e4711e86363e6209e0257/images/Log%20analysis%20flow.svg)