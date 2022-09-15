# Github Actions로 Organization Action 만들기

### 🎊 시작하기 전에...

이 글에서는 SvanBoxel이 만든 Actions를 사용한다.

[Project Github Link](https://github.com/SvanBoxel/organization-workflows)

또, 이 글에서는 Organization의 Repository에 PR이 올라왔을 때 Comment를 달아주는 Actions이다.

각 Repository에 Actions 코드를 넣게 되면, 변경사항이 발생하였을 때 모든 Repository의 코드를 수정해야하므로 중앙 관리형 Actions를 만들게 되었다.

### 1️⃣ 첫 번째로

위의 Github Actions App을 Organization에 설치해야한다.

[설치 링크](https://github.com/apps/organization-workflows)

Organization에 All repository 또는 실행시키고픈 Repository만 설정한다.

여기서 주의할 점은 **.github** Repository는 포함되어야한다. (Orgainzation에 없다면 생성하자.)

### 2️⃣ 두 번째로

앱을 설치했으니, 설정을 해야한다.

이 앱은 기본 설정 상 설정 정보를 .github에서 가져오기 때문에, 이 글에서는 .github Repository에 작업을 하겠다.

만약, 이미 다른 용도로 사용중이거나 다른 Repository를 사용하고 싶다면, [링크](https://github.com/SvanBoxel/organization-workflows/#app-configuration)를 확인하면 된다.

### 3️⃣ 세 번째로

```yaml
name: Pr Open Comment
on:
  repository_dispatch:
    types: [ org-workflow-bot ]
  pull_request:
    types: [ opened ]
    
jobs:
  register:
    runs-on: ubuntu-latest
    steps:
    - uses: SvanBoxel/organization-workflow@main
      with:
        id: ${{ github.event.client_payload.id }}
        callback_url: ${{ github.event.client_payload.callback_url }}
        sha: ${{ github.event.client_payload.sha }}
        run_id: ${{ github.run_id }}
        name: ${{ github.workflow }}
  comment:
    needs: [register]
    runs-on: ubuntu-latest
    steps:
      - name: Make Request
        id: myRequest
        uses: fjogeleit/http-request-action@v1
        with:
          url: "https://api.github.com/search/issues?q=sha:${{ github.event.client_payload.sha }}"
          method: 'GET'
      - name: Pn Rule Comment
        uses: actions/github-script@v5
        with:
          github-token: ${{secrets.TOKEN_GITHUB}}
          script: |
            github.rest.issues.createComment({
              issue_number: ${{ fromJson(steps.myRequest.outputs.response).items[0].number }},
              owner: "${{ github.event.client_payload.repository.owner }}",
              repo: "${{ github.event.client_payload.repository.name }}",
              body: 'P1: 꼭 반영해주세요 (Request changes)\n \
                     P2: 적극적으로 고려해주세요 (Request changes)\n \
                     P3: 웬만하면 반영해 주세요 (Comment)\n \
                     P4: 반영해도 좋고 넘어가도 좋습니다 (Approve)\n \
                     P5: 그냥 사소한 의견입니다 (Approve)\n \
                     질문은 그냥 코멘트로 😃'
            })
```

위 코드를 .github Repository의 .github/workflows 디렉토리 하위에 작성하면 된다.

간단하게 위 코드를 설명하자면, register job은 위 Actions App을 설정하는 job이다.

아래의 comment job은 Commit sha를 통해 PR Number를 가져오고, 해당 PR에 Comment를 다는 것이다.

github-token을 넣어주는 부분에 기본적으로 제공해주는 `GITHUB_TOKEN` 을 사용하지 않고, 따로 secret에 `TOKEN_GITHUB` 를 만들어서 사용한 이유는, 기본 토큰을 사용하면 코멘트를 생성하는 시점에 403 Forbidden 에러가 발생하기 때문이다.

## 마지막으로

이 글을 통해서 궁금한부분 또는 개선될 수 있는 부분이 보인다면 편하게 Issue를 달아주세요 😃