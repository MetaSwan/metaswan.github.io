---
title: "[악성코드] WannaCry 이전 북한 Lazarus 그룹의 웜 Brambul - 1"
tags: [악성코드, 라자루스, 북한, 워너크라이, 웜, malware, wannacry, lazarus, worm, north korea, korean]
---

# WannaCry와 Brambul의 관계
---
WannaCry는 2017년 5월에 대유행한 북한 Lazarus 그룹의 랜섬웨어입니다.
이 랜섬웨어를 통해 공격자들은 약 15만 달러를 벌었고, 이 공격에 따른 피해는 약 수십억 달러로 추정되고 있습니다.
WannaCry의 특징 중 하나는 웜과 유사하게 자기 자신을 복제하여 접근가능한 네트워크에 배포하며 SMB 취약점과 이메일을 통해 주로 배포됩니다.

Lazarus 그룹은 WannaCry 이전에도 이와 유사한 방식으로 동작하는 웜을 배포하였었는데 이 중 하나가 Brambul 이라는 이름의 웜입니다.
WannaCry는 이 Brambul 이라는 웜의 랜섬웨어 형태의 변형이라고 볼 수 있죠.

![1](https://cdn5.alienvault.com/blog-content/Screen_Shot_2018-02-08_at_5.42.47_PM.png)

[Integer 사의 기술 보고서에서 발췌](http://www.intezer.com/wp-content/uploads/2017/07/Intezer-WannaCry.pdf)

Brambul은 2009년에 제작된 후 배포되기 시작하였으며 10년이 지난 지금, 다른 악성코드에 비해 위험하다고 볼 수는 없지만 여전히 취약한 버전과 설정의 컴퓨터에는 피해를 줄 수 있습니다.

# 분석
---
MD5 :  [f024ff4176f0036f97ebc95decfd1d5e](https://www.hybrid-analysis.com/sample/7b2f8c43b4c92fb2add9fce264e92668dac2530493c51c5d6b45dcb764e208ed/?environmentId=100)

파일을 실행하더라도 눈으로 보기에는 아무런 영향도 없어보입니다.
따로 패킹이나 난독화는 되어있지않아 분석하기에 쉬운 편입니다.
덕분에 파일 내 포함 된 문자열만으로 대략적으로 행위를 예측할 수 있습니다.

## 파일내 포함 된 문자열
---
![strings](https://i.imgur.com/xgpGboe.png)

위의 정보를 바탕으로 이메일, IP 주소, 레지스트리 변경, 공유폴더 접근, 관련 프로세스 등을 추측할 수 있습니다.


## 행위 시작
---
![mainfunc](https://i.imgur.com/nLWaVUt.png)

![findusername](https://i.imgur.com/cKclzgO.png)

프로그램은 시작하면서 WSAStartup 함수를 호출하여 네트워크 연결이 가능한지 체크한 뒤
사용자의 이름(PC의 이름)을 가져옵니다. 제 PC의 경우에는 "swan"이라는 값을 가져왔습니다.
그 후 "gmail.com" 문자열을 스택에 푸쉬하고 다음과 같이 dnsquery를 호출합니다.

![dnsquery](https://i.imgur.com/O0B3XUC.png)

이후 사용자의 이름이 System 인지를 검증하고 결과에 따라 행위가 두가지로 분기됩니다.

![checksystem](https://i.imgur.com/nu7Qqd2.png)

![branch](https://i.imgur.com/GhWLwo9.png)


## 첫번째 루틴
---

이 경우에는 sub_402900 서브루틴을 호출하고, 이 서브루틴은 먼저 몇 번의 GetTickCount 이후 무작위로 IP 주소를 생성합니다.
그리고 무작위로 생성된 IP로 445포트(SMB포트)로 접속을 시도합니다.

![iptosmb](https://i.imgur.com/bykvnRb.png)

![445port](https://i.imgur.com/UUWYUUZ.png)

![connections](https://i.imgur.com/5slnWx2.png)

SMB 포트로의 접속이 성공하면 IPC와 연결을 시도합니다.

![ipcconnect](https://i.imgur.com/Ymvu1X4.png)

IPC까지 연결이 성공하면 관리자계정으로 SCM 데이터베이스에 접근하고 악성행위를 수행합니다.

![scm1](https://i.imgur.com/VmvgAtB.png)

![scm2](https://i.imgur.com/jN1KzYh.png)

>1. 정해진 제목으로 whiat1001@gmail.com 으로 SMTP 프로토콜을 이용하여 메일 발송
>2. admin 으로 공유폴더에 접근
>3. Windows Genuine Logon Manager (wglmgr) 서비스 생성
>4. Microsoft Windows Genuine Updater (wgudtr) 서비스 생성
>5. crss.exe 실행파일 생성

이렇게 생성된 서비스와 실행파일 통해 자가 복제 및 전파를 하는 것으로 보이며 첫번째 루틴은 종료가 됩니다.

(2편에서 계속)
