---
layout: default
title: rpc
nav_order: 2
parent: network
---

### RPC message diagram

```
  Client                      Server
  ------                      ------

          "fn", x, y
  request ---------->

                          compute fn(x, y)

            z = fn(x, y)
           <------------- response
```

로컬에서 실행되는 것과 같이 원격 서버에서 함수나 프로시져를 실행시키는 것이다.

### Software Structure

```
   Client             Server
   ------             ------

  client app         handlers
    stubs           dispatcher
   RPC lib           RPC lib
     net <-----------> net
```

Stub은 실제 `f(x,y)` 함수인 것처럼 보이지만 실제로는 인자들을 패키징해서 네트워크로 보낸 후에 원격 서버에게 `f(x,y)`를 실행하도록 한다. stub은 결과를 네트워크로 받아서 클라이언트에게 return한다.

- Marshalling: format data into packets. channles, functions 등은 불가능하다.
- Binding: let client know the server to talk to -- e.g. DNS
- Threads:

### at least once

```
    while true
        send req
        wait up to 10 seconds for reply
        if reply arrives
            return reply
        else
            continue
```

RPC 클라이언트는 응답을 일정 시간 기다리고 응답이 없으면 다시 요청을 한다.
이 과정을 여러 번 하고 일정 횟수가 넘어가면 에러를 반환한다.

문제가 생길 수 있는 상황

```
Client                              Server
------                              ------

put k, 10
            ----\
                 \
put k, 20   --------------------->  k <- 20
                   \
                    ------------->  k <- 10

get k       --------------------->

                10
            <---------------------
```

서버에서 요청을 무시한 게 아니라 응답이 느린 경우가 있는데 이 때 위처럼 문제가 생길 수 있다.
read-only 작업이거나 서버 쪽에서 중복에 대한 처리를 한다면 괜찮을 수 있다.

### at most once

서버 RPC 코드에서 중복에 대한 검사를 하고 중복 요청이라면 핸들러를 다시 실행시키지 않고 이전 응답을 보내준다.
클라이언트는 요청마다 XID를 실어서 보낸다.

```
    if seen[xid]:
      r = old[xid]
    else
      r = handler()
      old[xid] = r
      seen[xid] = true
```

at-most-once를 사용했을 때 고려할 점으로는, 고유의 XID를 부여하는 방법과 오래된 RPC를 삭제하는 방법이다.
XID를 `클라이언트 IP + sequence number`의 형식으로 한다면, 고유한 XID를 부여할 수 있고 클라이언트가 응답을 받았을 때 몇 번까지 응답을 확인했는지를 서버에게 알려준다면 서버가 그 번호까지의 RPC를 삭제할 수 있다.

---

References

https://wizardforcel.gitbooks.io/distributed-systems-engineering-lecture-notes/content/l02-rpc.html
