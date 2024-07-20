루팅 탐지에 적용되어있는 App을 우회함

<img src="https://github.com/user-attachments/assets/f0e8ede0-f4d5-4359-9ef4-009e1ca2dfa2" width=300>
<img src="https://github.com/user-attachments/assets/247d4001-3c63-4e26-8baa-a61d6fa9b5b9" width=300>

실행되자마자 튕기는 상황

<br>

## 패키지 이름

androidManfest.xml 에서 패키지 이름을 확인할 수 있음 

```java
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionCode="1"
    android:versionName="1.0"
    package="**owasp.mstg.uncrackable1**">
```

<br>

## 후킹

```python
import frida, sys

Hook_package = "owasp.mstg.uncrackable1" #실행할 App

def on_message(message, data):
    print("{} -> {}".format(message, data))

#JS가 잘 삽입됐는지 확인
jscode = """
    Java.perform(function() {
        var exitClass = Java.use("java.lang.System");
        exitClass.exit.implementation = function() {
            console.log('[*] System.exit function hooking Success!');
        }
    });
"""

try:
    device = frida.get_usb_device(timeout=10)
    pid = device.spawn([Hook_package])
    print("App is starting.. pid:{}".format(pid))

    process = device.attach(pid)
    device.resume(pid)
    script = process.create_script(jscode)
    script.on('message', on_message)
    print("[-] Running FR1DA!")
    
    script.load()
    sys.stdin.read()

except Exception as e:
    print(e)
```

<br>

실행 시 앱이 켜지며 경고창을 눌러도 튕기지 않는다. (후킹 성공)

<img src="https://github.com/user-attachments/assets/3ccdd905-0d30-4c10-a3e0-0ed2407e09b3" width=600>

<br><br>

## App분석

가장 먼저 실행되는 액티비티 확인

```java
<activity
            android:label="@string/app_name"
            android:name="sg.vantagepoint.uncrackable1.MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
```

`sg.vantagepoint.uncrackable1.MainActivity`가 가장 먼저 실행됨 

<br>

MainActivity에서도 가장 먼저 실행되는 건 onCreate 함수

```java
protected void onCreate(Bundle bundle) {
        if (c.a() || c.b() || c.c()) {
            a("Root detected!");
        }
        if (b.a(getApplicationContext())) {
            a("App is debuggable!");
        }
        super.onCreate(bundle);
        setContentView(R.layout.activity_main);
    }
```

c.a() || c.b() || c.c() 이 참일 경우 Root detected 출력
