---
layout: default
title: 리눅스 터미널에서 한글이 깨질 때(LANG)
nav_order: 2
parent: etc
---

LANG 환경변수를 설정해줘야한다.

```
LANG=ko_KR.UTF-8
```

이렇게 하면 로그도 한글로 나온다.

```
LANG=en_US.UTF-8
```

이렇게 하면 로그는 영어로 나오면서 한글을 출력할 때도 깨지지 않는다.

```
$ locale -a | grep en_US
en_US
en_US.iso88591
en_US.iso885915
en_US.utf8
```
