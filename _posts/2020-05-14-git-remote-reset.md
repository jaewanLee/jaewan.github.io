---
layout: post
title: 'Git 원격 Repo 상태로 Local 덮어쓰기'
author: jaewan
categories: [git]
image: assets/images/git_default.png
comments: false
---

# Git 원격 Repo 상태로 Local 덮어쓰기

작업을 진행하다 보면 가끔 로컬의 Git이 꼬여 귀찮은 경우가 많다.
만일 로컬에서의 모든 작업을 무시하고 remote의 코드로 복귀하기 원할 경우
reset을 사용하여 초기화 시킬 수 있다.

```
//원격 repo 정보 업데이트
git fetch --all

// 원격을 기준으로 강제 reset
git reset --hard origin/master

// pull
git pull origin master
```