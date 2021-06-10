# GitHub Actions



## 깃허브 액션(GitHub Actions)

GitHub Actions은 GitHub 저장소를 기반으로 소프트웨어 개발 워크플로우를 자동화 할 수 있는 도구이다. 간단하게 GitHub에서 직접 제공하는 CI/CD도구라고 할 수 있다.

워크플로우는 GitHub 저장소에서 발생하는 build, test, package, release, deploy 등 다양한 이벤트를 기반으로 직접 원하는 워크플로우를 만들 수 있다.

워크 플로우는 Runner라고 불리는 GitHub에서 호스팅 하는 Linux, macOs, Windows환경에서 실행된다. 그리고 이 Runners를 사용자가 직접 호스팅하는 환경에서 직접 구동시킬 수도 있다. 그것을 self-hosted runner라고 한다.

여러 사람이 공유한 워크플로우는 GitHub 마켓 플레이스에서 찾을 수 있다.



### 워크플로우 가격

워크플로우는 저장소마다 최대 20개까지 등록할 수 있다. 그리고 워크플로우 안에 존재하는 Job이라는 단계마다 최대 6시간 동안 실행될 수 있고, 초과하면 자동으로 중지된다.

그리고 GitHub 계정 플랜에 따라 전체 Git 저장소를 통틀어 동시 실행할 수 있는 Job 개수가 정해져 있다.

Job안에서 GitHub API를 호출한다면 1시간 동안 최대 1,000번까지만 가능하다.

공개 저장소에서는 무료이며, 비공개 저장소는 해당 계정에 부여된 무료 사용량 이후에 과금된다.



### 워크플로우 문법

워크플로우는 .yml파일로 작성이 된다.

```yaml
name: 워크플로우의 이름

on: push # 단일 이벤트 사용

on: [push, pull_request] # 이벤트 목록으로 사용

on:
	push:
		branches:
			- main
	pull_request:
		branches:
			- main
# 동작 유형 또는 구성과 함께 다중 이벤트 사용

on:
	release:
		types: [published, created, edited]
# types 키워드를 활용하면 워크플로우를 실행하는 활동의 범위를 좁힐 수 있다.
# ex) published, unpublished, created, edited, deleted or prereleased.

on:
	push:
		branches:
		  - main
		  - '적용할 브랜치'
		  - 'releases/**'
		tags:
			- v1
			- v1.*
# 적용할 브랜치를 설정할 수 있고, 태그에도 적용할 수 있다.

on:
	push:
		branches-ignore:
			- '제외할 브랜치'
			- 'releases/**-alpha'
		tags-ignore:
			- v1.*
# 제외할 브랜치를 설정할 수 있고, 태그에도 적용할 수 있다.

on:
	push:
		branches:
			- 'releases/**'
			- '!releases/**-alpha'
# 제외할 브랜치를 !로 설정할 수도 있다.

on:
	schedule:
		- cron: '30 5,17 * * *'
#시간으로 설정할 수도 있다.


```

위와 같이 GitHub Actions에서 이벤트를 설정하는 방법은 여러가지가 있다.