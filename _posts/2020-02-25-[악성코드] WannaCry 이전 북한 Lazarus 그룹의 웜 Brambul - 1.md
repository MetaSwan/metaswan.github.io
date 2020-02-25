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
---

파일 내 문자열으로 이메일, IP 주소, 레지스트리 변경, 공유폴더 접근, 관련 프로세스 등을 추측할 수 있습니다.




