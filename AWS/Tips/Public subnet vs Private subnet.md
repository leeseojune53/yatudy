# Public subnet vs Private subnet

### 🎊 시작하기 전에..

이 글은 Public subnet과 Private subnet의 **차이**를 다루는 글입니다.

### 😵 차이점

바로 이야기하자면, **라우팅 테이블**이 다르다.

Public subnet은 라우팅 테이블(Routing Table)에 **인터넷 게이트웨이**(igw, internet gate way)가 연결되어 있는 것이고, Private subnet은 라우팅 테이블에 **igw가 연결되어있지 않은 것**이다.

igw는 **VPC와 인터넷 간 통신**을 할 수 있게 만들어주는 **통로**이다.