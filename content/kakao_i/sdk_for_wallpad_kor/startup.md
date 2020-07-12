---
weight: 10
title: 시작하기
type: docs
bookHidden: false
tags: kakao_i
hackmd: 
---

# 시작하기

이 문서는 Wallpad 디바이스에서 Kakao i 음성 인식 서비스를 사용하기 위하여 Kakao i SDK for Wallpad를 이용한 개발 가이드로서, 고객사의 개발자를 대상으로 작성되었습니다. 

Kakao i SDK for Wallpad는 기본적으로 사용자의 음성 명령을 입력받고, 명령어를 분석하여 사용자 요청에 맞는 응답(음성 또는 UI)을 출력하는 기능을 제공합니다. 

**참고**
* 이 문서는  Kakao i Android SDK Version 1.2.4.18를 기반으로 작성되었으며, Android API 21을 필요로 합니다.

## Kakao i SDK 구성 요소

Kakao i SDK for Wallpad의 구성 요소는 다음과 같습니다.

표1. Kakao i SDK for Wallpad의 구성요소


| 구성요소 | 설명  |
| -------- | ------------------------------------------------------- |
| Kakao i Agent     | 음성 인식 및 인식 상태 전달 등 음성 인식의 주요 기능을 포함하는 모듈입니다.                                                                |
|    Kakao i Client      | Kakao i 서버와 SDK의 연동을 위해, 음성 인식 데이터와 음성 인식 상태 정보를 Kakao i 서버와 송신 및 수신할 때 사용되는 연동 관련 모듈입니다. |
|    AppClient      | Application의 통신 관련 설정을 할 수 있는 모듈입니다.                                                                                      |
|     Auditorium     | 사용자의 음성 명령을 녹음하고, 녹음된 결과(오디오 버퍼)를 필요한 곳에 공급하는 음성 입력 관련 모듈입니다.                                  |
|     Dialoid(Wake-up)     | Wake-Up 명령어를 설정하고, 인식된 발화 음성 상태 및 정보를 확인할 수 있는 음성 발화 모듈입니다.                                            |
|   Kapi Adapter       |Kakao i 음성 인식 서비스 인증 관련 모듈입니다.|
|    PhoneCall Manager      |전화 걸기 관련 모듈입니다. Wallpad에서 기능 구현 시, Kakao i PM에 문의 부탁드립니다.|


**주의**
* Kakao i 음성 인식 서비스는 Kakao 서비스와 구분되는 별도의 서비스입니다. Kakao i 음성 인식 서비스를 이용하기 위해서는 헤이카카오 서비스에 가입이 필요하며,  헤이카카오 서비스는 카카오 계정을 통해 가입할 수 있습니다. 

# 인터랙션 시나리오


다음은 사용자가 “오늘 날씨 알려줘"라는 음성 명령을 발화했을 때, 사용자, 벤더 애플리케이션, SDK 간의 인터랙션을 정리한 시나리오입니다.

1. Wallpad 디바이스에서 Kakao i SDK for Wallpad를 사용하기 위해 SDK 초기화를 수행합니다. 
2. 사용자가 Wake-up word(호출명령어)인 “헤이 카카오!”를 발화하면, SDK는 발화 명령어를 인식하고 발화 효과음을 재생하여 사용자에게 명령어를 입력 받을 수 있는 상태임을 알립니다.
3. 사용자가 “오늘 날씨 알려줘"라는 음성 명령을 하면, SDK는 사용자 음성 명령을 인식하고, 애플리케이션에 인식 상태를 실시간으로 전달합니다.
    - SDK는 명령어 인식을 완료하면 더 이상 사용자 음성을 입력받지 않고, 해당 음성 명령에 대한 응답을 출력합니다.

**참고**
* 응답 결과 출력 시 음성 답변과 함께 UI 템플릿 형식인 View Template이 사용될 수 있습니다. 
* 자세한 내용은 [음성인식 결과 화면 출력](https://www.notion.so/c400d4c95b4f46a6bad7bc7e118a8952#c6e319056a4d4d2d87c7566286db2c94)에서 확인하실 수 있습니다.

![](https://i.imgur.com/Wu8s2AM.png)
그림1. 인터랙션 시나리오