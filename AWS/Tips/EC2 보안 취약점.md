# EC2 보안 취약점

라우팅 테이블에서는 VPC 내부, 외부로 나뉜다.

로드밸런서를 이용해서 igw가 연결안되어있는 VPC와 연결시켜준다.

외부 -> ELB -> VPC 내부

ELB에서 https만 열어주면 22번 포트같은 critical한 포트는 접근 X

3 teir = Presentation layer + Business layer + Data layer (클라이언트 + 어플리케이션 + 데이터)

2 teir = Application layer + Data layer (클라이언트  + 데이터)



로드 밸런서

리스너 : 듣는 port

다음으로 전달 (target group)



target group에는 ec2가 있다.

ELB에서 상태검사에서 설정 가능

unhealthy는 라운드로빈에서 제외한다

