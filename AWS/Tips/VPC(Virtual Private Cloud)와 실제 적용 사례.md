# VPC(Virtual Private Cloud)와 실제 적용 사례

하나의 VPC는 여러 개의 보안 그룹을 가질 수 있으며, 여러개의 서브넷을 가질 수 있다.

서브넷은 하나의 Region에 여러 개 존재할 수 있다. 기본적으로는 각 AZ(가용영역)별로 존재한다.

서브넷은 라우트 테이블을 가지고 있다.



기본적으로 제공하는 VPC의 라우트 테이블은 0.0.0.0/0, 즉 모든 IPv4를 허용하므로 해킹에 취약할 수 밖에 없다. 따라서 보안을 신경쓴다면 Default VPC보다는 Custom VPC가 낫다고 생각한다. 물론 대략적인 지식이 있다는 가정하에.



필자는 Route Table에서 IGW 전체를대상으로 라우팅을 해준다면(모든 IPv4 허용), SG(Security Group)에서라도 방어해야한다고 생각한다.



아래는 OPGG 해커톤에서 구축한 AWS의 구조이다.

외부의 요청이 Route53을 통해서 정적 데이터 접근은 AWS의 CDN서비스인 Cloud Front로 연결되어 S3오브젝트에 접근하게되고, 동적 데이터(API)는 아래와 같이 Load Balancer(ALB)를 통해서 EC2 내부로 접근하게 된다.

![OPGG_VPC.png](https://raw.githubusercontent.com/leeseojune53/yatudy/bfa3147e122d1f803d0e4711e86363e6209e0257/images/AWS%20Architecture.svg)

