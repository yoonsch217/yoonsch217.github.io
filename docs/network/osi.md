---
layout: default
title: OSI 7 layer
nav_order: 2
parent: network
---

A penguin said that nobody drinks pepsi.

L2 : OSI Layer 2에 속하는 MAC 어드레스를 참조하여 스위칭하는 장비
L3 : OSI Layer 3에 속하는 IP주소를 참조하여 스위칭하는 장비
L4 : OSI Layer 3 ~ 4에 속하는 IP주소 및 TCP/UDP 포트정보를 참조하여 스위칭 하는 장비
L7 : OSI Layer 3 ~ 7에 속하는 IP주소, TCP/UDP 포트정보 및 패킷 내용까지 참조하여 스위칭 하는 장비

### L4/L7의 용도

- 일반적으로 서버들의 로드밸런싱을 위해 사용.
- 복수개의 웹서버가 있을 때, 임의의 웹서버에 접속을 시도하면, 스위치가 각 서버의 부하를 고려하여 적당한 서버와 연결.
- 설정에 따라 순차적 연결, 접속이 가장 작은 서버에 연결하는 방식이 있음.

### L4 스위치

- 4계층에서 패킷을 확인하고 세션을 관리하며, 로드밸런싱을 제공하는 스위치
- TCP/UDP 패킷 정보를 분석해서 해당 패킷이 사용하는 서비스 종류 별로 처리(HTTP, FTP, SMTP...)
- 세션관리, 서버/방화벽 로드밸런싱, 네트워크 서비스 품질 보장

### L7 스위치

- L4스위치의 서비스 단위 로드밸런싱을 극복하기 위해 포트+데이터 페이로드 패턴을 이용한 패킷 스위치(e-mail 내용/제목, URL..)
- connection pooling(시스템 부하감소), Traffic Compression(컨텐츠 압축 전송), 보안기능
- URL 기반

### L2 스위치

- 가장 흔하게 볼 수 있는 스위치. 가격이 저렴.
- L2주소는 MAC주소.
- MAC 주소를 읽어 스위칭을 하고, 이것을 어떤 포트로 보낼 것인지 스위칭(컨트롤)하는 징비.
- 스위치는 MAC 테이블을 가지고 있어서 이것을 기준으로 패킷을 해당 포트로 전달.
- 라우팅 불가능, 상위 레이어 프로토콜을 이용한 스위칭 불가능.

### L3 스위치(라우팅)

- IP 또는 IPX 주소를 읽어서 스위칭
- 네트워크상에 흘러가는 패킷이 들어오면 L3 스위치는 목적이 IP주소를 보고 적절한 포트로 패킷을 전송(라우팅).
- A라는 IP는 일로가라 B라는 IP는 일로가라

---

ping은 포트하고 관련이 없다.
ping은 Internet layer의 ICMP(Internet Control message protocol) 에 요청을 보내는 것이어서 Transport Layer보다 낮은 layer에 있다.
포트는 Transport layer와 관련이 있다.

포트를 확인하기 위해서는 telnet을 사용하는 방법이 있다.
