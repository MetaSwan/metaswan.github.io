---
title: "[악성코드] WannaCry 이전 북한 Lazarus 그룹의 웜 Brambul - 2"
tags: [악성코드, 라자루스, 북한, 워너크라이, 웜, malware, wannacry, lazarus, worm, north korea, korean]
---

#분석

[악성코드] WannaCry 이전 북한 Lazarus 그룹의 웜 Brambul - 1 에서 이어집니다.

## 두번째 루틴
---

두번째 루틴이 시작되면 곧바로 sub_401ba0, sub_401b30, sub_401040 세 개의 서브루틴을 호출합니다.

![secondroutine](https://i.imgur.com/YHZW75Y.png)

sub_401ba0 은 lsasvc.exe 파일을 생성한 뒤 프로세스를 실행합니다.
이후 첫번째 루틴과 동일하게 admin으로 공유폴더에 접근합니다.

![401ba0_first](https://i.imgur.com/XqDL9Nk.png)

![401ba0_second](https://i.imgur.com/wuRTvDI.png)

sub_401b30 은 "Software\Microsoft\Windows\CurrentVersion\Run" 경로에 "WindowsUpdate" 라는 이름의 Value를 추가시킴으로써 해당 프로세스가 컴퓨터가 켜질 때마다 자동 실행되도록합니다.

![401b30](https://i.imgur.com/ps1yBs7.png)

sub_401040 은 프로그램 실행의 맨 처음에 수행했던 것과 유사하게 gethostname 함수를 통해 사용자의 이름을 가져옵니다.

![401040](https://i.imgur.com/AdreJ8I.png)
