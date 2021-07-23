---
layout: default
title: 언어마다 속도가 다른 이유
nav_order: 2
parent: etc
---

c++이 빠르고 python이 느리다는 얘기는 들었다.
지금까지 알고 있는 개념으로는 c/c++ > Java > JavaScript/python 이 순서인 것 같다.

c/c++ 의 경우는 컴파일이 되고 그러면 컴퓨터가 바로 이해할 수 있는 기계어로 된다.

Java 같은 경우는 컴파일이 되긴 되지만 컴퓨터가 읽는 언어라기 보다는 JVM이 읽는 바이트 코드로 컴파일이 돼서 c/c++ 보다는 조금 느리다.

JS, python 의 경우는 컴파일이 없고 한 줄 한 줄 interpret 해야한다.
그래서 REPL 이 가능한 것이기도 하다. REPL 은 터미널로 해당 언어를 실행시켜서 코드를 한 줄씩 실행시킬 수 있는 것과 비슷하다.
