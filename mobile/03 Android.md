## 개발

JAVA 기반으로 개발됨

**JVM**

- JAVA 프로그램을 실행할 수 있는 가상머신
- JAVA를 프로그래밍하면 JAVA를 ByteCode로 바꿈 → JVM에 올려서 사용
- 모든 운영체제에서 사용될 수 있음

<br>

**Dalvik VM**

- JVM의 라이선스 문제 때문에 사용 
- ByteCode를 실행 전에 네이티브 코드로 바꿈 (JIT 컴파일 : 실행할 때마다 컴파일)
- 설치는 ART보다 빠름

<br>

**ART**

- OAT File 실행 (앱을 설치할 때 완전 네이티브 코드로 변환)
- 한번에 컴파일하기 때문에 Dalvik보다 속도 빠름

⇒ 최근에는 설치할 때는 Dalvik을 사용하다가 ART로 바뀌는 방식 사용 

<br>

## Android 언어

**DEX** : 안드로이드 실행 파일 

**Smali : Dalvik Code를 위한 어셈블리 언어.** 그대로 JAVA코드로 바꿀 수 있음

<br>

## 안드로이드 APK 구조

| 파일명 | 설명 |
| --- | --- |
| assets/ | 앱 실행에 필요한 자원(동영상같이 큰 파일)을 관리 |
| res/ | 앱 실행에 필요한 자원(아이콘같이 작은 파일) 관리 |
| MERA-INF/ | 인증 서명과 관련된 정보 |
| libs/ | 라이브러리 파일들 |
| AndroidManifest.xml | App 정보 (접근권한, 버전 등) |
| resource.arsc | res/ 정보 |
| class.dex | 달빅이 인식할 수 있도록 *.class 파일을 바이트코드로 변환시킨 소스파일  |

<br>

### **AndroidManifest.xml**

- **고유 패키지 이름(ID)** : 앱 후킹할 때 필요
- Activity 화면 : 어떤 화면들이 구성되어있는지
- 권한 정의
- 외부 라이브러리 정보

<br>

### **Activity**

사용자와 상호작용(클릭, 드래그)하는 UI 제공

앱의 화면

하나의 클래스 

<br>

**Activity 생명주기**

액티비티 시작 → `onCreate()` → `onStart()` → `onResum()` → (앱 띄워짐. 사용자가 앱 사용) → `onPause()`(다른 앱 띄워짐) → `onStop()` → `onRestart()` → onStart()

완전종료 → `onDestroy()`

<br>

### Broadcast Receiver

안드로이드 시스템에서 전송되는 방송 정보들 수신

ex) 충전기 꽂음. 문자 옴
