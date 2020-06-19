---
title: "방송 5 분 안에 시작하기"
date: 2020-06-18T13:36:21Z
draft: true
---

# 방송 5 분 안에 시작하기 :-D :-D

"5분 안에 시작하기"로 빨리 방송/통화를 체험해봅니다. 5분 안에 시작하기는 다음의 내용으로 이루어져있습니다.

1. **Service ID와 Secret Key 확인**
2. **샘플 앱으로 방송/시청 실시**
3. **웹 콘솔에서 이 방송에 대한 정보 조회**

## 준비 사항

* 웹 콘솔 회원 가입 : [https://console.remotemonster.com/](https://console.remotemonster.com/)
* GitHub 계정 또는 HTTPS가 지원되는 웹 호스팅 

## Service ID와 Secret Key 확인

리모트몬스터 콘솔을 방문하여 회원가입을 하고 "기본 프로젝트"를 선택합니다. Service ID와 Secret Key\(Service Key\)를 확인합니다. 이 Service ID와 Secret Key를 샘플 앱의 코드에 삽입할 겁니다.

[https://console.remotemonster.com/](https://console.remotemonster.com/)

주의 : Secret Key는 \*\*\*\*\*\*\*\*\*\*\*\*\*\* 이 아닙니다. 우측 눈 아이콘을 누르면 표시됩니다.

![](../.gitbook/assets/image-3%20%281%29.png)

## **샘플 앱으로 방송/시청 실시**

아래 GitHub 웹 사이트를 방문합니다.

{% embed url="https://github.com/RemoteMonster/remon-devguide-quickstartlivestreaming/" caption="" %}

아래와 같은 모습이 보입니다. simplelivestreaming.html, simplewatch.html 파일을 웹에 게시할 겁니다. 본 가이드는 GitHub Pages를 이용합니다. https가 지원되는 환경이라면 다른 곳에 파일을 게시해도 좋습니다.

![](../.gitbook/assets/image-2.png)

우측 상단의 Fork 버튼을 클릭합니다. 내 저장소가 생겼습니다. 내 저장소의 주소는 아래와 같습니다.

```text
https://github.com/내 아이디/remon-devguide-quickstartlivestreaming
```

저장소의 HTML 파일들을 웹에서 볼 수 있도록 설정합니다. "Settings" 탭으로 이동합니다. 페이지 아래에 "GitHub Pages" 섹션으로 이동하여, Source를 master branch로 선택합니다.

![](../.gitbook/assets/image-17.png)

![](../.gitbook/assets/image-8.png)

HTML 파일을 아래 주소에서 확인할 수 있습니다. 아래와 같은 화면이 표시됩니다.

```text
https://내 아이디.github.io/remon-devguide-quickstartlivestreaming/simplelivestreaming.html
```

```text
https://내 아이디.github.io/remon-devguide-quickstartlivestreaming/simplewatch.html
```

![](../.gitbook/assets/two-windows.png)

접속이 잘 되는 것을 확인했으면, 앱에 Service ID와 Secret Key를 입력합니다. 내 저장소로 이동하여 simplelivestreaming.html, simplewatch.html 파일을 클릭합니다. 아래와 같은 화면이 표시됩니다. 오른쪽 편집 아이콘\(연필 모양\)을 클릭합니.

![](../.gitbook/assets/image%20%283%29.png)

콘솔에서 확인한 Service ID와 Secret Key\(Service Key\)를 52번 행에 입력합니다. "Commit changes" 버튼을 눌러 저장합니다.

![](../.gitbook/assets/image-1.png)

웹 브라우저 창을 2개 엽니다. 아래 주소를 각각의 창에서 방문합니다.

```text
https://내 아이디.github.io/remon-devguide-quickstartlivestreaming/simplelivestreaming.html
```

```text
https://내 아이디.github.io/remon-devguide-quickstartlivestreaming/simplewatch.html
```

두 창에서 "시작" 버튼을 클릭합니다. Camera와 Microphone 사용을 **반드시** 허락\(Allow\)합니다.

## 웹 콘솔에서 이 방송에 대한 정보 조회

콘솔 창에서, "방송플랫폼 &gt; 채널 및 세션" 메뉴로 이동합니다. 방금 만든 방송에 대한 정보를 조회할 수 있습니다.

![](../.gitbook/assets/image-10.png)

**축하합니다. 방송플랫폼이 정상적을 동작하는 것을 확인하였습니다.**

튜토리얼의 새 프로젝트 설정, 단순 시청 앱 만들기 등을 참고하여, 방송/시청 앱을 원하는 대로 만들어보십시오.

