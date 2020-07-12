---
weight: 30
title: 음성 인식 개발 프로세스
type: docs
bookHidden: false
tags: kakao_i
hackmd: 
---

# 음성 인식 개발 프로세스

## 개발 프로세스

Android Studio 설정을 완료 후, 아래 순서에 따라 Kakao i SDK 연동 개발을 진행합니다. (수정1)


## **사전 작업**



음성 인식 개발 프로세스를 정상적으로 수행하기 위해서는 다음의 사전 작업이 필요합니다.

- Kakao SDK 연동
- 프로젝트에 Kakao SDK가 연동되어있지 않은 경우, Kakao Developers 사이트의 [Kakao SDK v2 사용법](https://developers.kakao.com/docs/latest/ko/getting-started/sdk-android)에 따라 연동 작업을 진행합니다.
- [Kakao Developers](https://developers.kakao.com/)에 애플리케이션 등록
-  [Kakao Developers](https://developers.kakao.com/)에 애플리케이션을 생성하는 방법은 Kakao i PM에게 문의 부탁드립니다.

## Kakao i SDK 초기화



Kakao i SDK를 초기화하기 위해서 하기 순서에 따라 `Application.onCreate()` 메서드에서  `KakaoI` 클래스에 내장된 메서드들을 체이닝 방식으로 확장합니다. 

1. 사전에 협의된 Phase 정보를 `KakaoI.with()`항목에 입력합니다.
    - 사용 가능 Phase는 `sandbox`, `stage`, `real`이며, Phase 정보를 확인해야 할 경우 Kakao i PM에 문의 부탁드립니다.

        ```kotlin
        KakaoI.with(applicationContext, "{사용 가능 Phase}")
        ```

2. Kakao 계정 인증 정보를 Kakao i SDK로 전달을 위해 `provideKakaoIAuth()`을 확장합니다. 
    - 모듈 확장 시, Kakao SDK에서 전달받은 인증 정보로 Access Token, Refresh Token, AppUser ID 정보를 제공하는 인터페이스가 구현되어야 합니다. 이 정보는 로그인 시점, 유저 확인 시점 등에서 Lazy 프로퍼티를 호출하며, 호출 시점에 값이 제공되지 않으면 오류가 발생합니다.

    **참고
    *** `AppUserId`의 경우 모바일앱의 appUserId로서, Kakao SDK에서 제공하는 Access Token Info에서 확인 가능합니다. 자세한 설명은 [Kakao Developers > 토큰관리](https://developers.kakao.com/docs/latest/ko/kakaologin/android#token-mgmt)를 참고 부탁드립니다.
    * Kakao SDK 초기화 및 개발 환경 설정은 [Kakao Developers](https://developers.kakao.com/docs/latest/ko/getting-started/env)의 구현 방식을 기반으로 합니다.

3. Kakao i 서버로  패키지 정보 전달을 위해 `providePackageInfo()`을 확장합니다. 
    - 확장 시에는 Package Name, Version Name, Version Code 정보를 포함하고 있는`packageInfo`를 구현해야 합니다.
    - Version Name과 Version Code 값을 별도로 지정하지 않을 경우에는 Default 값으로 전달됩니다.

    **참고**
    * Kakao i SDK를 사용하기 위해서는 Package Name을 Kakao i 서버에 미리 등록해야 합니다. 자세한 등록 방법은 Kakao i SDK PM에게 문의 부탁드립니다. 

4. 만약 Kakao i SDK에 기본 탑재된 View Template이 아닌 다른 View Template을 구현해서 사용할 경우,  `provideTemplateHandler()`을 확장합니다.
    - [의존성 추가](https://www.notion.so/Android-Studio-7db86e9315374e88ad422016af64bdd0#dacf2a5193dd4cd7b0bf0fd5023c979d) 작업 시, `com.kakao.i:sdk-android-agent`항목 추가가 필요합니다.

### **예제 코드**

다음은 `provideKakaoIAuth()`, `providePackageInfo()`, `provideTemplateHandler()` 메서드를 사용하여 Kakao i SDK를 초기화한 예시입니다.

```kotlin
override fun onCreate() {
   KakaoI.with(applicationContext, "sandbox")
       .module(object : Module() {
           override fun provideKakaoIAuth(): KakaoIAuth {
               return object : KakaoIAuth {
                   override fun getAccessToken(): String? =
                       Session.getCurrentSession().tokenInfo.accessToken

                   override fun getRefreshToken(): String? =
                       Session.getCurrentSession().tokenInfo.refreshToken

                   override fun getAppUserId(): Long =
                       getSharedPreferences("name", Context.MODE_PRIVATE)
                           .getLong(Constants.APP_USER_ID, 0L)

               }
           }

           override fun providePackageInfo(context: Context): PackageInfo {
               val packageInfo = super.providePackageInfo(context)
               packageInfo.packageName = getString(R.string.package_info)
               return packageInfo
           }

           override fun provideTemplateHandler(context: Context): TemplateManager =
                  TemplateManager(context, TemplateRenderer())

       })
       .init()
}
```

[ 표. Kakao i SDK 모듈](https://www.notion.so/7459a03038644037a18c2e88a330a1a6)

## 헤이카카오 가입

[https://www.notion.so/f16eb5dcc57f48979a8b620e1ef4aac0#87082a4afb4c496cbd454ce481d61c0b](https://www.notion.so/f16eb5dcc57f48979a8b620e1ef4aac0#87082a4afb4c496cbd454ce481d61c0b)



Kakao i 음성 인식 서비스를 이용하기 위해서 사용자는 [헤이카카오 서비스](https://kakao.ai/hey-kakao)에 가입해야 합니다. Kakao i SDK에는 자체적으로 헤이카카오 서비스 가입 프로세스가 구현되어 있습니다. 

- 헤이카카오 서비스는 Kakao 계정으로 가입할 수 있습니다.

1.  `KakaoI.checkAccount()`를 호출합니다.

    ```kotlin
    KakaoI.checkAccount()
    ```

2.  헤이카카오 계정을 확인 후, `checkAccount()` 콜백(Callback) 메서드로 현재 Kakao 계정 상태를 전달합니다. 
    - 각 메서드에 대한 설명을 참고하여, 필요 시 후속 진행 작업을 수행합니다.

[표. checkAccount 콜백](https://www.notion.so/65ef8251325a41ec83d99078e74f2f44)

### **예제 코드**

다음은 `checkAccount()` 콜백 메서드를 사용한 예제입니다.

```kotlin
//유저 상태를 확인하여 다음으로 수행해야할 Intent를 알려주거나 오류 상태를 전달 하는 Callback Interface
KakaoI.checkAccount(object : KakaoI.OnCheckCallback {
   override fun onSuccess() {
   /**
   * 사용자 상태 및 토큰 정보가 정상 으로 사용 가능한 상태
   */
   }

   override fun onAuthorizeFailed() {
   /**
   * AccessToken이 만료된 경우로, 인증 토큰 갱신 후 재시도 필요
   */
   }

   override fun onSignUpRequired(intentSupplier: KakaoI.IntentSupplier) {
   /**
   * 사용자가 Kakao i를 사용하고 있지 않은 상태로, Kakao i 가입이 요구되며,
   * 가입 Intent 가 전달되며 startActivity 로 Intent를 수행할 수 있음
   */
   }

   override fun onAgreementRequired(intentSupplier: KakaoI.IntentSupplier) {
   /**
   * 사용자가 추가로 동의해야할 약관이 존재할 경우 호출되며,
   * 약관동의 Intent 가 전달되며 startActivity 로 Intent를 수행할 수 있음
   */
   }

   override fun onError(p0: Exception?) {
   /**
   * 정의되지 않은 오류가 발생함
   */

   }
})
```

## Permission 확인



다음의 권한(Runtime Permission)을 Manifest에 추가하여, 사용자가 런타임에 각 권한을 승인하도록 요청합니다. 자세한 설명은 Google Developers 사이트의 [Android 개발자 > 앱 권한 요청](https://developer.android.com/training/permissions/requesting?hl=ko)에서 확인할 수 있습니다. 

[표. Runtime Permission](https://www.notion.so/152fcef17b2247648f70643ce74f6074)

## 음성 인식 활성화



Kakao i 음성 인식을 활성화하기 위해 False(Default)로 설정되어 있는 초기값을 `KakaoI.setEnabled()` 메드를 사용하여 True 값으로 변경합니다. 

```kotlin
KakaoI.setEnabled(true)
```

**참고**
* Wake-up word(호출명령어)의 초기 값은 “헤이카카오”로 고정되지만, 최종 사용자는 설정 화면에서 다른 Wake-up word를 설정할 수 있습니다. 
* Wake-up word가 유입될 때, 발화 효과음을 재생하는 과정은 Kakao i SDK 내부적으로 수행됩니다.

## 클래스 구현



음성 인식 결과 값을 애플리케이션으로 가져오기 위해서는 각 음성 인식 단계에서 클래스를 구현 및 정의해야 합니다. 각 클래스에 대한 상세 API는 본 가이드의 [API 레퍼런스](https://www.notion.so/API-d3f8f05ce721469b80180e291ed47526) ****챕터에서 확인하실 수 있습니다.

[표. 음성 요청 클래스 구현](https://www.notion.so/291e451b8c5f424ca814157eeed2fe62)

**참고**
* 음성 요청 클래스에 대한 상세 API는 본 가이드의 [API 레퍼런스](https://www.notion.so/API-f0b72e3171a04ab79f4279050ae7196e) 챕터에서 확인하실 수 있습니다. 

![Untitled%20d5b58263a22346eba1adc20f8a794463/Untitled.png](Untitled%20d5b58263a22346eba1adc20f8a794463/Untitled.png)

1. **초기화**
    - 애플리케이션에서 `KakaoI.startListen()` 메서드를 호출하고, `KakaoIListeningBinder`를 반환받습니다.
    - `KakaoI.startListen()`의 파라미터로 `KakaoIListeningBinder.EventListener` 콜백이 등록되면, 음성 인식 상태를 전달 받을 수 있습니다.
2. **음성 인식** 
    - 애플리케이션에서 `requestRecognition()` 메서드를 호출하여 음성 인식을 시작합니다.  음성 인식 상태에 따라 등록된 `EventListner` 콜백 함수가 호출됩니다.
3. **음성 인식 종료** 
    - `stopRecognition()` 메서드를 호출하여 음성 인식을 중간에 종료할 수 있습니다.

### 예제 코드

음성 인식 클래스 구현 시 필요한 샘플 코드는 다음과 같습니다.

```kotlin
class RecognizeActivity : Activity {
   private var listeningBinder: KakaoIListeningBinder? = null

   fun start() {
       listeningBinder = KakaoI.startListen(this@MainActivity, eventListener)
       listeningBinder!!.addListener(stateListener)
   }

   fun stop() {
       listeningBinder?.stopListen()
       listeningBinder = null
   }

   // Wake-up-word 없이 바로 Wake-up
   fun requestRecognition() {
       listeningBinder?.requestRecognition()
   }

   // 음성 인식 종료 (이미 녹음된 데이터가 존재할 경우, 해당 데이터로 음성 인식이 될 수도있음)
   fun stopRecognition() {
       listeningBinder?.stopRecognition()
   }

   // 음성이 입력중일 경우 음성 인식은 취소되며, 현재 진행중인 음성 인식 컨텍스트가 있다면 음성 인식은 종료됩니다.
   fun cancelDialog() {
       listeningBinder?.cancelDialog()
   }

   // KVS 연결 상황에서 발생할 수 잇는 오류들을 처리하는 callback
   private val eventListener = object : KakaoIListeningBinder.EventListener {
       override fun onMicUnavailable() {
           // Microphone 사용 불가 에러 상황
       }

       override fun onError(e: Exception?) {
           // 에러 상황
       }

       override fun onAgreementRequired(followingIntentFunc: KakaoI.IntentSupplier) {
           // 추가 약관 동의가 필요한 상황
           this@MainActivity.onAgreementRequired(followingIntentFunc.supply(this@MainActivity))
       }

       override fun onStartListen() {
           // 웨이크업 대기 상태 진입
       }

       override fun onStopListen() {
           // 웨이크업이 안되는 상태 진입, Microphone 사용하지 않음
       }

       override fun onWithdrawal() {
           // 서비스 탈퇴됨
       }

       override fun onAuthorizeFailed() {
           // 인증 실패
       }
   }

   // 음성 인식 상태를 모니터링 하는 callback
   private stateListener = object : KakaoI.StateListener {
       override fun onStateChanged(state: Int) {
           // int STATE_DEACTIVATED = 1;
           // int STATE_IDLE = 2;
           // int STATE_RECOGNIZING = 3;
           // int STATE_PROCESSING = 4;      
       }

       override fun onPartialResult(result: String?) {
           // 부분 인식 결과
       }

       override fun onResult(result: String?) {
           // 최종 인식 결과
       }
   }
}
```

## 음성 인식 결과 화면 출력



Kakao i SDK에서는 음성 인식 결과를 UI 화면으로 출력할 때, View Template이라는 레이아웃 화면을 사용합니다. 
Kakao i SDK는 기본적인 View Template을 제공하지만, 클라이언트의 특정 서비스에 맞춰서 View Template 수정도 가능합니다. 수정된 View Template 사용 시,  [Kakao i SDK 초기화](https://www.notion.so/0b662a19f26547fa8b521dbf745de0d7#908935e94c3f49d2a132d42ef63e71ca)를 수행할 때 `[provideTemplateHandler()](https://www.notion.so/9a14363c2a8b47099ba3e7c2b42afc4a?v=5d571b9986ea4f60b5e6845e2710b45d)` 메드를 호출해야합니다.  

**참고**
*  View Template 수정 시에도  음성 답변은 Kakao i SDK를 통해 재생됩니다.
 * View Template 구현에 관한 자세한 내용은 [Kakao i Engine] View Template 가이드를 참고하시기 바랍니다.

## 설정 화면 출력



`KakaoI.startSettingActivity(getContext()` 메서드를 호출하여 Kakao i 설정 화면을 최종 사용자에게 출력합니다. 

```kotlin
* KakaoI.startSettingActivity(getContext(), error -> /* Do something on error */)
```