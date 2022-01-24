---
layout: default
title: YARN Memory Control
nav_order: 2
parent: hadoop
---

## Using Memory Control in YARN

### YARN에서의 메모리 관리

- 주기적으로 컨테이너 메모리 사용량을 측정하고 제한량을 넘으면 종료시킨다. 좀 늦으면 노드가 종료될 수 있다.
- Strict Memory Feature
  메모리 관리가 한계를 넘은 컨테이너를 종료시킨다. 리눅스 커널의 croups의 OOM killer 기능을 사용한다.
  oom이 발생하면 cgroups를 사용해서 컨테이너를 종료시킬 수 있다.
  컨테이너가 137로 종료됐다면 /var/log/messages에서 로그를 확인할 수 있다.
- Elastic Memory Feature
  cgroups 기반이다. 시스템 메모리가 한계에 다다르게 되면 컨테이너를 종료시킬 수 있게 한다.
  `yarn.nodemanager.linux-container-executor.cgroups.hierarchy`에 있는 모든 컨테이너들의 parent cgroup이 메모리 제한을 넘기게 되면 cgroups kernel을 통해 노드매니저에게 알려준다.
  전체 메모리가 제한을 넘지 않으면 종료시키지 않기 때문에 elastic memory control을 사용하게 되면 컨테이너가 받은 메모리보다 더 많은 메모리를 사용할 수가 있다.
  메모리 사용량이 제한에 닿게 되면 커널은 모든 컨테이너를 멈춘 후 노드매니저에게 알려주게 되고, 노드매니저는 컨테이너를 골라서 preempt시킨다.(preempt가 종료시킨다는 건지, 초과한 자원만 회수한다는 건지?)

### Preemption Logic

기본 로직은 DefaultOOMHandler이다. 가장 최근에 메모리 제한을 넘은 컨테이너를 고르는 것이다. 아무 컨테이너도 찾지 못한다면 가장 최근 실행된 컨테이너를 preempt 한다. 컨테이너가 제한을 넘지 않았다면 preempt되지 않는 걸 보장해준다. 컨테이너가 burst하면 preempt당할 수 있다.(burst?)
모든 컨테이너가 제한을 넘지 않았지만 메모리가 부족한 경우가 있을 수 있다. oversubscription을 하는 경우도 이런 상황이 발생할 수 있다. preempt되면 컨테이너의 데이터는 유실되기 때문에 가장 최근의 컨테이너를 preempt하는 게 손실을 최소화할 수 있다.
`yarn.nodemanager.elastic-memory-control.oom-handler` 을 통해 핸들러를 변경할 수 있다.

### Physical and virtual memory control

Elastic Memory Control을 사용하는 경우 `yarn.nodemanager.pmem-check-enabled`나 `yarn.nodemanager.vmem-check-enabled`가 설정되면 메모리 제한이 물리 메모리나 가상 메모리(rss+swap in cgroups)를 통해 적용된다.
둘 다 true로 하지는 않는다. swap을 사용하지 않으면 두 값은 똑같을 것이고, swap을 사용하면 가상 메모리가 물리 메모리와 디스크에 page를 차지할 것이다. 이런 경우 memory limit은 가상 메모리로 설정되어야 한다. swap을 사용하면 물리 메모리는 단순히 가상메모리에 불과하고 컨테이너가 아닌 커널에 의해 조정이 된다. swap을 사용하는데 어떤 컨테이너가 물리 메모리 제한을 넘는다고 preempt 시킬 필요가 없다. 시스템이 메모리를 swap out하면 되기 때문이다.

### Virtual memory measurement and swapping

container monitor가 측정하는 가상 메모리와 elastic memory control 기능이 정하는 가상 메모리 제한 사이에는 다른 좀이 있다.
container monitor는 `proc` 파일시스템의 값을 사용하는 `ProcfsBasedProcessTree`를 기본으로 사용한다. 반환되는 가상 메모리 값은 각 컨테이너에 있는 모든 프로세스의 주소 공간 크기인데, 여기에는 anonymous pages, pages swapped out to disk, mapped files, reserved pages among others가 포함된다. reserved pages는 물리 메모리나 swapped 메모리에 있는 게 아니고 가상 메모리 중 큰 부분을 차지할 수 있다. reservable 주소 공간은 32 bit 프로세서에서는 제한되었지만 64 bit 에서는 커져서 이 메트릭이 덜 유용해졌다. JVM의 경우도 많은 양의 page를 reserve 하지만 실제로 사용하지는 않는다. 보여지는 가상 메모리 사용량에서 수 기가바이트를 차지할 수가 있지만 그렇다고 컨테이너에 문제가 있다는 건 아니다.

`CGroupsResourceCalculator`를 사용하게 되면 reserved address space를 제외하고, 물리 메모리와 swapped page의 합만 보여주게 된다.
`yarn.nodemanager.resource-calculator.class`를 `org.apache.hadoop.yarn.server.nodemanager.containermanager.linux.resources.CGroupsResourceCalculator`로 설정해주면 사용할 수 있다.

## 메모리 측정

```
$ java -cp `hadoop classpath` org.apache.hadoop.yarn.util.ProcfsBasedProcessTree {PID}
```

```
$ docker stats {container_id}
```

---

References
https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/NodeManagerCGroupsMemory.html
https://wikidocs.net/27332
