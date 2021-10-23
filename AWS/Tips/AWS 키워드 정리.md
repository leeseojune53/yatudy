# AWS 키워드 정리

![OPGG_VPC.png](https://raw.githubusercontent.com/leeseojune53/yatudy/bfa3147e122d1f803d0e4711e86363e6209e0257/images/AWS%20Architecture.svg)

### AZ(Availability Zone) : 가용영역

가용영역은 물리적으로 유의미한 거리를 둔 각 각의 영역을 이야기하는 것이다.

서울 리전(한국)을 예시로 들면, 광화문에 A AZ가 있다면, B AZ는 수원과 같이 물리적으로 유의미한 거리를 두고 있다.

천재지변, 통신 장애 등에 대응하기 위해서 사용하고, 가용영역을 활용하면 서비스의 가용성이 높아진다.

### Route 53

Route 53은 들어온 Request를 라우팅 해주는 도구이다. Route 53을 사용하면 요청을 다른 Aws 도구(RDS, EC2, Lambda) 등에게 편리하게 보낼 수 있다.

위 사진에서는 Server Request인지, Static Request인지 구분해서 라우팅을 해주었다.

### Cloud Front(CDN)

Aws의 CDN 서비스이다. 엣지 로케이션이라고 하는 여러 개의 엣지가 존재한다.

S3 버킷에 있는 정보에 직접 접근하는 것이 아니라 Cloud Front에 캐싱되어있는 정보에 접근한다.

사용자가 요청하면, 사용자에서 가장 가까운 엣지 로케이션으로 요청이 들어간다.

### S3

컴퓨터의 하드디스크에 값을 저장하듯이 S3는 그냥 정적인 파일을 저장하는 스토리지이다.

위에서 사용했듯이 FrontEnd의 빌드파일을 올려 배포를 할 수도있고, 이미지를 저장하는 용도로 사용할 수도 있다.

### ELB(ALB)

ELB에는 여러 Type이 존재하는데 가장 많이 사용하는 Type이 ALB(Application Load Balancer)이다.

말 그대로 로드 밸런서이다.

### VPC

가상 네트워크를 만드는데 사용한다.

#### Route table

컴네시간에 배웠었다.

VPC로 들어온 요청을 서브넷으로 뿌려주는 역할을 한다.

### Security Group

인바운드와 아웃바운드 정책을 설정해서 서브넷으로 들어온 요청을 걸러주는 역할을 한다.

허용할 포트번호, 허용할 IP 

