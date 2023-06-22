# Github Actions로 PR에서 Comment로 배포하기

## 개요

회사에서 PR별로 환경을 구성해서 쉽게 테스트할 수 있게 Comment로 배포할 수 있는 기능을 만들게 되어서 해당 경험을 공유한다.

참고로, 회사 레포는 모노레포로 구성되어있어서 내부 디렉토리 기준으로 어떤 프로젝트가 배포되어야하는지 구분해야했다.

### 설계

Github Actions는 아래와 같은 순서로 구성하였다.

1. Trigger
2. Checkout by pr number
3. Directory modify check
4. Pre-Deploy Comment
5. Deploy
6. After-Deploy Comment

### Trigger

Trigger는 issue에 comment가 달리는 기준이고, workflow 실행 직후 pull request의 comment인지, `/deploy`(배포 명령어)가 포함된 comment인지 검사한다.

```yaml
on:
	issue_comment:
		types: [ created, edited ]

jobs:
	deploy:
		env:
			COMMENT: ''
		if: |
			contains(github.event.comment.html_url, '/pull/') && 
			contains(github.event.comment.body, '/deploy')
```

### Checkout by pr number

기존에는 commit hash 기준으로 checkout을 하려고 했는데, github cli에 `pr checkout`이라는 기능이 있어서 사용하였다.

```yaml
name: Checkout Pull Request Branch
env:
	GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
run: |
	gh pr checkout ${{ github.event.issue.number }}
```

### Directory modify check

모노레포로 구성되어있어서 각 디렉토리별 변경을 감지해야한다.

gh cli의 pr diff를 이용해서 변경된 파일을 불러오고, 해당하는 directory(아래 코드의 예시에서는 `root_dir/specific_repo/`)의 변경이 일어났다면 output으로 changed true를 준다.

```yaml
changed_files=$(gh pr diff ${{ github.event.number }} --name-only)
if echo "$changed_files" | grep ^root_dir/specific_repo/; then
	echo "There are changes in root_dir/specific_repo/ directory."
	echo "changed=true" >> $GITHUB_OUTPUT
else
	echo "There are no changes in root_dir/specific_repo/ directory."
	echo "changed=false" >> $GITHUB_OUTPUT
fi
```

### Pre-Deploy Comment

위에서 changed가 true라면 수행한다.

배포 시간이 약 3분정도 걸리기에 배포 전에 PR Comment를 달아준다.

해당 step을 id로 지정하는 이유는 After-Deploy Comment에서 새로 Comment를 다는 것이 아니라, 이번 step에서 생성하는 comment를 수정하기때문에 지정한다.

ps. Comment에 현재 진행상황을 업데이트하고싶지만, Comment에 linux의 progress bar같은걸 넣을 수 없어서 포기하였다.

```yaml
name: Add Pre-Deploy comment
if: |
	steps.changed.outputs.changed == 'true'
id: comment
uses: actions/github-script@v5
with:
	github-token: ${{secrets.GITHUB_TOKEN}}
  script: |
  	const response = await github.rest.issues.createComment({
  		issue_number: context.issue.number,
    	owner: context.repo.owner,
      repo: context.repo.repo,
      body: 'SpecificRepo Deploying... \n ![Deploy Progress](https://raw.githubusercontent.com/jaranda-arthur/storage/main/default.gif)'
      });
      return JSON.stringify(response.data.id);
```

### Deploy

Deploy step에서는 Build하고, 특정 URL로 배포하는 sh / gradle을 실행한다. 필자는 GCP AppEngine의 버전 배포 시스템을 활용해 진행하였다.

```yaml
- name: deploy
	if: |
		steps.changed.outputs.changed == 'true'
	run: |
		cd root_dir/specific_repo/
		chmod +x gradlew
		export VERSION=dev-pr-${{ github.event.issue.number }}
		[URL 배포하는 로직]

```

### Success Comment

성공 시 Comment를 Env에 저장한다.

```yaml
- name: Success Comment
	if: success() && steps.changed.outputs.changed == 'true'
  run: |
  echo "Deploy passed!"
 	echo "COMMENT=🚀 JApi : https://dev-pr-${{ github.event.issue.number }}-URL" >> $GITHUB_ENV
```

### Fail Comment

실페 시 Comment를 Env에 저장한다.

```yaml
- name: Fail Comment Set
	if: failure() && steps.changed.outputs.changed == 'true'
  run: |
  	echo "Deploy failed!"
  	echo "COMMENT=specific_repo Deploy failed! 💥" >> $GITHUB_ENV
```

### Deployed Comment

위의 Pre-Deploy Comment Step에서 생성한 Comment를 위에서 설정한 COMMENT Env로 내용을 수정하는 Step이다.

```yaml
- name: Add Deploy comment
	if: always() && env.COMMENT != ''
  uses: actions/github-script@v5
  with:
  	github-token: ${{secrets.GITHUB_TOKEN}}
    script: |
    	github.rest.issues.updateComment({
      	comment_id: ${{ steps.comment.outputs.result }},
      	owner: context.repo.owner,
      	repo: context.repo.repo,
      	body: '${{ env.COMMENT }}'
      })
```

