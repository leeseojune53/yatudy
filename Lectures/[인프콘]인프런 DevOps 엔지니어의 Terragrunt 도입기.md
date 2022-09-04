# 인프런 DevOps 엔지니어의 Terragrunt 도입기

1. 개요
2. 인프랩에 불어온 변화의 바람
3. Terraform UP & Running
4. DRY Terragrunt
5. Terragrunt 제대로 사용하기
6. 한 걸음 더 나아가기
7. 마무리



## 2 인프랩에 불어온 변화의 바람

작년(2021) 9월에 인프런 합류, 가장 큰 변화는 IaC이다.

IaC(Infrastructure as Code, 코드형 인프라)



### 도움되는 사람

- 다양한 간접 경험을 원하는 1~3년차 뎁옵스 엔지니어
- Terragrunt에 관심이 있거나 사례가 궁금한 사람 등

새로운 프로젝트인 랠릿의 인프라는 이미 성공한 케이스인 인프런을 따라함.

진행될 모든 프로젝트들이 비슷한 인프라 아키텍처를 가짐.

수동으로 진행하게 되면 단순 반복작업이 많아지므로 IaC 도입 결정.

## 3 Terraform UP & Running

AWS CDK로 IaC를 부분적으로 사용하고 있었음. 또한, FE, BE, DevOps 모두 TS를 사용함.

CDK를 랠릿 인프라에 적용하다 보니까 수정사항이 생겨서 수정을 하였는데, 오류가 발생함.

Terraform은 웹에서 변경한 사항을 찾아서 문제가 발생했다는 것을 알려주지만, CDK는 CloudFormation과 비교하기 때문에 실시간을 인프라의 상태가 반영되지 않음.

위와같은 단점을 파악하고 Terraform으로 사용하기로 결정함.

CDK : 파라미터로 아무 것도 안 적으면 알아서 최적의 값을 넣는다.

Terraform : 파라미터로 아무 것도 안 적으면 아무 것도 안 넣는다.



GCP의 Terraformer를 이용해서 Subnet만 변환 해보았는데 2273줄이 나온다.. ㅎㄷㄷ

## 4 DRY Terragrunt

DevOps가 4명으로 늘어나면서 일관성있게 구성하는 것을 알아야했다.



가장 먼저, 프로젝트 구조를 정립

1. env: dev, stage, prod 등 등
2. domain: inflearn, rallit 등
3. service: admin, b2b, b2c 등



다음으로 중복되는 코드가 많다는 사실을 인지함.

tfstate 저장소 겸 CI/CD, 괜찮지만 코드 중복과 상관 없음.

이후에 Terragrunt를 발견함.

| Terraform                                                    | Terragrunt                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| main.tf 파일에 동일한 내용이 중복됌.                         | include를 이용해서 해결할 수 있다.                           |
| 다른 디렉터리의 변수를 참조하려면 terraform_remote_state, json.yaml parse 등 복잡한 방법. | include, dependency 블록, read_terragrunt_config 함수를 사용해서 간단하게 사용 가능 |
| 다수의 디렉터리에 대한 명령을 실행하려면 의존성을 고려해서 각 디렉터리에 직접 가서 명령을 입력해야함. | 알아서 의존 경로 계산 및 병렬 처리 수행 (terragrunt run-all) |



하지만, 결과는 좋지 않았다, 시행착오가 너무 많았다.



## 5 Terragrunt 제대로 사용하기

Terragrunt의 고정된 값들의 중복을 제거하고 싶었다. 하지만 문제가 존재하지만, 구체적인 예제가 없었다.

환경변수로 분리하려고 했지만, 모든 환경변수를 넣게 되었다. 그러자 환경변수 파일이 엄청 커지고 복잡해졌다.

따라서 규칙을 정했다.

- 해당 환경 내에서 2회 이상 재사용하면서, 동일한 의미를 갖는 값만 분리



환경 변수말고 구조로 중복을 제거해야 했다.

import를 활용함

domain, service, module 이 세가지가 같으면 inputs 구조가 같다는 사실을 파악함.

inputs 구조

- 인자의 종류
- 어떤 dependency의 output을 어떤 인자에 전달하는가?
- 어떤 환경 변수를 어떤 인자에 전달하는가?



해당 값들이 바뀌면 어떡하나? locals로 분리하고, dict구조로 사용

```
locals{
	role_requires_mfas = {
		dev = false
		prod = true
	}
}

inputs = {
	...
	role_requires_mfa = local.role_requires_mfas[local.env]
}
```



Terragrunt 도입 후 config 구조까지 도달하고 나니 Terraform만 사용했을 때 보다 일회성 스크립트가 아닌 코드를 짠다는 느낌, 프로젝트 전반에 걸쳐 일관적인 컨벤션을 적용하기 수월해짐.



## 6 한 걸음 더 나아가기

Terragrunt 구조 정립 후 Atlantis 기반 CI/CD 구축.. 하려했지만 지원을 안함.

그래서 Atlantis를 공부하며 배운 것을 기반으로 Jenkins 기반 CI/CD 환경 구축

Apply 실패 시 자동 Rollback 등 기능 추가 및 경험 기반 안정성 개선 예정.

인프라 모니터링에 Datadog, Grafana, MongodbAtlas를 사용함.

Terragrunt로 AWS, CloudFlare를 사용하고있음.
