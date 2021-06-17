---
layout: default
title: PS1 설정으로 터미널에 색 넣기
nav_order: 2
parent: etc
---

```
PS1="\[\e[34;1m\]\u@\[\e[36;1m\]\h:\[\e[32;1m\]\w:> \[\e[0m\]"
```

를 terminal에서 입력한다.

매번 터미널 입력을 해야하는 상황이라면 `~/.bashrc` 에 입력해놓는다.

---

```
# source .bash_profile
```

> if: Expression Syntax.

원인

csh쉘에서는 실행이 안되는 것이라고함

```
# echo $SHELL
/bin/csh
#pstree
|-sshd---sshd---sshd---csh
```

해결

```
# exec bash
```

출처: https://sunspectra.tistory.com/entry/Source-명령-실행불가-if-Expression-Syntax
