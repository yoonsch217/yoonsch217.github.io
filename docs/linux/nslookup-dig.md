---
layout: default
title: DNS의 TCP, UDP 쓰레드가 살아있는지 체크(nslookup, dig)
nav_order: 2
parent: linux
---


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## nslookup

> Nslookup is a program to query Internet domain name servers.  Nslookup has two modes: interactive and non-interactive. Interactive mode allows the user to query name servers for information about various hosts and domains or to print a list of hosts in a domain. Non-interactive mode is used to print just the name and requested information for a host or domain.

기본적으로는 dns 레코드를 조회하기 위한 명령어이다.

```
$ nslookup <HOSTNAME> <DNS_SERVER>
```

의 형태로 사용한다.

```
user@MacBook-Pro-2:~:$ nslookup google.com
Server:		10.22.64.6
Address:	10.22.64.6#53

Non-authoritative answer:
Name:	google.com
Address: 172.217.26.46

user@MacBook-Pro-2:~:$ nslookup google.com 168.126.63.1
Server:		168.126.63.1
Address:	168.126.63.1#53

Non-authoritative answer:
Name:	google.com
Address: 172.217.175.14

user@MacBook-Pro-2:~:$ nslookup google.com 8.8.8.8
Server:		8.8.8.8
Address:	8.8.8.8#53

Non-authoritative answer:
Name:	google.com
Address: 172.217.175.110
```
서로 다른 DNS를 갖고 `google.com` 의 IP 주소를 얻는 동작이다.
위 결과로 얻은 서로 다른 IP는 모두 `google.com`의 IP 주소이다.


```
$ nslookup <IP_ADDRESS>
```
로 reverse lookup을 할 수도 있다.
찾으려는 서버가 속한 DNS의 설정에 PTR 항목이 설정되어 있어야 조회가 가능하다.



## dig

> dig is a flexible tool for interrogating DNS name servers. It performs DNS lookups and displays the answers that are returned from the name server(s) that were queried. Most DNS administrators use dig to troubleshoot DNS problems because of its flexibility, ease of use and clarity of output. Other lookup tools tend to have less functionality than dig.

`nslookup`과 마찬가지로 DNS 네임서버를 조회하는 명령어이다. DNS를 정해서 request를 날릴 수 있다.

```
user@MacBook-Pro-2:~:$ dig google.com +short
172.217.31.174

user@MacBook-Pro-2:~:$ dig @168.126.63.1 google.com +short
172.217.161.78

user@MacBook-Pro-2:~:$ dig @8.8.8.8 google.com +short
172.217.175.110
```

## DNS의 tcp, upd 쓰레드 체크

어떤 DNS의 tcp, udp 쓰레드가 살아있는지를 이 명령어들로 확인할 수 있다.
기본적으로는 upd 통신을 한다.
체크하고자 하는 DNS 주소를 `myDNS.com`이고 해당 DNS에서 조회 가능한 테스트 서버 주소가 `test-server.com` 이라면

- udp 쓰레드 살아있는지 체크
```
nslookup test-server.com myDNS.com
```
혹은
```
dig @myDNS.com test-server.com
```

- tcp 쓰레드 살아있는지 체크

vc 옵션을 붙이면 tcp 기반 통신을 한다.

```
nslookup -vc test-server.com myDNS.com
```
혹은
```
dig +vc @myDNS.com test-server.com
```

위 명령어들이 응답이 없으면 쓰레드에 이상이 있는 것으로 판단할 수 있다.