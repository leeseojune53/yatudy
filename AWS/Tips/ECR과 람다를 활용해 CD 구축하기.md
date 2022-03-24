# ECR과 람다를 활용해 CD 구축하기

### 🎊 시작하기 전에..

<img src="https://github.com/leeseojune53/yatudy/blob/main/images/Aws/aws-CD-pipeline.png?raw=true" alt="aws-CD-pipeline.png" style="zoom:50%;" />

*위 사진과 같은 구조입니다.*

우선 AWS에 대해서 VPC와 EC2등 기타 기능에 대해서 이해도가 있어야 읽기 편합니다. 그리고 이 글은 조금 불친절합니다. 고민이 생길 부분에 대한 **힌트**를 남겨놓았습니다. 일반적인 부분은 **구글링**을 해주시면 좋을 것 같습니다.

또한, Docker에 대한 기본적인 지식이 있어야 이해하기 편합니다.

마지막으로, 이 글에서는 Github Actions를 활용해서 Docker image를 Push할 것입니다.

**참고하기 좋은 글**

- [ECR private 저장소 pull 받는 법](https://github.com/leeseojune53/yatudy/blob/main/AWS/Tips/ECR%20private%20%EC%A0%80%EC%9E%A5%EC%86%8C%20pull%20%EB%B0%9B%EB%8A%94%20%EB%B2%95.md)
- [EC2 SSM 활용하기](https://github.com/leeseojune53/yatudy/blob/main/AWS/Tips/EC2%20SSM%20%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0.md)

### 👍 장점

다른 서비스에 비해 **유연한 확장**이 가능하다. 예를들어

Swarmpit(Auto Deploy) 기능은 새로운 이미지가 들어왔을 때(Update X) 새로 등록해줘야한다.

반면에 이 글에서 다룰 방법을 사용하면 Lambda에서 코드 수정을 통해서 입맛대로 고칠 수 있다. 이미지의 tag를 받아와서 해당 tag를 새로운 서비스로 생각하고, 배포하는 등 등

### 1️⃣ 가장 먼저

ECR(*Elastic Container Registry*)에서 저장소(Repository)를 생성한다. Private 저장소를 **권장**한다.

### 2️⃣ 두 번째로
EC2의 역할에 **AWSLambda_FullAccess, AmazoneEC2ReadOnlyAccess, AmazoneSSMFullAccess**를 넣어준다.

### 3️⃣ 세 번째로

*이 글에서는 Python 3.9를 기준으로 작성되었습니다. 또한 Docker Swarm을 사용하고 있다.*

Lambda로 이동한다.

Lambda의 IAM 역할에 **AWSLambda_FullAccess, AmazoneEC2ReadOnlyAccess, AmazoneSSMFullAccess**를 넣어준다.

이 단계의 핵심은 EventBridge에서 전달받을 **Event에서 필요한 데이터를 가져**오고, 그것을 통해 인스턴스에 **SSM**(*AWS Systems Manager*)을 활용해서 **Docker image를 업데이트** 시키는 것이다.

우선 boto3를 활용해서 원하는 인스턴스와 SSM의 client를 가져온다.

```python
ssm = boto3.client("ssm")
ec2 = boto3.client('ec2')
```

이후 SSM에서 send_command를 이용해서 EC2에 Command를 보낸다.

```python
ssm.send_command(
            InstanceIds=[instanceid],
            DocumentName="AWS-RunShellScript",
            Parameters={
                "commands": ["aws ecr get-login-password --region {region} | docker login --username AWS"
                "--password-stdin {userId}.dkr.ecr.{region}.amazonaws.com",
                f"docker pull {docker_image}",
                f"docker service update --image {docker_image} {container_name}"]
            },
    )
```



### 4️⃣ 네 번째로

위에서 저장소를 생성했다면 Amazone EventBridge로 이동한다.

여기서 핵심은 ECR에 이미지가 **Push**되었을 때 EventBridge가 대상에 **이벤트를 전달**하게 하는 것이다.

**대상**으로는 위에서 생성한 **람다를 지정**해준다.

