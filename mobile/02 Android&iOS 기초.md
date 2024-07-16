`Rooting` / `Jailbreak` : 단말기에서 최고 관리자 권한을 획득하는 과정

- 단말기에서 실행되는 앱 Process에 접근해 코드 흐름을 조작하기 위해
- 앱이 사용하는 정보들 열람, 수정하기 위해

<br>

## Rooting 🤖🪛

안드로이드에서 root를 획득하는 과정

Boot Loader Unlock → Custome Revoery → Custome ROM Install 

<br>

### 1. Boot Loader Unlock

부트로더 : 안드로이드 OS 부팅 초기에 실행. 커널이 올바르게 실행되기 위한 작업들을 수행 → 커널을 RAM에 로드 

**부트로더 Unlock** - 루트권한을 추가한 운영체제(커널)를 디바이스 RAM에 넣기 위해 

<br>

### 2. Recovery

안드로이드 OS 유지관리 기능 제공 (백업, 기기 초기화 등)

순정 recovery는 안드로이드에 필요한 최소한의 기능만 사용 가능함 (제한된 기능)

<br>

**Custom Recovery**

- 제한된 기능을 우회하기 위해 AOSP (Android Open Source Project)에서 몇 가지 기능을 추가해 만든 Recovery Image.
- fastboot mode를 이용해서 설치 → 안 되는 경우 Odin 사용

<br>

### 3. Custom ROM

안드로이드 기기 펌웨어 

<br>

**Custom ROM**

- 순정 Android ROM Image에서 setuid권한을 가진 `su binary`를 추가한 버전
- Custom Recovery를 이용해 Custom ROM (root권한을 이용할 수 있는) 설치

**⇒** 🌟 **안드로이드를 부팅할 때마다 root권한이 있는 수정된 운영체제를 로드** 

<br>

## Jailbreak 🍎

iOS에서 root권한 획득 ⇒ **특정 버전의 iOS 취약점이 발견되어야 함** 

임의로 **펌웨어 수정이 불가능**함 (소스코드 비공개)

최신 버전은 취약점이 발견되지 않아 탈옥이 불가능할 수 있음 

<br>

### Untethered Jailbreak - 완전 탈옥

한번 탈옥하면 평생 탈옥

<br>

### Tethered Jailbreak - 반 탈옥

재부팅하면 탈옥 풀림

지금 대부분의 탈옥은 반 탈옥 형식

<br>

### Semi-tethered Jailbreak - 준 반탈옥

재부팅하면 탈옥 풀림

특정 앱을 실행하면 바로 탈옥이 됨 

<br>

## **iOS 탈옥 방법** 🍎🪛

iOS 기종 및 버전마다 구체적인 방법들은 다름

🔗[3utools](https://www.3u.com/firmwares) 이용 → Phone이랑 연결해서 기기 버전 클릭하면 바로 탈옥시켜준다함 

탈옥을 끝내면 앱 2개가 설치 됨 (`iOS tweak`)

- Cydia
- Sileo

여러 해커 및 개발자들이 각자 자기가 만든 App의 레포지토리를 여기에 올려둠. → 레포지토리와 이름을 통해 설치 가능 

탈옥의 목적은 iOS tweak 때문이기도 함 (App store는 인증된 앱만 등록할 수 있기 때문)

<br>

## 안드로이드와 아이폰 Root Shell 접속 방법 💻

**Android :** ADB를 이용해서 `adb shell` 이라 입력하면 들어갈 수 있음 

<br>

**iOS** : 직접 쉘에 들어갈 수 없고 SSH를 사용해야 함

방법1. App Store에 등록X → 탈옥 후 Cydia,Sileo에서 `OpenSSH`설치

방법2. 3uTools에서 Toolbox → [OpenSSH]
