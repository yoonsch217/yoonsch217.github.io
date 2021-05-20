---
layout: default
title: 네트워크 속도 측정(rsync)
nav_order: 2
parent: Linux 명령어
---


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## rsync

> Rsync is a fast and extraordinarily versatile file copying tool. It can copy locally, to/from another host over any remote shell, or to/from a remote rsync daemon.

파일과 디렉토리를 로컬 혹은 원격으로 복사할 수 있는 명령어이다.

```
USER@LOCAL:~:$ rsync -avh --progress ./test.files USER@REMOTE:~/
sending incremental file list
delta.files
       1.24G 100%  112.04MB/s    0:00:10 (xfer#1, to-check=0/1)

sent 1.24G bytes  received 31 bytes  107.42M bytes/sec
total size is 1.24G  speedup is 1.00
```

적당한 크기의 파일을 직접 rsync를 통해 remote로 전송함으로써 전송 속도를 볼 수 있다.