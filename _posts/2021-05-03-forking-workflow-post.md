---
layout: post
title:  "Forking Workflow"
date:   2021-05-03
excerpt: "협업하기 준비"
project: true
tag:
- github
- git
- project
comments: false
---


앞으로 협업하기 위해 선택한 깃허브 이용 방법

**Forking Workflow**

중앙 원격 저장소를 fork 해 개인 원격 저장소에 생성

원하는 로컬 디렉토리에서 git clone [개인 원격 저장소 주소]

개인 로컬 저장소와 중앙 원격 저장소 연결을 위한 git remote add upstream [중앙 원격 저장소 주소]

git status 로 파일 관리 상태 확인

git add 로 스테이지에 파일 올리기

git commit 으로 커밋

git commit -m “커밋 메시지” 를 하면 바로 커밋 가능

git pull upstream main 으로 중앙 원격 저장소 pull

git push 로 개인 원격 저장소로 push

깃허브로 가서 pull request

팀장은 검토 후 merge