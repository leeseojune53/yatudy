# GitHub Flow란?

[TOC]

![GitHub Flow Model](http://cdn-ak.f.st-hatena.com/images/fotolife/s/shoma2da/20151104/20151104223339.png)

## GitHub Flow

### 특징

- release 브랜치가 명확하지 않은 시스템에서 사용에 맞게 되어있다.

- GitHub의 서비스 특성상. Release라는 개념이 없는 서비스를 진행하고 있어서 그렇다.

  웹 서비스들이 Release라는 개념이 없어지고 있어서 사용하기 편할 것.

- hotfix와 가장 작은 기능을 구분하지 않는다.

### main 브랜치

main 브랜치는 항상 최신의 상태이며, stable 상태로 Product에 배포되는 브랜치이다.

GitFlow와는 다르게 Feature 브랜치나 develop 브랜치가 존재하지 않는다. 따라서 새로운 기능을 추가하거나 버그를 해결하기 위한 브랜치의 이름은 자세하게 어떤 일을 하고 있는지에 대해서 작성한다.

### Push를 자주 한다.

항상 Remote Repository에 자신이 하고 있는 일들을 올려 다른 사람들도 확인할 수 있도록 한다.

### PR(Pull Request)

Feedback 또는 도움이 필요할 때, Merge(브랜치 -> main) 준비가 완료되었을 때 PR을 생성한다.

### Deploy

main으로 Merge되고 Push되었을 때는 즉시 배포되어야 한다.



### 장점

- 브랜치 전략이 단순하다.
- GitHub 사이트에서 제공하는 기능을 모두 사용하여 작업을 진행한다.
- 코드 리뷰를 자연스럽게 사용할 수 있다.
- CI가 필수적이고, 배포는 작동으로 진행할 수 있다.



### 단점

- CI와 배포 자동화가 되어있지 않은 시스템에서는 사람이 직접 해야한다.

