---
layout: default
title: git 명령어()
nav_order: 2
has_children: true
permalink: /docs/etc
---


# Search
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 기존에 있던 commit 에 내용 추가하기(rebase / squash)

코드 작업을 한 서버 A에서 아래의 절차를 한다.

1. `git rebase -i HEAD~2` => 최근 2 개의 커밋에 대해 작업한다. N개의 커밋에 대해 작업할 땐 `HEAD~N`
2. pick 으로 시작하는 vi가 뜨는데 첫 번째 꺼 빼고 squash 로 바꿔준다. 기존의 커밋에 내용을 합친다.
3. commit message 수정하는 vi 가 뜨고 알맞게 해준다.
4. `git push origin {branch name} -f`


서버 B에서 squash 전의 코드가 이미 pull 돼있다면 force merge 된 코드를 git pull 로 받아올 수 없다.

1. `git fetch --all` 저장소의 최신 상태를 fetch해오기만 하고 코드에 수정 작업은 일어나지 않는다.
2. `git reset --hard origin/{branch name}` 원하는 branch 의 최신 상태로 바꾼다. 기존의 수정사항이 다 날라간다.


## git 비밀번호 물어보지 않게 하기

1. `git config --global credential.helper 'cache --timeout=864000'`
default는 15분이다. 900초.
2. `git config credential.helper 'cache --timeout=864000'`
현재 디렉토리만 적용하려면 global 뺀다.
3. `git config credential.helper store`
credential을 디렉토리에 저장해서 캐싱 말고 그냥 반영구적으로 안 물어보게 할 수 있다.


## 특정 commit 의 코드로 복구하기

코드에 문제가 있을 때 stable 했던 상태의 commit으로 되돌릴 필요가 있다.
`git reset --hard 0ad5a7a6`