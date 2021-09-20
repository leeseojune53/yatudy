# ELK란?

### 📌 정의

로그 데이터 분석 도구이다.

E = Elastic Search, 분석 및 저장

L = Logstash, 로그 수집

K = Kibana, 시각화 도구

#### 🧪 ElasticSearch

Logstash를 통해 수신된 데이터를 저장소에 **저장**하는 역할.

분산 검색엔진.

#### 🎞 Logstash

수집할 로그를 선정해서, 지정된 서버에 인덱싱하여 **전송**하는 역할.

#### ✨ Kibana

데이터를 **시각적**으로 탐색, 실시간으로 **분석**하는 역할

🔗

### ELK Stack

위의 ELK에 Beats가 추가된 것.

#### 🧶 Beats

에이전트로 설치하여 다양한 유형의 데이터를 ElasticSearch 또는 Logstash에 전송하는 데이터 발송자.