---
layout: default
title: 원하는 프로세스들 pid를 뽑아 작업 종료시키기(pgrep)
nav_order: 2
parent: Linux 명령어
---


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## pgrep

> pgrep looks through the currently running processes and lists the process IDs which matches the selection criteria to stdout. All the criteria have to match.


`my_process` 라는 문자열이 들어간 프로세스를 모두 찾아서 종료시키고 싶다.

해당 프로세스를 찾을 때 `ps -ef | grep 'my_process'`를 할 수도 있겠지만 pid만을 뽑고 싶을 땐 `pgrep -f my_process` 로 할 수 있다.

> -f
> The pattern is normally only matched against the process name. When -f is set, the full command line is used.

이렇게 뽑아낸 pid들의 작업을 종료시키기 위해서는
`pgrep -f my_process | xargs kill` 명령어로 할 수 있다.

