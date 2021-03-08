---
layout: post
title: "[Git] SVN and Git"
date: 2021-03-08 14:00:00
categories: Git
permalink: /Git/svn-and-git
---

# Why?

지금까지 회사에서는 SVN을 사용하고 있다가 이번에 git으로 변경하게되었다. 그동안 사용했던 SVN과 이제 사용할 git이 어떤 차이점이 있는지 알고싶어 이 글을 작성하게 되었다.



## svn?

**중앙 집중 버전 관리 시스템**이다. 디렉토리를 Trunk / Branches / Tags로 구분하여 프로젝트에 영향을 최소화하며 변경할 수 있다.

- Trunk: 주요 개발 영역이다.

- Branches: Release 버전과 유지보수 버전을 분리하고 싶을 때 주로 사용한다. branch에서 수정하다가 trunk로 병합하는 것이 일반적이다.

- Tags: 현재 release 된 버전을 따로 보관하기 위해 사용한다. 개발을 위한 것이 아니라 '보관'만을 위한 것이기 때문에 export만 가능하다.

  

## git?

**분산 버전 관리 시스템**이다.  중앙 집중 버전 시스템은 commit == 중앙서버에 업로드라면, git에서는 최종 서버에 올리기까지 몇 가지의 단계를 더 거치게 된다. 그 과정에서 나온 개념중 하나는 로컬 저장소와 원격 저장소 개념이다.  

- 좀 더 깊이 있게 확인하고 싶다면 git 공식 페이지에서 확인해봐도 좋을것 같다.
- https://git-scm.com/book/ko/v2



### 차이점1. 저장소 위치

- svn은 보통 저장소가 서버에 있다.
- git은 로컬 PC에 로컬 저장소가 존재하며, 다른 사람들과 작업을 공유하는 원격 저장소가 존재한다.
- 로컬 저장소의 장점은 무엇일까?
  - 인터넷을 경우하지 않기 때문에 훨씬 빠르다.
  - 부담없이 커밋이 가능하다.
  - SVN은 서버에 문제가 생기면 모든 버전관리가 중단되지만 git은 원격 저장소와 연결이 끊어져도, 원격 저장소가 파괴되어도 로컬 저장소로 복원이 가능하다.
  - git에서는 로컬 저장소에 commit을 하기 전, commit할 파일을 선택해 올릴 수 있는 스테이지 영역이 존재한다. 

<img src = "/img/repo_1.JPG" class="middle-image"/>

<img src = "/img/repo_2.JPG" class="middle-image"/>

### 차이점2. 브랜치

- svn의 서버는 변경사항만 저장하며 최소한의 자료구조를 유지한다.
- svn에서 브랜치를 만들면, 전체 **파일**을 네트워크를 통해 통째로 **내려받기** 때문에 느리고 부담스러울 수 있다.
- git은 서버에 디렉토리 구조를 만들지 않고, 연속된 스냅샷을 저장한다. 
- 작업중에 branch와 master를 자유롭게 이동할 수 있다.

<img src = "/img/repo_4.JPG" style=" height: 300px;" class="middle-image"/>

svn은 변경되는 내용(ccc)만 저장을 한다면 git은 변경된 결과들에 대한 사진을 찍어서 저장하는 느낌으로 이해했다.



즉, git은 로컬 저장소와 원격 저장소가 분리되어 분산으로 처리가 가능하고 데이터가 안전하며 단계가 구분되어 있어 커밋 대상을 분리할 수 있다. 또한 파일을 내려받는것이 아니기때문에 빠르고 이전 커밋내용에 추가하거나 커밋의 순서를 변경할 수 있다는 장점도 존재한다.



그 외 이제 사용해야할 git의 간단한 명령어들과 브랜치를 확인해보려고 한다.

## Git 명령어

<img src = "/img/repo_3.JPG" style=" height: 200px;" class="middle-image"/>

- Rebase : Rebase는 merge와 동일하게 하나의 브랜치를 다른 브랜치로 병합하는 기능이다. 대신 아래와 같이 브랜치가 깔끔해 보이는 장점이 있다. 

<img src = "/img/repo_5.JPG" style=" height: 200px;" class="middle-image"/>



## Git flow

- Git flow에는 5가지 종류의 브랜치가 존재한다. 항상 유지되는 메인 브랜치들(master, develop)과 일정 기간 동안만 유지되는 보조 브랜치들(feature, release, hotfix)이 있다.
- master : 배포될 수 있는 브랜치 (svn의 trunk개념)
- develop : 다음 배포 버전을 개발하는 브랜치
- feature : 기능을 개발하는 브랜치
- release : 이번 배포 버전을 준비하는 브랜치
- hotfix : 출시 버전에서 발생한 버그를 수정하는 브랜치

<img src = "/img/repo_6.JPG" style=" height: 500px;" class="middle-image"/>



## 글을 마치며..

위 차이점을 알아보면서 개인적으로 생각해봤을 때 엄청난 장/단점이 있다!! 라기보다는 현재 어떠한 구조를 가지고 있고 몇 명이서 어떻게 운용하는지에 따라 알맞게 사용하는게 좋을 것이라 생각한다.  **(1) 브랜치나 태그를 많이 사용하지 않거나 (2) 1~2명이 규모가 작은 서비스를 운용하거나 (3) svn서버가 문제가 있었던 적이 없다**고 한다면 svn을 사용해도 문제가 없을것이라고 생각한다. 이제 git을 통해 협업을 해봐야 그 차이가 더 명확해 질 것이라 기대해본다.

[참조1 : https://d2.naver.com/helloworld/1859580]

[참조2 : https://blog.rhostem.com/posts/2017-01-07-git-basic]

[참조3 : https://deep-dive-dev.tistory.com/18]