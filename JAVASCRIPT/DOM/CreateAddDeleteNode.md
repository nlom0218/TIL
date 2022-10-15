# 노드 생성과 추가 및 삭제

## 1. 개요

`innerHTML` 프로퍼티와 `insertAdjacentHTML` 메서드는 문자열을 파싱하여 노드를 생성하고 DOM에 반영한다. 다른 방법으로도 요소 노드, 텍스트 노드를 생성하고 추가하여 DOM에 반영할 수 있다. 이 방법에 대해 살펴본다.

---

## 2. 요소 노드 생성 - createElement(tagName)

`Document.prototype.createElement(tagName)` 메서드는 요소 노드를 생성하여 반환한다. 매개변수는 태그 이름을 나타내는 문자열이다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul></ul>
    <script>
      const $ul = document.querySelector("ul");
      const $li = document.createElement("li");
    </script>
  </body>
</html>
```

위 과정을 통해 `<ul></ul>`과 `<li></li>`가 만들어진다. 만들어진 요소 노드를 각각의 변수에 할당한다. 하지만 위의 과정을 통해 DOM에 추가되지는 않는다. 따라서 이후에 생성된 요소 노드를 DOM에 추가하는 처리가 필요하다.

---

## 3. 텍스트 모드 생성 - createTextNode(text)

`Document.prototype.createTextNode(text)` 메서드는 텍스트 노드를 생성하여 반환한다. 매개변수는 생성할 텍스트를 문자열이다.

```javascript
const textNode = document.createTextNode("Cat");
```

위 과정을 통해 `Cat`이라는 텍스트가 담긴 텍스트 노드가 만들어진다. 이 또한 DOM에 추가하는 작업을 하지 않아 추가하는 처리가 필요하다.

---

## 4. 자식 노드로 추가 - appendChild(childNode)

`node.prototype.appendChild(childNode)` 메서드는 매개변수 childNode에게 인수로 전달한 노드를 `apppendChild()` 메서드를 호출한 노드의 마지막 자식 노드로 추가한다.

---

### 4-1. 텍스트 노드를 $li 요소 노드에 추가

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul></ul>
    <script>
      const $ul = document.querySelector("ul");
      const $li = document.createElement("li");
      const textNode = document.createTextNode("Cat");

      $li.appendChild(textNode);
      console.log($li);
    </script>
  </body>
</html>
```

![appendChild1](/image/JS/DOM/CreateAddDeleteNode/appendChild1.png)

`textNode`를 `$li` 자식으로 추가하는 과정이다. 위와 같은 경우 요소 노드에 자식 노드가 하나도 없는 경우에는 텍스트 노드를 생성하여 요소 노드의 자식 노드로 텍스트 노드를 추가하는 것보다 `textContent` 프로퍼티를 사용하는 편이 더욱 간편하다.

---

### 4-2. $li 요소 노드를 $ul 요소 노드에 추가

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul></ul>
    <script>
      const $ul = document.querySelector("ul");
      const $li = document.createElement("li");
      const textNode = document.createTextNode("Cat");

      $li.appendChild(textNode);
      $ul.appendChild($li);
      console.log($ul);
    </script>
  </body>
</html>
```

![appendChild2](/image/JS/DOM/CreateAddDeleteNode/appendChild2.png)

위 과정을 통해 새롭게 생성한 요소 노드가 DOM에 추가된다.

---

## 5. 복수의 노드 생성과 추가

복수의 노드를 생성할 땐 어떻게 해야할까? 아래의 예제를 살펴보자.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul></ul>
    <script>
      const $ul = document.querySelector("ul");
      const animal = ["Cat", "Dog", "Lion"];
      animal.forEach((item) => {
        const $li = document.createElement("li");
        const textNode = document.createTextNode(item);
        $li.appendChild(textNode);
        $ul.appendChild($li);
      });
    </script>
  </body>
</html>
```

위의 과정을 통해 `ul` 요소 노드에 3개의 `li` 요소 노드가 자식 노드로 추가된다. 하지만 위 과정에선 3번의 리플로우와 리페인트가 실행되어 효율적이지가 않다.

이 문제는 아래의 방법으로 해결할 수 있다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul></ul>
    <script>
      const $ul = document.querySelector("ul");
      const $fr = document.createDocumentFragment();
      const animal = ["Cat", "Dog", "Lion"];
      animal.forEach((item) => {
        const $li = document.createElement("li");
        const textNode = document.createTextNode(item);
        $li.appendChild(textNode);
        $fr.appendChild($li);
      });
      $ul.appendChild($fr);
    </script>
  </body>
</html>
```

반복문이 종료되고 마지막에 `$ul` 요소 노드에 `$fr` 요소 노드가 추가되므로
리플로우와 리페인트가 한 번만 발생한다.

`createDocumentFragment()` 메서드는 `DocumentFragment` 노드를 만들어 주는데 이 노드는 DOM에 추가하면 자신은 제거되고 자신의 자식 노드만 DOM에 추가되는 특징을 가지고 있다.

---

## 6. 지정한 위치에 자식 노드 추가 - insertBefore()

마지막 위체에 노드를 추가하는 것이 아니라 특정 위치에 자식 노드를 추가하기 위해서는 `Node.prototype.insertBefore(newNode, childNode)` 메서드를 사용한다.

두 번째 인수로 전달받은 노드는 반드시 `insertBefore()` 메서드를 호출한 노드의 자식 노드이어야 한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul>
      <li id="dog">dog</li>
      <li id="cat">cat</li>
    </ul>
    <script>
      const $ul = document.querySelector("ul");
      const $cat = document.getElementById("cat");

      const $li = document.createElement("li");
      $li.textContent = "lion";

      $ul.insertBefore($li, $cat);
    </script>
  </body>
</html>
```

![insertBefore](/image/JS/DOM/CreateAddDeleteNode/insertBefore.png)

`cat` 전에 `lion`이 생성되어 추가되었다.

---

## 7. 노드 삭제 - removeChild()

`Node.prototype.removeChild(child)` 메서드는 `child` 매개변수에 인수로 전달한 노드를 DOM에서 삭제한다. 인수로 전달한 노드는 `removeChild()` 메서드를 호출한 노드의 자식 노드이어야 한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul>
      <li id="dog">dog</li>
      <li id="cat">cat</li>
    </ul>
    <script>
      const $ul = document.querySelector("ul");
      const $cat = document.getElementById("cat");
      $ul.removeChild($cat);
    </script>
  </body>
</html>
```

![removeChild](/image/JS/DOM/CreateAddDeleteNode/removeChild.png)

`cat`이 삭제된 것을 확인할 수 있다.

---

## 8. Conclusion

> 요소 노드와 텍스트 노드를 생성하고 DOM에 추가하는 방법과 노드를 삭제하는 방법도 알아봤다. 이런 경우는 코드가 길어지기 때문에 함수로 만들어 호출을 통해 사용하는 것이 가독성이 좋을 듯 하다. 또한 텍스트 노드의 생성같은 경우 `textContent` 프로퍼티 사용이 더 편리하니 상황에 따라 복잡하지 않게 코드를 구현하자.

---

## 참고

도서 - 모던 자바스크립트 Deep Dive: 자바스크립트의 기본 개념과 동작 원리

---

📅 2022-10-16
