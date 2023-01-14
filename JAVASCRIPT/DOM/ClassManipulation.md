# 클래스 조작

## 1. 개요

클래스 어트리뷰트 값을 변경하여 여러가지 작업을 할 수 있다. 예를 들어 클래스 선택자를 사용하여 `CSS class`를 미리 정의한 다음 클래스를 추가하여 스타일을 변경할 수 있다.

class 어트리뷰트에 대응하는 DOM 프로퍼티는 class가 아니라 `className`과 `classList`이다. 이를 살펴보자.

---

## 2. className

`Element.prototype.className` 프로퍼티는 HTML 요소의 class 어트리뷰트 값을 취득하거나 변경한다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <style>
      .box {
        width: 50px;
        height: 50px;
      }
      .red {
        background-color: red;
      }
      .blue {
        background-color: blue;
      }
    </style>
  </head>
  <body>
    <div class="box red"></div>
    <script>
      const $box = document.querySelector('div');
      console.log($box.className);
    </script>
  </body>
</html>
```

콘솔의 결과로 `"box red"`가 출력된다. 이렇게 `className` 프로퍼티를 참조하면 class 어트리뷰트 값을 문자열로 반환한다. 하지만 `className` 프로퍼티는 문자열을 반환하므로 공백으로 구분된 여러 개의 클래스를 반환하는 경우 다루기가 불편하다.

즉, 위의 예시에서 `red` 클래스를 `blue` 클래스로 바꾸기 위해선 아래와 같은 작업이 필요하다.

```javascript
$box.className = $box.className.replace('red', 'blue');
// 또는
$box.className = 'box blue';
```

---

## 3. classList

`Element.prototype.classList` 프로퍼티는 class 어트리뷰트의 정보를 담은 `DOMTokenList` 객체를 반환한다. 이 객체는 유사 배열 객체이면서 이터러블이다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <style>
      .box {
        width: 50px;
        height: 50px;
      }
      .red {
        background-color: red;
      }
      .blue {
        background-color: blue;
      }
    </style>
  </head>
  <body>
    <div class="box red"></div>
    <script>
      const $box = document.querySelector('div');
      console.log($box.classList);
    </script>
  </body>
</html>
```

![classList](/image/JS/Dom/ClassManipulation/classList1.png)

`DOMTokenList` 객체는 다양한 메스드를 제공한다.

---

### 3-1. classList - add(...className)

`add()` 메서드는 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값으로 추가한다.

```javascript
$box.classList.add('blue'); // class = "box red blue"
$box.classList.add('foo', 'bar'); // class = "box red blue foo bar"
```

---

### 3-2. classList - remove(...className)

`remove()` 메서드는 인수로 전달한 1개 이상의 문자열과 일치하는 class 어트리뷰트에서 삭제한다. 인수로 전달한 문자열과 일치하는 클래스가 class 어트리뷰트에 없으면 에러 없이 무시된다.

```javascript
$box.classList.remove('foo'); // class = "box red blue bar"
$box.classList.remove('bar', 'blue'); // class = "box red"
```

---

### 3-3. classList - item(index)

`item()` 메서드는 인수로 전달한 `index`에 해당하는 클래스를 class 어트리뷰트에서 반환한다.

```javascript
$box.classList.item(0); // box
$box.classList.item(1); // red
```

위는 아래와 같은 결과를 나타낸다.

```javascript
$box.classList[0];
$box.classList[1];
```

---

### 3-4. classList - contains(className)

`contains()` 메서드는 인수로 전달한 문자열과 일치하는 클래스가 class 어트리뷰트에 포함되어 있는지 확인한다.

```javascript
$box.classList.contains('red'); // ture
$box.classList.contains('blue'); // false
```

---

### 3-5. classList - replace(oldClassName, newClassName)

`replace()` 메서드는 class 어트리뷰트에서 첫 번째 인수로 전달한 문자열을 두 번째 인수로 전달한 문자열로 변경한다.

```javascript
$box.classList.replace('red', 'blue'); // class = "box blue"
```

---

### 3-6 classList - toggle(className[, force])

`toggle()` 메서드는 class 어트리뷰트에 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거하고, 존재하지 않으면 추가한다.

```javascript
$box.classList.toggle('border');
```

두 번째 인수로 불리언 값으로 평가되는 조건식을 전달할 수 있다.

- `true`이면 강제로 첫 번째 인수로 전달받은 문자열을 추가
- `false` 이면 첫 번째 인수로 전달받은 문자열을 제거

---

## 4. Conclusion

> `classList` 프로퍼티는 바닐라 자바스크립트로 프로젝트를 할 때 자주 사용했던 것 중 하나이다. `add()`, `remove()` 메서드를 많이 사용했는데 다음에 사용할 기회가 생긴다면 `toggle()` 메서드를 적극적으로 사용해봐야겠다.

---

## 참고

도서 - 모던 자바스크립트 Deep Dive: 자바스크립트의 기본 개념과 동작 원리

---

📅 2022-10-16
