---
layout: default
title: tcp & udp
nav_order: 2
parent: network
---

TCP 는 연결지향 프로토콜이고 UDP 는 연결없는 프로토콜이다.
TCP보다 UDP가 더 빠르다.
TCP 는 SYN, SYN-ACK, ACK 와 같은 handshaking 방식을 사용한다. UDP 는 handshake protocol을 사용하지 않는다.
TCP 는 에러 검증을 하고 복구를 한다. UDP 에러 검증은 하지만 에러가 있으면 버린다.
TCP has acknowledgment segments, but UDP does not have any acknowledgment segment.
TCP 는 무겁고, UDP는 가볍다.
