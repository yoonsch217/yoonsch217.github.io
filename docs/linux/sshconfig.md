---
layout: default
title: ssh config 설정
nav_order: 2
parent: Linux 명령어
---


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## ssh config 파일

문제 상황: ssh 접속할 때마다 인증을 해야하는 상황

```
$ cat .ssh/config
Host p
HostName targetHost
User myName
ControlMaster auto
ControlPath ~/.ssh/controlmasters/%r@%h:%p
ControlPersist 4h
```

control path는 직접 만들어줘야한다.
