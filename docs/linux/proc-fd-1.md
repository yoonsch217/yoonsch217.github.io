---
layout: default
title: 프로세스의 표준 출력 확인하기(/proc/<pid>/fd/1)
nav_order: 2
parent: linux
---


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

원하는 프로세스의 stdout을 확인할 수 있다.

`ps -ef` 혹은 `pgrep` 명령어로 원하는 프로세스의 찾는다.

`/proc/<pid>/fd` 위치에 `0`, `1`, `2` 가 있는데 각각 stdin, stdout, stderr를 뜻한다.
fd는 file descriptor이다.

pid 123의 프로세스에 대한 stdout을 보기 위해서는 `tail -f /proc/123/fd/1` 를 할 수 있다.
