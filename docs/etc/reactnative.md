---
layout: default
title: React Native
nav_order: 2
parent: etc
---

내부 이해

변경이 발생한 노드를 찾고 네이티브에서 랜더링한다.

쓰레드 통신을 위해 브릿지가 있다.
JS 영역과 Native 영역이 독립적으로 관리된다. 메모리는 공유한다.
JS 영역이 돌다가 Native 영역이 돌아도 비동기로 JS도 계속 돈다.
Native 호출이 많으면 부하 이슈가 있다.
이를 위해 큐에 넣어 5ms 마다 실행하는 걸로 부하 감소를 노력한다.
브릿지 호출을 최소화해야한다.

production levelㅇㅣ라면 Expo CRNA 를 사용하지 말자.
앱이 너무 커지고 추가적인 native 모듈을 설치할 수 없다.
빌드방식으로 시작을 하자.

버전 고정하는 방법:
1. 설치마다 고정 버전으로 설치하기
npm install --save --save-exact react-native-fbsdk
yarn add --exact react-native-fbsdk
2. 전역 기본 옵션으로 설정하기
npm config set save-exact=true

Flow 는 처음부터 사용한다.
파라미터 타입 오류를 감지할 수 있다.

터치 영역은 44dp 보장하자. 실제 보이는 버튼보다 더 크게 제공할 수 있다.

타이머/이벤트 리스너 사용시 꼭 제거한다. 
화면 전환 전에 타이머 멈췄다가 다시 돌아왔을 때 다시 타이머 실행시키는 꼼꼼한 작업이 필요하다.

flatlist를 사용해라 viewlist 말고

shrinkResources 옵션 사용하지 않기
사용하는 리소스까지 영향을 받는대

TinnPng 를 사용하면 이미지 최적화를 시킬 수 있다.


---

youtube 1강


