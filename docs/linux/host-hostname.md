---
layout: default
title: 호스트의 IP 주소 얻기(host, hostname)
nav_order: 2
parent: Linux 명령어
---


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## host

> host is a simple utility for performing DNS lookups. It is normally used to convert names to IP addresses and vice versa.

```
root@HOST1:~:$ host HOST2.com
HOST2.com has address 10.xxx.xxx.xxx
```

원하는 호스트의 IP 주소를 알 수 있다.
자신의 호스트명을 넣으면 자신의 IP 주소를 알 수 있다.

## hostname

> Hostname is used to display the system's DNS name, and to display or set its hostname or NIS domain name.

hostname 명령어를 통해 자신의 IP를 알 수 있다.

> -i, --ip-address
>> Display the network address(es) of the host name. Note that this works only if the host name can be resolved. Avoid using this option; use hostname --all-ip-addresses instead.

`-i` 옵션을 넣는 경우 장비의 호스트명만 나온다.


```
root@HOST:~:$ hostname -i
10.xxx.xxx.xxx
```

`-I` 옵션을 넣는 경우는 해당 장비가 물고 있는 모든 IP 주소가 나온다.

> -I, --all-ip-addresses
>> Display all network addresses of the host. This option enumerates all configured addresses on all network interfaces. The loopback interface and IPv6 link-local addresses are omitted. Contrary to option  -i,  this  option  does  not depend on name resolution. Do not make any assumptions about the order of the output.

```
root@HOST:~:$ hostname -I
10.xxx.xxx.xxx 10.yyy.yyy.yyy 172.17.0.1
```

위 예시에서 `10.xxx.xxx.xxx` 는 장비 원래의 IP 주소이고 `10.yyy.yyy.yyy`는 L4 binding 된 VIP 주소이다.
`172.17.0.1` 는 docker0 interface 의 IP 주소이다.