---
title: "Git 명령어 정리"
date: "2020-02-06T22:40:32.169Z"
template: "post"
draft: false
slug: "git-command-list-post"
category: "Git"
tags:
  - "Git"
  - "Wecode"
description: "Git의 주요 명령어를 정리해 보았다."
socialImage: "/media/workflow.jpg"
---

##Git 작업의 흐름

![workflow](/media/workflow.png)
Working directory는 실제 파일
Index는 staging area
HEAD는 최종 commit 확정본을 나타낸다.


### git init
**.git** 이라는 하위 디렉토리를 만든다. 새로운 git 저장소가 만들어지는 것이다.
**.git** 디렉토리에는 저장소에 필요한 뼈대 파일(Skeleton)이 들어있고 이 명령만으로는 프로젝트의 어떤 파일도 관리하지 않는다.
### git add <파일 이름>
```
git add .
```
git이 파일을 관리하게 만드려면 저장소에 파일을 추가하고 커밋해야 한다. **git add** 명령으로 변경된 파일을 인덱스에 추가한다.
git의 기본 작업 흐름에서 첫단계이다.
### git commit -m "이번 확정본에 대한 설명"
실제 변경 내용을 확정하기 위한 명령어다. 변경된 파일이 HEAD에 반영된다.
### git push origin master
로컬저장소의 HEAD 안에 머물고 있는 변경 내용을 원격 서버로 올린다. 다른 branch를 발행하려면 master를 원하는 branch 이름으로 바꿔 준다.
### git remote
현재 프로젝트에 등록된 리모트 저장소를 확인할 수 있다. 이 명령은 리모트 저장소의 단축 이름을 보여준다.
저장소를 Clone하면 **origin** 이라는 리모트 저장소가 자동으로 등록되기 때문에 'origin'이라는 이름을 볼 수 있다.
```
$ git remote
origin
```
**-v** 옵션을 주면 단축이름과 URL을 함께 볼 수 있다.
```
$ git remote -v
origin	https://github.com/DanSJKim/DanSJKim.github.io.git (fetch)
origin	https://github.com/DanSJKim/DanSJKim.github.io.git (push)
```

### git remote add <단축이름> <원격 서버 주소>
기존에 있던 원격 저장소를 복제한 것이 아니라면 원격 서버의 주소를 git에게 알려줘야 한다.
```
$ git remote add origin https://github.com/DanSJKim/DanSJKim.github.io.git
```

![branch](/media/branch.png)
### git branch <branchname>
현재 브랜치에서 새로운 브랜치를 생성한다.
```
$ git branch Feature-1
```
옵션을 지정하지 않고 branch 명령어를 실행하면 브랜치 목록 전체를 확인할 수 있다.
```
$ git branch
  Feature-1
* master
// 앞부분에 * 이 붙어있는 것이 선택 된 브랜치다.
```

### git checkout <branchname>
앞서 만든 **Feature-1**라는 브랜치를 사용하기 위해 이 브랜치를 사용하겠다고 명시적으로 지정해 준다.
**checkout**이란 내가 사용할 브랜치를 지정하는 것을 의미한다.
```
$ git checkout Feature-1
Switched to branch 'Feature-1'
```

### git clone
다른 프로젝트에 참여하거나 Git 저장소를 복사하고싶을 때 **git clone**을 사용한다.
```
$ git clone https://github.com/DanSJKim/DanSJKim.github.io.git
```
