## 원본 : [Do it! 쉽게배우는 웹앱&하이브리드 앱](https://www.inflearn.com/course/%EC%89%BD%EA%B2%8C%EB%B0%B0%EC%9A%B0%EB%8A%94-%EC%9B%B9%EC%95%B1%ED%95%98%EC%9D%B4%EB%B8%8C%EB%A6%AC%EB%93%9C-%EC%95%B1/)

### 폰갭(PhoneGap) - 코르도바(cordova)
  - Hybrid App ( 하이브리드앱 )
  https://phonegap.com
  https://play.google.com/store/apps/details?id=com.adobe.phonegap.app
  니토비 - 폰갭 - 어도비인수 - Apache Open Source

### Install
1. PhoneGap Desktop 애플리케이션 설치 :         http://docs.phonegap.com/getting-started/1-install-phonegap/desktop/
   - PhoneGap 애플리케이션을 생성하기위한 드래그 앤 드롭 인터페이스를 제공합니다.
   - [PhoneGap CLI](http://docs.phonegap.com/getting-started/1-install-phonegap/cli/) : 커맨드라인 방식의 대안으로 사용할 수 있음.

<br/>

2. Install PhoneGap Developer ( 폰갭 디벨로퍼 앱 )
    - 로컬에서 하이브리드앱을 IOS,안드로이드, 윈도폰의 모바일 기기의 멀티 플랫폼에서 폰갭 앱을 직접 테스트 지원 개발 환경.
    - [Google Play](https://play.google.com/store/apps/details?id=com.adobe.phonegap.app)
    - [Windows Phone Store](https://www.microsoft.com/en-us/p/phonegap-developer/9wzdncrdfsj0)
    - [~iTunes~ Currently not available but you can still build it yourself!](https://blog.phonegap.com/update-on-the-phonegap-developer-app-ios-99e07e3309dd)

    2.1. PhoneGap Developer 앱 실행.
    2.2. Once installed, move on to the next step where you will create your first PhoneGap app using the tool you selected in step 1.

    http://docs.phonegap.com/en/edge/guide_platforms_index.md.html#Platform%20Guides

##### ETC. PhoneGap CLI.
  0. node 설치 (https://nodejs.org/ko/download/)
  1. 폰갭 CLI를 설치하기위함)
  ```sh
    $ npm install -g phonegap@latest
    $ npm install -g cordova    <<-- 안해도 되는듯.
    $ phonegap
    Usage: phonegap [options] [commands]
    Description:
    PhoneGap command-line tool.
    Commands:
       help [command]       output usage information
       create <path>        create a phonegap project
        ...
  ```

#### Create a PhoneGap Project
```sh
C:\phoneGap>md project
C:\phoneGap>cd project
C:\phoneGap\project>phonegap create hello_world
```

#### Start Server
```sh
C:\phoneGap\project>cd hello_world
C:\phoneGap\project\hello_world>phonegap serve
[phonegap] starting app server...
[phonegap] listening on 192.1.0.247:3000
[phonegap]
[phonegap] ctrl-c to stop the server
[phonegap]
```
#### Test
  1. 폰갭 디벨로퍼 앱 - PhoneGap Developer 실행
  2. "192.1.0.247:3000" & "Connect" Click
  3. 수정 - /project/hello_world/wwww/index.html

#### 안드로이드 용 APK 배포파일 만들기 환경 준비
  1. [JAVA SDK](https://www.oracle.com/technetwork/java/javase/downloads/)
  2. [Apache ANT](https://ant.apache.org/bindownload.cgi)
  3. [안드로이드 SDK(ADT)](https://developer.android.com/studio/)
  4. 환경변수 설정 - Cordova 안드로이드 플랫폼 준비
    # 환경변수
     ANDROID_HOME C:\android\adt-bundle\SDK
     JAVA_HOME C:\Program Files\Java\jdk1.8.0_25

    # Path
     %JAVA_HOME%\bin;
     %ANDROID_HOME%\tools;
     %ANDROID_HOME%\platform-tools;
     %ANDROID_HOME%\build-tools;
     D:\android\apache-ant\apache-ant-1.10.5\bin;

  5. [zipalign](https://developer.android.com/studio/command-line/zipalign) - apk 압축 및 정렬준비

#### Add Android Platform
```sh
  C:\phoneGap\project\hello_world>cordova platform add android
```

#### Build Android
```sh
  C:\phoneGap\project\hello_world>cordova build android --release
```
## Signing & Alignment
#### 1. keytools
```sh
keytool -genkeypair -v -keystore xample.keystore -alias publishingdoc -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password:
Re-enter new password:
What is your first and last name?
  [Unknown]:  Ham Chimpanze
What is the name of your organizational unit?
  [Unknown]:  NASA
What is the name of your organization?
  [Unknown]:  NASA
What is the name of your City or Locality?
  [Unknown]:  Cape Canaveral
What is the name of your State or Province?
  [Unknown]:  Florida
What is the two-letter country code for this unit?
  [Unknown]:  US
Is CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US
Enter key password for <publishingdoc>
        (RETURN if same as keystore password):
Re-enter new password:
[Storing xample.keystore]

$ keytool -list -keystore xample.keystore
```

#### 2. jarsigner : apk 서명
```sh
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore xample.keystore mono.samples.helloworld.apk publishingdoc
```

#### 3. zipalign : 최적화
```sh
zipalign -f -v 4 mono.samples.helloworld-unsigned.apk helloworld.apk
```
