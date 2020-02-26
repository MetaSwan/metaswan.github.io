---
title: "[악성코드] WannaCry 이전 북한 Lazarus 그룹의 웜 Brambul - 2"
tags: [악성코드, 라자루스, 북한, 워너크라이, 웜, malware, wannacry, lazarus, worm, north korea, korean]
---


# 분석
---

[[악성코드] WannaCry 이전 북한 Lazarus 그룹의 웜 Brambul - 1](https://metaswan.github.io/posts/%EC%95%85%EC%84%B1%EC%BD%94%EB%93%9C-WannaCry-%EC%9D%B4%EC%A0%84-%EB%B6%81%ED%95%9C-Lazarus-%EA%B7%B8%EB%A3%B9%EC%9D%98-%EC%9B%9C-Brambul-1) 에서 이어집니다.

## 두번째 루틴
---

두번째 루틴이 시작되면 곧바로 sub_401ba0, sub_401b30, sub_401040 세 개의 서브루틴을 호출합니다.

![secondroutine](https://i.imgur.com/YHZW75Y.png)

### sub_401ba0 
lsasvc.exe 파일을 생성한 뒤 프로세스를 실행합니다.
이후 첫번째 루틴과 동일하게 admin으로 공유폴더에 접근합니다.

![401ba0_first](https://i.imgur.com/XqDL9Nk.png)

![401ba0_second](https://i.imgur.com/wuRTvDI.png)

---

### sub_401b30 
레지스트리 "Software\Microsoft\Windows\CurrentVersion\Run" 경로에 "WindowsUpdate" 라는 이름의 Value를 추가시킴으로써 해당 프로세스가 컴퓨터가 켜질 때마다 자동 실행되도록합니다.

![401b30](https://i.imgur.com/ps1yBs7.png)

---

### sub_401040 
프로그램 실행의 맨 처음에 수행했던 것과 유사하게 gethostname 함수를 통해 사용자의 이름을 가져옵니다.

![401040](https://i.imgur.com/AdreJ8I.png)

---

세개의 서브루틴이 실행 된 이후에는 GetVersion 함수로 운영체제의 버전을 가져옵니다.
"WinNt", "Win2000", "WinVista", "Win2003", "WinXp", "Unkonwn" 으로 분류하는 것을 확인할 수 있었습니다.

![getversion](https://i.imgur.com/tf9xlZV.png)

이후 whiat1001@gmail.com 문자열을 스택에 푸쉬하고 sub_401430 서브루틴을 호출하여 SMTP 프로토콜을 이용하여 데이터를 전송합니다. 발신하는 계정과 메일 서버는 whiat1001과 gmail.com으로 동일하지만, 발신자 계정을 johnS203@yahoo.com 으로 위조하여 발송한 뒤, 프로세스는 종료됩니다. sub_401430 에서 SMTP와 메일 헤더 관련 문자열들을 확인할 수 있었습니다.

![mailstrings](https://i.imgur.com/8i8Zlp5.png)

---

# 행위 결과

>1. 무작위 IP로 SMB 접근 시도 및 공유폴더, IPC, SCM 데이터베이스에 접근하여 자가 복제 및 배포
>2. whiat1001@gmail.com 을 johnS203@yahoo.com 으로 위장하여 SMTP 프로토콜을 이용하여 메일 발송
>3. admin 으로 공유폴더에 접근
>4. Windows Genuine Logon Manager (wglmgr) 서비스 생성
>5. Microsoft Windows Genuine Updater (wgudtr) 서비스 생성
>6. crss.exe 실행파일 생성
>7. lsasvc.exe 생성 및 실행
>8. 레지스트리 "Software\Microsoft\Windows\CurrentVersion\Run" 경로에 "WindowsUpdate" Value 추가

