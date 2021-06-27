# GitFlow란?

[TOC]

## GitFlow

Vincent Driessen이 말한 branching model을 구현한 Git 확장 모듈이다.

기본 브랜치는 5가지를 이야기한다.
feature > develop > release > hotfix > main 브랜치가 존재하며, 머지 순서는 앞에서 뒤로 진행된다.
release 브랜치와 hotfix 브랜치의 경우, develop 브랜치의 오른쪽에 존재하기에 모두 develop 브랜치도 머지를 하도록 구성이 되어있다.

Vincent Driesen은 관련하여 스크립트로 명령을 구성해놨으며, 그냥 설치를 하여 CLI에서 명령으로 작업을 하여도 되고, GUI 툴들에서 기본 내장 git-flow 명령이나 플러그인을 설치하여 작업을 진행할 수 있도록 보편화되어있는 브랜칭 모델이다.
![GitFlow 그림](http://nvie.com/img/git-model@2x.png)

## 구조와 흐름

가장 중심이 되는 브랜치는 main과 develop 브랜치이며, 이 두 개 브랜치는 무조건 있어야 한다. 이름은 바뀔 수 있지만, 웬만해서는 변경하지 않고 진행하도록 한다. Git도 Production에서 사용하는 브랜치는 main을 사용하게 되니 관련된 부분을 변경하면 새로운 사람이 왔을 때 혼란이 올 수 있다.

## Feature 브랜치

- 브랜치 나오는 곳 : develop
- 브런치가 들어가는 곳 : develop
- 이름 지정 : master, develop, release-*, hotfix-*를 제외한 어떤것이든 가능. 새로운 기능을 추가하는 브런치이다. feature브런치는 origin에는 반영하지 않고, 개발자의 repository에만 존재하도록 한다.

## Release 브랜치

- 브랜치 나오는 곳 : develop
- 브랜치가 들어가는 곳 : develop, main
- 이름 지정 : release-* 새로운 Production 릴리즈를 위한 브런치이다. 지금까지 한 기능을 묶어 develop 브랜치에서 release브랜치를 생성하고, develop 브랜치에서는 다음번 릴리즈에서 사용할 기능을 추가한다. release 브랜치에서는 버그 픽스에 대한 부분만 커밋하고, **릴리즈가 준비되었다고 생각하면** main으로 머지를 진행한다. main으로 머지 후 tag를 이용하여 릴리즈 버전에 대해 명시를 한다.

## Hotfix

- 브랜치 나오는 곳 : main
- 브랜치 들어가는 곳 : develop, master
- 이름 지정 : hotfix-* Production에서 발생한 버그들은 전부 hotfix브랜치로, 수정이 끝나면 develop, main브랜치에 반영하고, master에 tag를 추가해준다. 만약 release 브랜치가 존재한다면, release 브랜치에 hotfix 브랜치를 머지하여 릴리즈 될 때 반영이 될 수 있도록 한다.

## 장점

- 웬만한 에디터와 IDE에는 플러그인으로 존재한다.

## 단점

- 브랜치가 많아 복잡하다.
- 안 쓰는 브랜치가 있다. 몇몇 브랜치는 포지션이 애매하다.