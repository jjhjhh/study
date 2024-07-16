<img src="https://github.com/user-attachments/assets/2df2b59b-6b77-424c-85fb-3fc398833108" width=300>

Nox VT꺼짐 이슈 : Windows 기능 켜기/끄기 → [가상 머신 플랫폼] **해제**

위 작업을 수행하면 NoxPlayer가 정상적으로 실행 된다.

<br>

🔗[jadx](https://github.com/skylot/jadx) : apk 분석 프로그램 

<br>

## 앱 분석🪛

AndroidManifest.xml 에서는 앱 실행 시 가장 먼저 실행되는 액티비티를 확인할 수 있음 

<br>

**`android.intent.action.MAIN` → 가장 먼저 실행되는 액티비티**

위 속성을 갖고있는 com.insecureshop.ProductListActivity이 가장 먼저 실행됨을 알 수 있음. 해당 액티비티를 확인해보아야 함 

```java
<activity android:name="com.insecureshop.ProductListActivity">
            <intent-filter>
                <action android:name="**android.intent.action.MAIN**"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
```

<br>

라이프사이클을 보면 알 수 있듯이 **`onCreate()`** 함수 먼저 보아야 함.

해당 액티비티가 실행됐을 때 가장 먼저 실행되는 함수 

```java
public void onCreate(Bundle savedInstanceState) {
        ...
        if (TextUtils.isEmpty(prefs.getInstance(applicationContext).**getUsername()**)) {
            Intent intent = new Intent(this, (Class<?>) **LoginActivity**.class);
            **startActivity(intent);**
            finish();
            return;
        }
        ...
    }
```

사용자 이름이 없으면(isEmpty)  ⇒ 로그인이 되어있지 않으면 **`LoginActivity`** 를 실행하고 아니면 다음 동작 수행 

<br>

[LoginActivity]

```java
public final void onLogin(View view) {
        ...
        Log.d("userName", username);
        Log.d("password", **password**);
        **boolean** auth = Util.INSTANCE.**verifyUserNamePassword**(username, password);
        if (auth) {
            ...
            Intent intent = new Intent(this, (Class<?>) ProductListActivity.class);
            startActivity(intent);
            return;
        }
        ...
    }
```

로그인 결과를 boolean 형식의 auth변수에 담음 

<br>

[LoginActivity - verifyUserNamePassword 함수]

```java
public final boolean verifyUserNamePassword(String username, String password) {
        ...
        String passwordValue = **getUserCreds**().get(username);
        return StringsKt.equals$default(passwordValue, password, false, 2, null);
    }
```

<br>

[LoginActivity - **getUserCreds**]

```java
private final HashMap<String, String> getUserCreds() {
        HashMap userCreds = new HashMap();
        **userCreds.put("shopuser", "!ns3csh0p");**
        return userCreds;
    }
```

id와 pw를 확인할 수 있다. ⇒ 로그인 성공


<img src="https://github.com/user-attachments/assets/ca7ebd2a-4d1f-4233-8314-7a50b4d248dd" width=300>
