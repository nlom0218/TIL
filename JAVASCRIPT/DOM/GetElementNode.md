# 요소 노드 취득하기

## 1. 개요

HTML의 구조나 내용을 동적으로 조작하기 위해서는 우선적으로 요소 노드를 취득해야 한다. DOM은 요소 노드를 취득할 수 있는 다양한 메서드를 제공한다.

---

## 2. id를 이용한 요소 노드 취득 - Document.prototype.getElementById()

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul>
      <li id="apple">apple</li>
      <li id="banana">banana</li>
      <li id="orange">orange</li>
    </ul>
    <script>
      const $apple = document.getElementById("apple");
      $apple.style.color = "red";
      console.log($apple);
    </script>
  </body>
</html>
```

id 값은 HTML 문서 내에서 유일한 값이어야 한다. 하지만 만약 중복된 id가 여러 개 존재하더라도 에러는 발생하지 않지만 `getElementById` 메서드는 인수로 전달된 id 값을 갖는 첫 번째 요소 노드만 반환한다. 즉, `getElementById` 메서드는 언제나 단 하나의 요소 노드를 반환한다.

만약 인수로 전달된 id 값을 갖는 HTML 요소가 존재하지 않는다면 `getElementById` 메서드는 `null`를 반환한다.

`getElementById` 메서드는 id 값과 동일한 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당된다. 위의 예시에서 id 값이 `apple`인 노드 요소를 `$apple` 변수에 할당하였다. 하지만 이와 동시에 id 값인 `apple`도 암묵적으로 선언되고 `$apple`와 같은 노드 객체가 할당된다. 즉 아래의 값은 `true`가 된다.

```html
<!DOCTYPE html>
<!-- ... -->
    <script>
      const $apple = document.getElementById("apple");
      $apple.style.color = "red";
      console.log($apple === apple);
    </script>
  </body>
</html>
```

하지만 만약 id 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 이 전역 변수에 노드 객체가 재할당되지 않는다.

---

## 3. 태그 이름을 이용한 요소 노드 취득 - Document.prototype.getElementsByTagsName()

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul>
      <li id="apple">apple</li>
      <li id="banana">banana</li>
      <li id="orange">orange</li>
    </ul>
    <script>
      const $lis = document.getElementsByTagName("li");
      console.log($lis);
    </script>
  </body>
</html>
```

![getElementsByTagName](/image/JS/DOM/GetElementNode/getElementsByTagsName.png)

`getElementsByTagName` 메서드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 `HTMLCollection` 객체를 반환한다. 이 객체는 유사 배열 객체이면서 이터러블이다.

HTML 문서의 모든 요소 노드를 취득하려면 `getElementsByTagName` 메서드 인수로 `'*'`를 전달한다.

`Document.prototype.getElementsByTagsName()` 메서드는 DOM 전체에서 요소 노드를 탐색하여 반환하는 반면 `Element.prototype.getElementsByTagsName()` 메서드는 특정 요소 노드를 통해 호출되며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.

---

## 4. class를 이용한 요소 노드 취득 - Document.prototype.getElementsByClassName()

```javascript
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul>
      <li class="fruit apple">apple</li>
      <li class="fruit banana">banana</li>
      <li class="fruit orange">orange</li>
    </ul>
    <script>
      const $lis = document.getElementsByClassName("fruit");
      const $apple = document.getElementsByClassName("fruit apple");
      console.log($lis);
      console.log($apple);
    </script>
  </body>
</html>
```

![getElementsByClassName](/image/JS/DOM/GetElementNode/getElementsByClassName.png)

`getElementsByClassName()` 메서드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 `HTMLCollection` 객체를 반환한다.

`Element.prototype.getElementsByClassName()` 메서드는 특정 요소 노드를 통해 호출하며 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색한다.

---

## 5. CSS 선택자를 이용한 요소 노드 취득

### 5-1. Document.prototype/Element.prototype.querySelector()

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul>
      <li class="fruit apple">apple</li>
      <li class="fruit banana">banana</li>
      <li class="fruit orange">orange</li>
    </ul>
    <script>
      const $banana = document.querySelector(".banana");
      console.log($banana);
      $banana.style.color = "red";
    </script>
  </body>
</html>
```

- 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 여려 개인 경우 첫 번째 요소 노드만 반환한다.
- 인수로 전달된 CSS 선택자를 만족시키는 요소 노드가 존재하지 않는 경우 `null`를 반환한다.

---

### 5-2. Document.prototype/Element.prototype.querySelectorAll()

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul>
      <li class="fruit apple">apple</li>
      <li class="fruit banana">banana</li>
      <li class="fruit orange">orange</li>
    </ul>
    <script>
      const $fruit = document.querySelectorAll(".fruit");
      console.log($fruit);
      $fruit.forEach((item) => (item.style.color = "blue"));
    </script>
  </body>
</html>
```

![querySelectorAll](/image/JS/DOM/GetElementNode/querySelectorAll.png)

인수로 전달된 CSS 선택자를 만족시키는 요소가 존재하지 않는 경우 빈 `NodeList` 객체를 반환한다. `NodeList` 객체는 유사 배열 객체이면서 이터러블이다.

HTML 문서의 모든 요소 노도를 취득하려면 `querySelectorAll` 메서드의 인수로 전체 선택자 `'*'`를 전달한다.

---

## 6. 정리

`querySelector` 메서드와 `querySelectorAll` 메서드는 CSS 선택자 문법을 사용하여 좀 더 구체적인 조건으로 요소 노드를 취득할 수 있고 일관된 방식으로 요소 노드를 취득할 수 있다는 장점을 가지는 대신 `getElementById` 메서드, `getElements***` 메서드보다 다소 느리다.

- `id` 값을 가진 요소 노드를 취득하는 경우: `getElementById` 메서드 사용
- 그 외의 요소 노드를 취득하는 경우: `querySelector` 메서드와 `querySelectorAll` 메서드 사용

---

## 7. Conclusion

> 정말.. 오래전? 2년 전에 배웠던 내용을 다시 정리하니 감회가 새롭다. 차이가 있다면 과거엔 아무런 지식 없이 그냥 노드 요소를 취득하기 위한 메서드를 썼다면 이번엔 기준을 잡았다는 것과 유사 배열 객체, 이터러블이라는 개념을 어느 정도 알고 있다는 점이다. 리액트도 재밌고 앞으로 공부를 많이 해야 하지만 항상 기본이 되는 바닐라JS 공부도 꾸준히 하자.

---

## 참고

도서 - 모던 자바스크립트 Deep Dive: 자바스크립트의 기본 개념과 동작 원리
