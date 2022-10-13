# innerHTML

## 1. 개요

`textContent` 프로퍼티, `innerText` 프로퍼티는 HTML 마크업이 파싱되지 않은 채 텍스트의 형태로 추가된다. 그렇다면 텍스트 노드에 HTML 마크업을 파싱하여 추가하는 방법은 무엇일까? 바로 `innerHTML` 프로퍼티를 사용하면 된다.

`innerHTML` 프로퍼티는 요소 노드의 HTML 마크업을 취득하거나 변경하는데 사용된다.

---

## 2. innerHTML 프로피터 반환 값

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">
      Hello
      <span>world</span>
    </div>
    <script>
      const $foo = document.getElementById("foo");
      console.log($foo.innerHTML);
    </script>
  </body>
</html>
```

위의 코드에서 콘솔 값으론 `"Hello <span>world</span>"`이 출력된다.

---

## 3. innerHTML 프로피터를 사용하여 자식 노드 수정하기

요소 노드의 `innerHTML` 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열에 포함되어 있는 HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">
      Hello
      <span>world</span>
    </div>
    <script>
      const $foo = document.getElementById("foo");
      $foo.innerHTML = "Hi <span>everyone</span>";
    </script>
  </body>
</html>
```

![innerHTML1](/image/JS/DOM/InnerHTML/innerHTML1.png)

`innerHTML` 프로퍼티를 사용하면 아래의 예시처럼 노드 추가, 교체, 삭제가 가능하다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="animal">
      <li>dog</li>
    </ul>
    <script>
      const $foo = document.getElementById("animal");
      // 노드 추가
      $foo.innerHTML += "<li>cat</li>";
      $foo.innerHTML += "<li>lion</li>";

      // 노드 교체
      $foo.innerHTML = "<li>tiger</li>";

      // 노드 삭제
      $foo.innerHTML = "";
    </script>
  </body>
</html>
```

---

## 4. innerHTML 프로퍼티의 단점

### 4-1. 취약한 크로스 사이트 스트립팅 공격

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">
      Hello
      <span>world</span>
    </div>
    <script>
      const $foo = document.getElementById("foo");
      $foo.innerHTML = `<img src="x" onerror="alert(document.cookie)">`;
    </script>
  </body>
</html>
```

크로스 사이트 스트립팅 공격을 해결하는 방법은 `HTML 새니티제이션`을 사용하는 것이다. 이는 사용자로부터 입력받은 데이터에 의해 발생할 수 있는 크로스 사이트 스크립팅 공격을 예방하기 위해 잠재적 위험을 제거하는 기능을 말한다.

대표적인 라이브러리는 `DOMPurify`가 있다.

---

### 4-2. 모든 자식 노드를 제거하고 DOM을 변경

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="animal">
      <li>dog</li>
    </ul>
    <script>
      const $foo = document.getElementById("animal");
      $foo.innerHTML += "<li>cat</li>";
    </script>
  </body>
</html>
```

위의 예시에서 `<li>dog</li>`는 변경이 없으므로 다시 생성할 필요가 없다. 하지만 `$foo.innerHTML += "<li>cat</li>";`으로 인해 `<li>dog</li>`는 삭제되었다가 다시 생성된다.

즉, `innerHTML` 프로퍼티에 HTML 마크업 문자열을 할당하면 유지되어도 좋은 기존의 자식 노드까지 모두 제거하고 다시 처음부터 새롭게 자식 노드를 생성하여 DOM에 반영한다. 이를 효율적이지 않다.

---

### 4-3. 불가능한 새로운 요소를 삽입할 때 삽입될 위치 지정

```html
<ul id="animal">
  <li>dog</li>
  <li>cat</li>
</ul>
```

`innerHTML` 프로퍼티를 사용하여 `dog`와 `cat` 사이에 새로운 요소를 삽입할 수 없다.

`innerHTML` 프로퍼티는 복잡하지 않은 요소를 새롭게 추가할 때 유용하지만 기존 요소를 제거하지 않으면 위치를 지정해 새로운 요소를 삽입해야 할 때는 사용하지 않는 것이 좋다.

---

## 5. Conclusion

> `innerText` 프로퍼티와 `innerHTML` 프로퍼티는 언뜻 보기엔 비슷한 역할을 한다고 생각할 수 있다. 그래서 예전에는 두 개를 거의 구분하지 않고 사용을 하였다. 하지만 엄연히 차이점이 존재하기 때문에 상황에 맞게 잘 사용을 해야 겠다. 또한 `innerHTML` 프로퍼티의 단점도 존재하기 때문에 단점이 들어나는 상황이 오면 다른 프로퍼티를 사용하여 요소 노드내에 자식을 추가하도록 하자.

---

## 참고

도서 - 모던 자바스크립트 Deep Dive: 자바스크립트의 기본 개념과 동작 원리

---

📅 2022-10-13
