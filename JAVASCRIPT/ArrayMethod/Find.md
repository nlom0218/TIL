# 원하는 요소 찾기(find)

## 1. 개요

이번 챕터에서 다루게 될 `Array.find()` 메서드는 배열에서 원하는 요소를 찾을 때 유용하다. 만약 요소가 아니라
요소의 인덱스를 찾고 싶으면 `Array.findIndex()` 메서드를 사용하면 된다.

## 2. Array.find()

- **특징**
  - 콜백 함수를 만족하는 첫 번째 요소의 값을 반환
  - 배열을 반환하는 것이 아니라 해당 요소값을 반환
- **반환 값**: 주어진 콜백 함수의 반환값이 `true`인 첫 번째 요소, `true`인 요소가 존재하지 않는다면 `undefined`를 반환
- **구문**
  ```javascript
  arr.find(callback[, thisArg])
  ```
- **매개변수**
  - `callback`: 배열의 각 값에 대해 실행할 함수, 아래의 3개의 인수를 받음(아래참고)
    - `element`: 콜백 함수에서 처리할 현재 요소
    - `index`: 콜백 함수에서 처리할 현재 요소의 인덱스
    - `array`: `find()`를 호출한 배열
  - `thisArg`: callback을 실행할 때 this로 사용할 값

몇가지의 예시를 통해 `Array.find()`메서드의 사용법을 알아보자.

---

### 2-1. 속성 중 하나를 사용하여 배열에서 객체 찾기

```javascript
const userList = [
  { id: 1, name: "HD", age: 29 },
  { id: 2, name: "KH", age: 27 },
  { id: 3, name: "BY", age: 29 },
  { id: 4, name: "GY", age: 30 },
];
const user1 = userList.find((item) => item.name === "GY");
const user2 = userList.find((item) => item.age === 27);
const user3 = userList.find((item) => item.age === 29);
```

각각 콜백 함수의 반환값이 `true`인 첫 번째 요소를 반환한다.
`age`가 29인 요소는 첫 번째와 세 번째가 있지만 이 중 처음으로 `true`을 반환하는 요소는 첫 번째 요소이다.

![find 1](/image/JS/ArrayMethod/Find/find_1.png)

---

### 2-2. 특정 조건을 만족하는 첫 번째 요소 찾기

```javascript
const nums = [1, 3, 5, 11, 12, 19, 8, 2];
const even = nums.find((item) => item % 2 === 0);
const evenIndex = nums.findIndex((item) => item % 2 === 0);
const evens = nums.filter((item) => item % 2 === 0);
```

배열 중 첫 번째로 등장하는 짝수를 찾고있다. 추가로 `Array.findIndex()` 메서드를 사용하여 첫 번째로
등장하는 짝수의 인덱스를 찾고, `Array.filter()` 메서드를 사용하여 `nums` 배열에서 짝수인 요소만 추려서 새로운
배열을 만들었다.

![find 2](/image/JS/ArrayMethod/Find/find_2.png)

---

### 2-3. 앞에서 부터가 아닌 뒤에서 부터 요소를 찾고 싶으면?

`Array.find()` 메서드는 0번 인덱스 부터 요소를 찾아간다. 하지만 뒤에서 부터 요소를
찾고 싶으면 어떻게 해야할까? 이런 경우 `Array.findLast()` 메서드를 사용하면 된다.

```javascript
const nums = [1, 3, 5, 11, 12, 19, 8, 2];
const even = nums.findLast((item) => item % 2 === 0);
```

![find 3](/image/JS/ArrayMethod/Find/find_3.png)

---

## 3. Conclusion

> 어렵지 않고 복잡하지 않은 메서드! 지금까지 많이 사용은 하지 않은 메서드! 필요할 때가 오겠지?

---

## 참고

[MDN - Array.prototype.find()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find)

---

📅 2022-08-20
