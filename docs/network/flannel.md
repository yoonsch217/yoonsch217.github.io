---
layout: default
title: Flannel Network
nav_order: 2
parent: network
---

## Flannel

https://gruuuuu.github.io/cloud/docker-rhel02/#

docker 설치하고 network interface를 보면 docker0이라는 virtual interface가 있다.
virtual ethernet bridge이다.
기본적으로 L2 통신 기반이며 container가 하나 생성될 때마다 container의 intercae가 하나씩 binding된다.
container가 외부로 통신할 때는 docker0을 지나야한다.

![image](https://media.oss.navercorp.com/user/17362/files/39461280-7527-11eb-9173-b04ceabc446c)

서로 다른 호스트끼리 하나의 컨테이너 클러스터를 구성하고 싶어 containerA와 containerB를 네트워크상으로 연결하는것이 가능할까요?
결론적으로는 불가능합니다. containerA에 할당된 ip는 123.0.1.1로 docker가 할당해준 ip입니다. 이는 도커호스트 내부에서밖에 사용할 수 없습니다.

즉 containerA에서 containerB로 ping을 보내려면, (Host1)docker0->eth0->(Host2)eth0->docker0->(ContainerB)eth0의 과정을 통해 전달되어야합니다. 이는 복잡한 포트포워딩이 필요합니다.
하지만, 런타임으로 인바운드-아웃바운드 포트가 랜덤하게 정해지는 애플리케이션에서는 포트포워딩으로는 해결할 수 없습니다.

![image](https://media.oss.navercorp.com/user/17362/files/52e75a00-7527-11eb-860d-9e72a9179fa8)

Flannel은 한마디로 원래의 패킷을 한번더 감싸는 역할을 하게 됩니다. 그림으로 보면 Node1의 Container1에서 Node2의 Container2로 보내는 패킷을 UDP로 감싸게 됩니다.
UDP의 헤더에는 출발지와 목적지의 정보가 들어있습니다.
이를 통해 컨테이너 간 통신을 포트포워딩 없이도 가능하게 합니다.

etcd가 필요하다.
https://github.com/etcd-io/etcd
분산 key-value store 시스템. 어떤 노드의 어떤 ID의 컨테이너가 어떤 IP할당받았는지 알 수 있다.

## 도커 네트워크

https://jonnung.dev/docker/2020/02/16/docker_network/

도커에서 사용할 수 있는 네트워크 종류는 브리지(bridge), 호스트(host), 논(none) 등이 있다.

## flannel

https://github.com/flannel-io/flannel

flanneld라는 작은 agent를 각 호스트에 실행시킨다.
쿠버네티스 API나 etcd를 사용해서 네트워크 설정을 저장한다.

쿠버네티스와 같은 플랫폼은 각 컨테이너가 클러스터 내에서 고유한 routable IP를 갖는다고 가정한다.
이렇게 함으로써 얻는 장점은 싱글 호스트 IP를 공유하면서 생기는 복잡한 포트 매핑을 할 필요가 없다는 점이다.

플란넬은 클러스터 내의 여러 노드들 사이에 layer3 ipv4 네트워크를 제공한다.
CNI 플러그인을 제공한다.

https://github.com/flannel-io/flannel/blob/master/Documentation/running.md
플란넬 실행시키기
