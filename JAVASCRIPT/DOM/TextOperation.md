# 요소 노드의 텍스트 조작

## 1. 개요

노드 객체의 값 즉, 텍스트 노드의 텍스트를 변경하기 위한 프로터치에 대해 살펴본다.

---

## 2. nodeValue

노드 객체의 `nodeValue` 프로퍼티를 참조하면 텍스트 노드의 값을 반환한다. 이는 문서 노드나 요소 노드의 `nodeValue` 프로퍼티를 참조하면 `null`를 반환하기 때문에 텍스트 노드에서 `nodeValue` 프로퍼티를 참조해야 한다.

아래의 예시를 보자.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">Hello</div>
    <script>
      const $foo = document.getElementById("foo");
      console.log($foo.nodeValue);
    </script>
  </body>
</html>
```

위의 예시에선 `$foo` 는 요소 노드이다. 그렇기 때문에 `$foo.nodeValue`는 `null`를 반환한다.

여기에 첫 번째 자식 노드를 가리키는 프로퍼티인 `firstChild`를 통해 텍스트 노드를 가져 올 수 있다. 즉, `$foo.firstChild`는 텍스트 노드가 된다.

다시 예시를 살펴보자.

```html
<html>
  <body>
    <!-- ... -->
    <script>
      const $foo = document.getElementById("foo");
      console.log($foo.firstChild);
    </script>
  </body>
</html>
```

`$foo.firstChild` 값은 `hello`이며 여기서에서 `nodeValue`를 통해 `$foo.firstChild` 값을 변경할 수 있다.

즉, 아래와 같이 사용할 수 있다.

```html
<html>
  <body>
    <!-- ... -->
    <script>
      const $foo = document.getElementById("foo");
      $foo.firstChild.nodeValue = "world";
    </script>
  </body>
</html>
```

`nodeValue` 프로퍼티의 사용 방법을 정리하자면 아래와 같다.

- 텍스트를 변경할 요소 노드를 취득한다.
- 취득한 요소 노드의 텍스트 노드르 탐색한다. 텍스트 노드는 요소 노드의 자식 노드이므로 `firstChild` 프로퍼티를 사용하여 탐색할 수 있다.
- 탐색한 텍스트 노드의 `nodeValue` 프로퍼티를 사용하여 텍스트 노드의 값을 변경한다.

---

## 3. textContent

`nodeValue` 프로퍼티는 텍스트 노드를 참조해야 하지만 `textContent` 요소 노드를 참조하여 텍스트를 조회, 변경을 할 수 있다. 즉, 요소 노드의 `textContent` 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역 내의 텍스트를 모두 반환한다. 이때, HTML 마크업은 무시된다.

아래의 예시를 살펴보자.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">Hello world</div>
    <div id="boo">Hello <span>world</span></div>
    <script>
      const $foo = document.getElementById("foo");
      const $boo = document.getElementById("boo");
      console.log($foo.textContent);
      console.log($boo.textContent);
    </script>
  </body>
</html>
```

위 코드에서의 콘솔의 값은 모두 `Hellow world`를 반환한다. 이렇게 `textContent` 프로퍼티는 HTML 마크업을 무시한다.

`nodeValue` 프로퍼티와 `textContent` 프로퍼티 중 어떤 것을 사용하는 것이 더 간단할까? 정답은 `textContent` 프로퍼티이다. `nodeValue` 프로퍼티는 텍스트 노드에서만 가능하기 때문에 요소 노드에 접근을 했다 해도 `firstChild` 프로퍼티로 텍스트 노드에 접근을 해야하기 때문이다.

`textContent` 프로퍼티를 사용하여 텍스트를 변경해보자. 요소 노드의 `textContent` 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가된다. 이때 할당한 문자열에 HTML 마크업이 포함되어 있더라도 문자열 그대로 인색되어 텍스트로 취급된다. 즉, HTML 마크업이 파싱되지 않는다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <!-- ... -->
    <script>
      const $foo = document.getElementById("foo");
      const $boo = document.getElementById("boo");
      $foo.textContent = "Hello world2";
      $boo.textContent = "Hello <span>world3<span>";
    </script>
  </body>
</html>
```

![textContent](/image/JS/DOM/TextOperation/textContent.png)

---

## 4. innerText

`innerText` 프로퍼터티는 `textContent` 프로퍼티와 유사한 동작을 한다. `HTMLElement`의 프로퍼티로 `tag`를 가지고 있는 노드에서 작동한다. 이는 `text Node`에서는 사용할 수 없다는 뜻이 된다.

`innerText` 프로퍼티는 브라우저에 반드시 렌더링 된 상태여야 값을 취득할 수 있다. 다시 말해 CSS로 인해 값이 렌더링이 되지 않는다면 값을 취득할 수 없게 된다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">Hello <span>world</span></div>
    <script>
      const $foo = document.getElementById("foo");
      console.log($foo.innerText);
    </script>
  </body>
</html>
```

위의 예시에서 `$foo.innerText`는 어떤 값을 가지게 될까? 바로 `Hello world`의 값을 가진다. 그렇다면 아래의 예시에선 어떨까?

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">Hello <span style="visibility: hidden">world</span></div>
    <script>
      const $foo = document.getElementById("foo");
      console.log($foo.innerText);
    </script>
  </body>
</html>
```

위의 예시에선 `$foo.innerText`는 `Hello`의 값을 가진다. 이렇게 `innerText` 프로퍼티는 CSS에 순종적으로 CSS에 의해 렌더링이 되지 않는다면 텍스트로 반환하지 않는다.

---

## 5. textContent과 innerText의 차이

1. `textContent` 프로퍼티는 `<script>, <style>`을 포함하여 모든 elements를 취급하지만, `innerText` 프로퍼티는 사람이 읽을 수 있는 elements만을 취급한다.

2. `textContent` 프로퍼티는 모든 elements를 반환하는 반면, `innerText` 프로퍼티는 styling상 인지될 수 있는(실질적으로 브라우저에 렌더링이 된) elements만 반환한다.
   - 이 때문에 `innerText` 프로퍼티는 `textContent` 프로퍼티보다 느리다.

---

## 6. Conclusion

> 요소 노드 내의 텍스트를 변경하기 위해 `innerText` 프로퍼티를 많이 사용하였다. 오늘 공부하기 전 까지는 `nodeValue` 프로퍼티와 `textContent` 프로퍼티는 모르고 있었다. `innerText` 프로퍼티와 `textContent` 프로퍼티는 비슷하지만 엄연한 차이가 있기 때문에 상황에 맞게 잘 선택하여 사용을 하자. 참고로 도서 - 모던 자바스크립트 Deep Dive에서는 `innerText` 프로퍼티보다는 `textContent` 프로퍼티를 추천하고 있다.

---

## 참고

도서 - 모던 자바스크립트 Deep Dive: 자바스크립트의 기본 개념과 동작 원리  
[innerText와 textContent의 차이 + innerHTML](https://velog.io/@tohero/innerText%EC%99%80-textContent%EC%9D%98-%EC%B0%A8%EC%9D%B4)

---

📅 2022-10-12
