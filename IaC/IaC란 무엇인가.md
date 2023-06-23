# IaC란 무엇인가

### 📌 IaC란?
Infrastructure as Code의 준말로 수동 프로세스가 아닌 코드를 통해 인프라를 관리하고 프로비저닝 하는 것을 말한다.

### 👍 장점
1. 일관성 : 매번 동일한 환경을 프로비저닝하도록 보장한다.
2. Git과 같은 버전 관리 시스템을 사용할 수 있다.
3. 휴먼 에러가 감소한다.

### 👎 단점
1. 프레임워크에 따라서 일부코드로 관리가 되지 않는 영역이 존재한다.
2. 다중 계정 운영 방식, 망분리(ISMS), 협업 등등에 대한 Best Practice 자료가 부족하다.

### 종류
IaC의 종류로는 Terraform, Ansible, Chef, Puppet, CDK, CloudFormation / SAM 등이 존재한다.