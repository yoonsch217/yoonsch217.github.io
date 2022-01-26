---
layout: default
title: Linux와 Docker 컨테이너의 Out of Memory(OOM)
nav_order: 2
parent: Linux 명령어
---

## OOM

### Docker 메모리

https://docs.docker.com/config/containers/resource_constraints/

커널에서 시스템 function을 수행하기에 메모리가 부족하다고 판단하게 되면 OOME(out of memory exception)을 던지고 프로세스를 종료시키기 시작한다.
Docker나 다른 중요한 앱도 대상이 될 수 있기 때문에 잘못 종료되면 문제가 생긴다.

도커 데몬의 OOM priority를 조정함으로써 다른 프로세스에 비해 종료되지 않도록 한다.
container의 OOM priority는 조정이 안 된다. `--oom-score-adj`를 직접 매우 낮은 값으로 세팅하거나 `--oom-kill-disable`을 세팅하면 안 된다.

OOME로 인한 문제를 완화하기 위한 방법

- 실제 서비스를 하기 전에 앱이 어느 정도의 메모리가 필요한지 테스트를 해본다.
- 앱이 알맞은 리소스를 갖고 있는 호스트에서만 실행되는지 체크한다.
- 컨테이너가 가질 수 있는 메모리를 제한한다.
- swap을 신경쓴다. swap은 메모리에 비해 느리지만 메모리 부족한 상황을 일시적으로 보완할 수 있다.
- 컨테이너를 서비스로 전환하는 것을 고려한다. 서비스로 전환한 후 그 서비스가 실행될 수 있는 장비에서 실행을 한다.

### 도커 메모리 제한하기

hard limit을 두어서 주어진 양 이상의 메모리를 쓰지 못 하게 할 수 있고, soft limit을 두어서 호스트 장비의 메모리 부족과 같은 특정 상황이 되지 않는다면 메모리를 계속 사용할 수 있도록 할 수도 있다.
아래 설정값으로 보통 positive int를 받게 되고 `b`, `k`, `m`, `g`의 단위가 뒤에 붙는다.

- `-m` `--memory=`
  컨테이너가 사용할 수 있는 최대의 메모리이다. 6m 이상의 값을 주어야한다.
- `--memory-swap`
  컨테이너가 디스크로 swap할 수 있는 메모리의 양이다.
- `--memory-swappiness`
  호스트 커널이 컨테이너가 사용하는 anonymous page의 일정 부분을 swap out할 수 있는데 그 퍼센트를 설정한다.
- `--memory-reservation`
  호스트 장비에 메모리가 부족하거나 contention이 생길 때 활성화되는 soft limit으로 `--memroy`보다 작은 값을 가져야한다.
- `--kernel-memory`
  컨테이너가 사용할 수 있는 최대의 컨테이너 메모리 양이다. 4m 이상의 값이 주어져야한다. 커널 메모리는 swap out될 수 없다.
- `--oom-kill-disable`
  default로는 OOM 이 발생하게 되면 커널은 컨테이너 안의 프로세스를 종료시킬 수 있다. `-m` 옵션이 설정되어 있고 `--oom-kill-disable` 옵션도 사용하면 이걸 막을 수 있다. `-m` 을 설정하지 않은 채 `--oom-kill-disable`을 사용하면 커널은 호스트 시스템의 프로세스를 종료시킬 수도 있다.

`-m`와 `--kernel-memory` 두 가지의 메모리 제한이 있다. -일반 메모리 제한x, 커널 메모리 제한x: 기본값이다.

- 일반 메모리 제한x, 커널 메모리 제한o: 모든 cgroup이 필요한 메모리 크기가 호스트 장비의 메모리보다 클 때 사용될 수 있다. 커널 메모리가 호스트 장비에서 감당할 수 있는 정도를 넘지 않도록 설정할 수 있다.
- 일반 메모리 제한o, 커널 메모리 제한x: 전체 메모리는 제한되지만 커널 메모리는 특별한 제한이 없다.
- 일반 메모리 제한o, 커널 메모리 제한o: 메모리 관련 문제를 디버깅할 때 유용하다. 커널 메모리 제한이 유저 메모리 제한보다 작다면 커널 메모리가 부족해지면 컨테이너는 OOM을 겪게 된다. 커널 메모리 제한이 유저 메모리 제한보다 커지게 된다면 이 커널 제한으로는 컨테이너가 OOM을 겪지 않는다.

### Linux OOM Management

https://www.kernel.org/doc/gorman/html/understand/understand016.html

얼만큼의 page가 potentially available한지 알기 위해 다음의 값을 합한다.

- total page cache: 페이지 캐시는 쉽게 반환할 수 있다.
- total free pages: 이미 사용 가능한 상태이다.
- total free swap pages: userspace 페이지가 page out할 수 있다.
- total pages manged by `swapper_space`: free swap page를 두 번 계산하는 거지만.
- total pages used by the dentry cache: 쉽게 반환할 수 있다.
- total pages used by the inode cache: 쉽제 반환할 수 있다.

이 페이지들의 합이 요청을 처리하기 충분하면 vm_enough_memory()는 true를 반환한다.
false가 반환되면 caller는 메모리가 충분하지 않다는 것을 알게 되고 일반적으로 ENOMEM을 userspace로 반환한다.

장비가 메모리가 부족한 상황이 되면 오래된 페이지가 회수된다. 그럼에도 불구하고 메모리가 부족하다면 out_of_memory()가 호출되어 시스템이 oom인지 확인하고 프로세스를 종료시켜야할지 체크한다.

메모리가 부족해서 out_of_memory()가 호출되어도 실제 OOM 상황인지 여러 조건으로 한 번 더 체크한다.

badness는 다음 계산으로 결정된다.
`badness_for_task = total_vm_for_task / (sqrt(cpu_time_in_seconds) * sqrt(sqrt(cpu_time_in_minutes)))`
많은 메모리를 사용하면서 오래 실행되지 않은 프로세스를 찾는다. 만약 프로세스가 root 프로세스이거나 CAP_SYS_ADMIN 이 있다면 이 점수는 4로 나누어진다. CAP_SYS_RAWIO(access to raw device) 가 있다면 점수는 또 4로 나누어지게 된다.

이렇게 작업이 선택되면 SIGKILL을 보낸다.
