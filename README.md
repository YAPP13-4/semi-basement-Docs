# semi-basement-DevelopGuide
semi-basement의 개발 가이드  
본 문서는 MacOS 기준으로 작성되었습니다.  
## 개발환경 셋팅
권장하는 SW 목록은 아래와 같습니다.  
* Visual Stuio Code : Code Editor
* Source Tre : Git GUI Tool
* API Tester : PostMan 

### Visual Studio Code 
효율적 협업을 위해 아래 플러그인들을 설치합니다.  

* Prettier : 정해진 기준에 맞춰 코드를 자동 정렬해 줍니다. 
* GitLens : 코드 Line을 클릭하면 어떤 개발자가 어떤 커밋메세지와 함께 해당 코드를 작성 했는지 바로 확인할 수 있습니다. 
* Git History : 파일별로 Commit History를 확인할 수 있습니다. 

#### Prettier 설정 변경하기
`Preference -> Settings` 에서 설정을 변경합니다. 
`Search settings` 창에 Prettier 를 검색하고, `Edit in setting.json` 을 클릭합니다. 

```json
{
    "editor.formatOnSave": true
}
```

위 내용을 적어줍니다.  Javascript의 기본 `TabSize`는 2칸이며, 저장시에 자동으로 코드가 정렬되도록 합니다.  
이 외의 내용은 `.prettierrc` 의 셋팅을 적용합니다. 

### SourceTree 설정하기 
SourceTree를 다운 받고, 만약 계정이 연동되어 있지 않다면 계정을 먼저 연동시켜 줍니다.

#### 계정 연동 
`설정->계정` 탭에서 설정할 항목은 다음과 같습니다.  
* 호스트 : GitHub
* 인증방식 : OAuth
* 계정연결 : 본인 계정 연결
* 프로토콜 : HTTPS (권장)


#### Repository 연동
Repository WebPage 의 우측에 위치한 `Clone or download` 버튼을 클릭합니다.  
`파일->새로 만들기` 에서 `URL에서 복제` 버튼을 클릭합니다. 

* 원본 URL : 복사한 Git의 주소 
* 목적지 경로 : Git과 연동 할 나의 local Folder  
* 이름 : 사용자가 개별 설정하는 Project Name 

잠시 기다리면 연동이 완료되고 Source Tree 기본 셋팅은 여기까지 입니다.  

## 프로젝트 진행하기  
프로젝트에서 지켜야 할 규칙은 `GitFlow`, `기능단위 이슈 생성` , `Commit Message 양식` 입니다.  

#### 기능단위 이슈 생성
개발하고자 하는 기능을 GitHub에 이슈로 등록합니다.  

> WebPage -> Issue Tab에서 NewIssue를 클릭합니다. 

필요한 항목들은 다음과 같습니다.  

* Title : 기능 요약 
* Leave a Comment : 기능에 대해 MarkDown 형식으로 설명을 덧붙입니다.  
* Assignees : 담당자를 지정합니다.  
* Label : 기능 분류에 필요한 Tag 입니다. 해당 이슈의 정보를 빠르게 파악할 수 있습니다.  
* Projects : 해당 기능이 어떤 프로젝트에 필요한지 지정합니다. (Optional)
* MileStone : 기능별 구현을 명확한 기준을 두고 진행할 때 주로 사용합니다. (Optional)  

모두 작성되었다면 `Submit new issue` 버튼을 클릭하고 해당 **issue number** 를 확인합니다. 

> Title 옆에 `#` 다음 숫자가 issue number 입니다.  
 
#### GitFlow 
GitFlow의 정석적인 사용법에 대해 알고 싶다면 [우아한 형제들 GitFlow](http://woowabros.github.io/experience/2017/10/30/baemin-mobile-git-branch-strategy.html)을 클릭하세요.  

전체적인 흐름은 다음과 같습니다.  
`기능 단위 브랜치 -> develop -> master` 순으로 병합되도록 합니다. `master` branch는 배포시에만 사용하니 배포할 때가 아니면 Commit이나 Merge하지 않습니다.  

##### Branch CheckOut 
Web에서 issue를 생성하고 확인한 issue number 정보를 바탕으로 브랜치를 생성합니다.  
새로운 Branch는 `develop` 에서 checkOut 합니다.  
`SourceTree(프로젝트와 연동된 화면) -> 브랜치 -> 새 브랜치` 에서 브랜치를 생성할 수 있습니다. 

> 아래부터 작성되는 내용은 본 프로젝트에 최적화된 GitFlow 설명입니다.  

Issue로 생성한 기능이 Front 관련 기능일 아래와 같은 형식으로 브랜치를 생성합니다. 
```
front/issuenumber-feature
```

`issuenumber` 부분에는 이슈 생성시 확인한 issuenumber를 적고, feature에는 해당 기능을 한 단어로 요약해서 적습니다.  
새로운 기능 개발을 위한 `git check out`이 완료되었습니다. 

##### Pull Request 
기능 개발이 완료되었다면 해당 브랜치는 `develop` 브랜치로 Merge 되어야 합니다. 
Web ProjectPage에서 `새로운 Pull Request` 를 생성합니다.  
(제일 첫 화면에서 노란색 Tab의 버튼을 클릭하면 됩니다. )  
`develop < --- myBranch` 기능을 개발한 Branch에서 Develop 으로의 merge가 맞는지 확인합니다.  
  
Reviewer가 필요하다면, 오른쪽 Tab에서 지정하고 Commiter의 리뷰를 기다립니다. 
필요하지 않다면, Merge를 바로 승인하고 `delete Branch` 버튼을 눌러 해당 브랜치를 삭제합니다.  
  
SocureTree에서도 브랜치를 삭제해주어야 합니다.  
`브랜치->브랜치 삭제` 버튼을 눌러 `종류 : Local` 로 분류되어 있는 해당 브랜치를 삭제합니다.  


#### Commit Message 양식
Commit Message에 포함되어야 하는 내용은 다음 세 가지 입니다. 

* issueNumber
* Classification : 해당 작업이 단순 기능개발인지, 무언가 고치는 내용인지 등. 
* Description : 설명 

##### Commit Message 형식
```
[#issueNumber] Classification : description 
```

##### Commit Message 예시
```
[#1] feat : Music Player Prev 기능 구현 
[#2] fix : Prev 시 undef Error 수정 
```


## 참고
Thx To, [@nayunhwan](https://github.com/nayunhwan)
