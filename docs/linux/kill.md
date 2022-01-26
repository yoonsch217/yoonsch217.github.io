---
layout: default
title: 원하는 프로세스 종료시키기(kill)
nav_order: 2
parent: Linux 명령어
---

sudo kill $(ps -ef | grep mycommand | awk '{print $2}')
