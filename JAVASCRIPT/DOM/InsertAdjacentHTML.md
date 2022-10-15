# insertAdjacentHTML

## 1. 개요

요소를 추가하기 위해 `innerHTML` 프로퍼티를 사용하면 기존 요소를 모두 제거한 후 새롭게 생성이 된다. 이는 비효율적이라 할 수 있다. 이를 해결할 수 있는 `insertAdjacentHTML` 메서드를 알아보자.

---

## 2. insertAdjacentHTML

`insertAdjacentHTML(position, DOMString)` 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다.

첫 번째 인수로 전달할 수 있는 문자열은 `beforebegin`, `afterbegin`, `beforeend`, `afterend`의 4가지이다.

![insertAdjacentHTML position](https://basicweb.ru/javascript/primer/insertadjacent.png)

---

### 2-1. insertAdjacentHTML - beforebegin

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">
      Hello
      <div>world</div>
    </div>
    <script>
      const $foo = document.getElementById("foo");
      $foo.insertAdjacentHTML("beforebegin", "<div> beforebegin insert</div>");
    </script>
  </body>
</html>
```

![insertAdjacentHTML - beforebegin](/image/JS/DOM/InsertAdjacentHTML/beforebegin.png)

---

### 2-2. insertAdjacentHTML - afterbegin

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">
      Hello
      <div>world</div>
    </div>
    <script>
      const $foo = document.getElementById("foo");
      $foo.insertAdjacentHTML("afterbegin", "<div> afterbegin insert</div>");
    </script>
  </body>
</html>
```

![insertAdjacentHTML - afterbegin](/image/JS/DOM/InsertAdjacentHTML/afterbegin.png)

---

### 2-3. insertAdjacentHTML - beforeend

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">
      Hello
      <div>world</div>
    </div>
    <script>
      const $foo = document.getElementById("foo");
      $foo.insertAdjacentHTML("beforeend", "<div> beforeend insert</div>");
    </script>
  </body>
</html>
```

![insertAdjacentHTML - beforeend](/image/JS/DOM/InsertAdjacentHTML/beforeend.png)

---

### 2-4. insertAdjacentHTML - afterend

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">
      Hello
      <div>world</div>
    </div>
    <script>
      const $foo = document.getElementById("foo");
      $foo.insertAdjacentHTML("afterend", "<div> afterend insert</div>");
    </script>
  </body>
</html>
```

![insertAdjacentHTML - afterend](/image/JS/DOM/InsertAdjacentHTML/afterend.png)

---

## 3. insertAdjacentHTML 장점과 단점

`insertAdjacentHTML` 메서드는 기존 요소에 영향을 주지 않고 새롭게 삽입될 요소만 파싱하여 자식 요소로 추가한다는 것이다. 이는 `innerHTML` 프로퍼티보다 효율적이고 빠르다.

하지만 `innerHTML` 프로퍼티와 마찬가지로 HTML 마크업 문자열을 파싱하므로 크로스 사이트 스크립팅 공격에 취약하다.

---

## 4. Conclusion

> `insertAdjacentHTML` 메서드는 아직 사용해 본 적이 없는 메서드이다. `innerHTML` 프로퍼티만 사용을 해보았다. 앞으로는 더 효율적인 `insertAdjacentHTML` 메서드를 사용해보도록 하자. 또한 위치까지 정할 수 있으니 활용할 수 있는 방안 더욱 많을 것이다.

---

## 참고

도서 - 모던 자바스크립트 Deep Dive: 자바스크립트의 기본 개념과 동작 원리

---

📅 2022-10-15
