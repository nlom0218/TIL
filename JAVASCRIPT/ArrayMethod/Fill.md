# 배열에 정적인 값으로 채우기(fill)

## 1. 개요

ES6에서 도입된 `Array.fill()` 메서드에 대해 살펴본다.

---

## 2. Array.fill()

- **특징**
  - 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채움
  - 원본 배열이 수정됨
- **반환 값**: 변형한 배열
- **구문**
  ```javascript
    arr.fill(value[, start[, end]])
  ```
- **매개변수**

  - `value`: 배열을 채울 값
  - `start`: 시작 인덱스, 기본 값은 0
  - `end`: 끝 인덱스, 기본 값은 배열의 길이

예시를 통해 `Array.fill()`의 사용법을 살펴보자.

---

### 2-1. 첫 번째 인수만 전달할 경우

```javascript
const arr = [1, 2, 3];
arr.fill("hi!");
```

![fill 1](/image/JS/ArrayMethod/Fill/fill1.png)

첫 번째 인수만 전달하게 되면 전달 받은 값을 배열의 처음부터 끝까지 요소로 채운다.

---

### 2-2. 두 번째 인수가 함께 전달할 경우

```javascript
const arr = [1, 2, 3, 4, 5];
arr.fill("hello!", 2);
```

![fill 2](/image/JS/ArrayMethod/Fill/fill2.png)

두 번째 인수로 요소 채우기를 시작할 인덱스를 전달할 수 있다.

---

### 2-3. 세 번째 인수가 함께 전달할 경우

```javascript
const arr = [1, 2, 3, 4, 5];
arr.fill("hello!", 2, 4);
```

![fill 3](/image/JS/ArrayMethod/Fill/fill3.png)

세 번째 인수로 요소 채우기를 멈출 인덱스를 전달 할 수 있다. 이때, 세 번째 인수로 전달 받은 인덱스는 포함하지 않는다.
위의 예시에서는 2부터 3(4는 미포함)까지 "hello!"로 채워진다.

---

## 3. Conclusion

> `Array.fill()`를 제대로 사용해 본 적은 아직 없다. 지금까지 계속 프로젝트를 진행하면서 배열에 담겨진 정보를
> 그대로 사용해서 그런듯하다. 하지만 앞으로 언젠가는 필요할 날이 있을거니 이러한 메서드가 있다는 것을 잘 알두고 넘어가도록 하자.
> 어렵고 복잡한 메서드가 아니니 필요한 상황이 오면 잘 사용하도록 하자.

---

## 참고

도서 - 모던 자바스크립트 Deep Dive: 자바스크립트의 기본 개념과 동작 원리  
[MDN - Array.prototype.fill()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)

---

📅 2022-08-26
