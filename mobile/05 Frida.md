DBI Framework (Dynamic Binary Instrumentation) : 바이너리 동작을 동적으로 조사하는 일 

<br>

### 특징

- Client / Server 구조
- **JS**를 통한 바이너리 조작
- 멀티플랫폼 (Windows, Linux, iOS, Android)

<br>

## Frida Setting

> 💡
NOX사용을 위해 VT를 활성화하면 WSL2 와 충돌한다.
WSL1을 사용하자
> 

<br>

**CMD 관리자 권한으로 설치해야한다.**

`> pip install frida`

`> pip install frida-tools`

<br>

## NOX 설치 후 환경변수 설정

시스템 환경변수에 NOX 추가

<img src="https://github.com/user-attachments/assets/cbc2e7ac-1709-4687-bf3c-81cf05a5597c" width=300>

<br>

cmd에 아래 명령 실행되면 성공 

```bash
> nox_adb version
Android Debug Bridge version 1.0.36
Revision 0e9850346394-android
```

<br>

NOX를 켜고 컴퓨터 cmd에서 연결 

```bash
>nox_adb shell
dream2lteks:/ #
```

<br>

## NOX Setting 

설정 - 일반 - [ROOT 켜기] 활성화

<img src="https://github.com/user-attachments/assets/4bbc8ee6-992f-4705-a43d-a15e3e97fbdf" width=300>

<br><br>

설정 앱 - 시스템 - 태블릿 정보 - 빌드번호 계속 클릭 (개발자모드 됨) 

개발자 옵션 - 디버깅 - USB 디버깅 허용


<img src="https://github.com/user-attachments/assets/b05a11a8-733e-4c40-b1f5-041f06066d01" width=300>
<img src="https://github.com/user-attachments/assets/7e77fe87-825e-4d7f-a11a-cb30f7e6065d" width=300>

<br><br>

## Frida Server 설치 

🔗https://github.com/frida/frida/releases 에서 내 `NOX 아키텍처(x86_64)`와 `Frida 버전(16.4.5)`에 맞는 Frida-Server 를 설치한다

<img src="https://github.com/user-attachments/assets/930a6aa0-e2e4-4fde-8c55-e807cb4444dc" width=300>

<br>

**Show all … 을 눌러서 아래에 있는 frida-server-16.4.5-android-x86_64 를 설치해주도록 하자**

<br>

```bash
> tar -xJf frida-server-16.4.5-android-x86_64.xz
#🌟사용이 편하도록 파일 이름을 frida-server로 변경해주었다 

#NOX로 옮김
adb connect 127.0.0.1:62001

adb push frida-server /data/local/tmp/

adb shell "chmod 755 /data/local/tmp/frida-server"
```

<br>

## Frida server 연결 

```bash
adb shell

su root

cd /data/local/tmp

chmod 755 frida-server

mount -o rw,remount /system

cp frida-server /system/priv-app

cd /system/priv-app

**./frida-server** & #백그라운드 실행
```

<br>

연결된 디바이스들을 확인할 수 있다. SM-G955N는 삼성S8+ 이다 

```bash
> adb devices -l
List of devices attached
127.0.0.1:62001        device product:SM-G955N model:SM_G955N device:dream2lteks
```

<br>

연결 테스트 

```bash
>adb shell "ps | grep frida-server"
root          4144     1  129000  65788                     0 S frida-server
```

