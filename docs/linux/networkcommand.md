---
layout: default
title: 네트워크 확인 명령어
nav_order: 2
parent: Linux 명령어
---

netstat -anp | grep $pid

이걸로 pid의 포트를 확인할 수 있다.

포트를 쓰고 있는 pid 찾기
netstat -nlp | grep :80

- nc target.com 80
  해당 서버의 80번 포트로 접속이 가능한지 알 수 있다.

- netstat -tnlpa | grep 53
  53번 포트 확인할 때 사용.

- grep -w 80 /etc/services
  80번 포트가 어떻게 쓰이는지 알 수 있다.

```
irteamsu@dev-yoonsch217-ncl:~/go:> grep -w 80 /etc/services
http            80/tcp          www www-http    # WorldWideWeb HTTP
http            80/udp          www www-http    # HyperText Transfer Protocol
http            80/sctp                         # HyperText Transfer Protocol
```

1024 미만의 포트는 아파치와 같이 잘 알려진 서비스에 사용된다.

- Well Known Ports: those from 0 through 1023.
- Registered Ports: those from 1024 through 49151
- Dynamic and/or Private Ports: those from 49152 through 65535

대표적인 포트들

- 21: FTP Server
- 22: SSH Server (remote login)
- 25: SMTP (mail server)
- 53: Domain Name System (Bind 9 server)
- 80: World Wide Web (HTTPD server)
- 110: POP3 mail server
- 143: IMAP mail server
- 443: HTTP over Transport Layer Security/Secure Sockets Layer (HTTPDS server)
- 445: microsoft-ds, Server Message Block over TCP

- netstat -tulpn
  열려있는 포트, 소켓 확인하기

lsof, sockstat

- netstat -nat | grep LISTEN
  listening socket(open port) 확인하기

- netstat -tulpn | grep :80
  80 port 확인

- /sbin/iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
- service iptables save
  포트가 열려있어도 firewall에 의해 거부될 수 있다.
  firewall level에서 tcp 포트 80번을 연다.

- nc -l 5000
  포트5000을 연다

- nc unix.system.ip.here 5000
  특정아이피의 포트로 연결한다.

- nc -l 5555 > output.txt
  다른 컴퓨터로 데이터를 보낸다.
