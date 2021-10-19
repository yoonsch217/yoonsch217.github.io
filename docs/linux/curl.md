---
layout: default
title: curl 빌드하기(curl)
nav_order: 2
parent: Linux 명령어
---


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## curl

문제 상황: 기존의 curl이 Kerberos 인증을 못 받아가는 현상

해결 방향: 성공하는 환경에서는 curl --version으로 확인했을 때 버전과 feature이 달랐다. 정확한 원인 파악 이전에 새롭게 빌드한 curl로는 되는지 확인해본다.

참고: https://geekflare.com/curl-installation/
이 블로그에서 받아오는 사이트 주소만 바뀌었다.

```
yum install wget gcc openssl-devel -y
wget https://curl.se/download//curl-7.67.0.tar.gz
gunzip -c curl-7.67.0.tar.gz | tar xvf -
cd curl-7.67.0
./configure --with-gssapi
make
make install
./src/curl --version
```
