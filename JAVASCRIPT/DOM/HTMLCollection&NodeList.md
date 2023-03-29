# HTMLCollection과 NodeList

## 1. 개요

DOM 컬렉션 객체인 `HTMLCollection`과 `NodeList`는 DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체다. `HTMLCollection`과 `NodeList` 어떤 특징을 가지고 있을까?

공동적으로 둘 다 `유사 배열 객체`이면서 `이터러블`이다. 그렇기 때문에 `for...of` 문으로 순회할 수 있으며 `스프레드 문법`을 사용하여 간단히 배열로 변환할 수 있다.

***

## 2. HTMLCollection

`HTMLCollection` 객체는 `getElementsByTagName`, `getElementsByClassName` 메서드가 반환하는 객로 `살아 있는(live)` DOM 컬렉션 객체이다.

`살아 있는(live)` 이란 뜻을 이해하기 위해 아래의 예시를 살펴보자.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <style>
      .blue {
        color: blue;
      }
    </style>
  </head>
  <body>
    <ul>
      <li class="fruit">apple</li>
      <li class="fruit">banana</li>
      <li class="fruit">orange</li>
    </ul>
    <script>
      const $fruit = document.getElementsByClassName("fruit");
      for (let i = 0; i < $fruit.length; i++) {
        $fruit[i].className = "blue";
      }
    </script>
  </body>
</html>
```

위의 결과는 어떻게 예상이 되는가? 모든 li 요소가 파란색으로 랜더링이 될 것 같지만 그렇지 않다. 결과는 아래의 사진과 같다.

![HTMLCollection rendering](../../image/JS/DOM/HTMLCollection\&NodeList/HTMLCollection1.png)

이런 결과가 나타나는 이유를 알아보자.

1. 첫 번째 반복문(i === 0) 첫 번째 `li` 요소에 `fruit` 클래스가 `blue` 클래스로 변경된다. 이때 더 이상 `$fruit`에서 해당 노드 요소는 실시간으로 제거된다. 이처럼 `HTMLCollection` 객체는 실시간으로 노드 객체의 상태 변경을 반영하는 `살아 있는(live)` DOM 컬렉션 객체이다.
2. 두 번재 반복문(i === 1) 첫 번째 반복에서 첫 번째 `li` 요소는 `$fruit`에서 제거되었다. 따라서 `$fruit[i]`는 세 번째 `li` 요소이다. 즉 `orange`를 포함한 `li` 요소의 클래스가 `blue`로 변경된다.
3. 세 번째 반복문(i === 2) 현재 `$fruit` 객체엔 2개의 요소가 있다. 즉 인덱스가 `2`인 요소는 없으므로 `false`가 되어 반복이 종료된다.

이를 해결할 수 있는 방법은 아래와 같다.

```html
<script>
  const $fruit = document.getElementsByClassName("fruit");
  for (let i = 0; i < $fruit.length; i++) {
    $fruit[i].className = "fruit blue";
  }
</script>
```

```html
<script>
  const $fruit = document.getElementsByClassName("fruit");
  while ($fruit.length > 0) {
    $fruit[0].className = "blue";
  }
</script>
```

아래는 가장 좋은 해결책이다. 유사 배열 객체이면서 이터러블인 `HTMLCollection` 객체를 배열로 변환하여 해결한다.

```html
<script>
  const $fruit = document.getElementsByClassName("fruit");
  [...$fruit].forEach((item) => (item.className = "blue"));
</script>
```

***

## 3. NodeList

`HTMLCollection` 객체의 문제를 해결하기 위해 `querySelectorAll` 메서드를 사용하는 방법도 있다. `querySelectorAll` 메서드는 DOM 컬렉션 객체인 `NodeList` 객체를 반환한다. 이 객체는 실시간으로 노드 객체의 상태 변경을 반영하지 않는`(non-live)` 객체이다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <style>
      .blue {
        color: blue;
      }
    </style>
  </head>
  <body>
    <ul>
      <li class="fruit">apple</li>
      <li class="fruit">banana</li>
      <li class="fruit">orange</li>
    </ul>
    <script>
      const $fruit = document.querySelectorAll(".fruit");
      $fruit.forEach((item) => (item.className = "blue"));
    </script>
  </body>
</html>
```

`NodeList` 객체는 `NodeList.prototype.forEach` 메서드를 상속받아 사용할 수 있는 편리함이 있다.

이렇게 `NodeList` 객체는 대부분의 경우 노드 객체의 상태 변경을 실시간으로 반영하기 않고 과거의 정적 상태를 유지하는 `non-live` 객체로 동작한다.

> 하지만, 그래도 `HTMLCollection` 객체든 `NodeList` 객체든 배열로 변환하여 사용하는 것이 좋다.

`HTMLCollection` 객체와 `NodeList` 객체는 모두 유사 배열 객체이면서 이터러블하기 때문에 스프레드 문법이나 Array.from 메서드를 사용하여 간단히 배열로 변환할 수 있다.

***

## 4. Conclusion

> 과거 바닐라 자바스크립트로 웹 개발 공부를 할 때 막혔던 부분 중 하나가 바로 `HTMLCollection` 객체와 `NodeList` 객체였다. 그 땐 유사 배열 객체, 이터러블이라는 개념을 잘 알지 못하고 또한 배열, 배열 메서드에 대해서도 깊게 알지 못하여 어떻게 활용을 할 지 감이 잡히지 않아서였다. 하지만 지금은 여러개의 요소 노드를 관리해야 할 상황이 온다면 어려움, 걱정 없이 잘 해결해 나갈 수 있는 자신감이 든다.

***

## 참고

도서 - 모던 자바스크립트 Deep Dive: 자바스크립트의 기본 개념과 동작 원리

***

📅 2022-10-11
