---
weight: 20
title: Android Studio 설정
type: docs
bookHidden: false
tags: kakao_i
hackmd: 
---

# Android Studio 설정


## 프로젝트에서 SDK 사용



프로젝트에서 Kakao i SDK를 사용하기 위해서 다음 순서에 따라 Kakao i SDK를 빌드 의존성으로 추가한 후 불러오기를 진행합니다

## Gradle 설정



1. **Android Studio** | **New Project**로 이동하여 ****신규 프로젝트를 생성합니다
2. build.gradle로 이동하여 `minSdkVersion` 레벨을 21, `JavaVersion`을 8로  설정 후, 애플리케이션의 패키지 이름을 `applicationId`로 지정합니다.

    ```groovy=
    android {
            defaultConfig {
                ...
                minSdkVersion 21 //Android SDK API 레벨을 21로 설정
                applicationId "서버에 등록된 패키지명"
            }

            compileOptions {
                //Java 컴파일러 타겟을 Java8로 설정
                sourceCompatibility JavaVersion.VERSION_1_8
                targetCompatibility JavaVersion.VERSION_1_8
            }

            kotlinOptions {
    	    //Jvm Target을 1.8로 지정
                jvmTarget = "1.8"
            }
        }
    ```

    **참고**
    * SDK를 사용하기 위해서는 먼저 Kakao i 서버에 애플리케이션의 패키지명을 등록해야 합니다. 자세한 등록 방법은 Kakao i SDK 기획 PM에게 문의 부탁드립니다. 

3. `build.gradle (Project)` 파일의 `allprojects-repositories`  섹션의 `mavenCentral()` 아래에 각 레포지토리 URL을 추가합니다.

    ```groovy
    allprojects {
       repositories {
           google()
           jcenter()
           mavenCentral()
           maven { url 'https://devrepo.kakao.com/nexus/content/groups/public/' }
           //출시 시
           maven { url 'http://maven.daumcorp.com/content/repositories/daum' }
           //테스트 시
           maven { url 'http://maven.daumcorp.com/content/repositories/daum-snapshots' }
       }
    ```

    [표. Maven Repository 경로](https://www.notion.so/fc23817eb8054ac2966e6319fa0ffc86)

    **주의**
    * Release(출시)와 Snapshots(테스트)의 Maven Repository 경로가 상이하므로, 반드시 의존성 구분과 개발 단계를 확인한 후에 해당 경로를 추가해야 합니다.

4. `build.gradle (module: app)` 파일에 SDK 의존성 항목을 추가합니다. 

    [표. Kakao i SDK 의존성](https://www.notion.so/b6c0e399a9ed43aa840f6aeb7af79c05)

5. SDK 최신 버전을 컴파일하기 위해 `build.gradle (module: app)` 파일의 `dependencies {}` 섹션에 다음을 추가합니다.
- Kakao i SDK의 의존성 항목은 향후 추가 및 변경될 수 있으므로, 최신 정보는 Kakao i PM에게 문의 부탁드립니다.

    ```groovy
    dependencies {
           implementation 'com.kakao.sdk:usermgmt:1.27.0'
           implementation 'com.kakao.i:sdk-android:1.2.6.24'
        }
    ```

6. 프로젝트를 빌드하면, Kakao i SDK를 애플리케이션에 가져올 수 있습니다.

## Manifest 설정



사용자 권한을 획득하기 위해서 Android Manifest를 업데이트합니다. 

1. Project/.../`AndroidManifest.xml` 파일을 오픈합니다.
2. 다음 Manifest 권한 획득 표를 참고하여, 필요한 퍼미션을 업데이트합니다.

    [표. Mainfest 권한 획득](https://www.notion.so/06c435a50b8a484c95bde371c7292498)

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.READ_CONTACTS" />
<uses-permission android:name="android.permission.CALL_PHONE"/>
```

**참고**
* Manifest에 퍼미션 허용에 대한 상세 내용은 [Google Developers](https://developer.android.com/guide/topics/manifest/uses-permission-element) 사이트에서 확인하실 수 있습니다.