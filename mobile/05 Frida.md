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

설정 - 일반 - [ROOT 켜기] 활성화

<img src="https://github.com/user-attachments/assets/4bbc8ee6-992f-4705-a43d-a15e3e97fbdf" width=300>


<br><br>

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

디바이스 아키텍처를 확인한다

```bash
dream2lteks:/ # getprop ro.product.cpu.abi
**x86_64**
```

<br>

그리고 🔗https://github.com/frida/frida/releases 에서 내 `NOX 아키텍처(x86_64)`와 `Frida 버전(16.4.5)`에 맞는 Frida-Server 를 설치한다.

```bash
>pip show frida
Name: frida
**Version: 16.4.5**
...
```

<br>

cmd에서 압축을 푼 파일을 녹스 앱플레이어 경로로 옮김 

```bash
> nox_adb push frida-core-devkit-16.4.5-android-x86_64 /data/local/tmp/frida-server

/data/local/tmp/frida-server/: 4 files pushed. 0 files skipped. 15.7 MB/s (276426788 bytes in 16.775s)
```

<br>

frida-server 설치 완료

<br>

`> frida-ps -U` 했을 때 결과가 뜨면 성공이다


<img src="https://github.com/user-attachments/assets/91ac3627-fbf6-4b0b-9c6e-aec4de04b3bf" width=300>
