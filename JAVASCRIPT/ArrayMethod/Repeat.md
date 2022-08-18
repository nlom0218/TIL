# 배열의 요소를 순회하기(forEach, map, some, every)

## 1. 개요

이번 챕터에서는 배열의 요소 가지고 특정 작업을 하나하나 수행하는 메서드에 대해 살펴본다. 나는 이와 같은 과정을 배열의 요소를 순회한다고 표현한다. 물론 이번 챕터에서 다루지 않는 다른 메서드 중에서도
배열를 순회하는 메서드도 있다. 그렇다면 왜 다 다루지 않고 4가지만 다루는 것일까? 이번 챕터에서 설명하는 메서드를 선택한 기준은 다음과 같다.

1. 배열의 요소를 통해 특정 작업을 하는 메서드
2. 반복하는 과정이 머리속으로 떠올려지는 메서드
3. 콜백함수가 있는 메서드
4. 위의 조건이 모두 만족하더라도 메서드가 특정 작업을 수행할 때 사용되는 것이라면 제외

`Array.filter()`, `Array.find()`, `Array.reduce()`, `Array.sort()`, `Array.findIndex()`는 모두
위의 1, 2, 3번 조건을 모두 만족하지만 그들만이 가지는 특정 작업을 수행하기 때문에 다른 챕터에서 다룬다.

---

## 2. Array.forEach()

- **특징**
  - `for` 문을 대체할 수 있는 고차 함수
  - 반복문을 통해 자신을 호출한 배열을 순회하면서 수행해야 할 처리를 콜백 함수로 전달받아 반복 호출
  - 원본 배열을 변경하지 않지만 콜백 함수를 통해 원본 배열을 변경할 수 있음
  - `break`, `continue`을 사용할 수 없음
- **반환 값**: undefined
- **구문**
  ```javascript
  arr.forEach(callback(currentvalue[, index[, array]])[, thisArg])
  ```
- **매개변수**
  - `callback`: 각 요소에 대해 실행할 함수, 세 가지 매개변수를 받음
    - `currentvalue`: 처리할 현재 요소
    - `index`: 처리할 현재 요소의 인덱스
    - `array`: `forEach()`를 호출한 배열
  - `thisArg`: callback을 실행할 때 this로 사용할 값

예시를 통해 `Array.forEach()`메서드의 사용법을 알아보자.

---

### 2-1. 콜백 함수의 반복 호출

```javascript
const number = [1, 2, 3, 4, 5];
number.forEach((item, index, arr) => {
  console.log(`item: ${item}, index: ${index}, arr: ${arr}`);
});
```

![forEach 1](/image/JS/ArrayMethod/Repeat/forEach1.png)

---

### 2-2. 새로운 배열 만들기

각각의 숫자의 제곱을 가지는 새로운 배열을 만들어 보자.

```javascript
const number = [1, 2, 3, 4, 5];
const newNumber = [];
number.forEach((item) => {
  newNumber.push(item * item);
});
```

콜백 함수에서의 `index`, `arr`는 필요가 없으므로 생략한다.

![forEach 2](/image/JS/ArrayMethod/Repeat/forEach2.png)

---

### 2-3. 원본 배열 수정하기

`Array.forEach()` 메서드의 콜백 함수는 세 개의 매개변수를 가진다고 했다. 그 중 세 번째가 원본 배열을 가리키게 되는데
이를 직접 변경하면 원본 배열이 변경된다.

```javascript
const number = [1, 2, 3, 4, 5];
number.forEach((item, index, arr) => {
  arr[index] = item * item;
});
```

![forEach 3](/image/JS/ArrayMethod/Repeat/forEach3.png)

---

## 3. Array.map()

- **특징**
  - 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환
  - 원본 배열은 변경되지 않음
  - `Array.forEach()` 메서드는 단순히 반복문을 대체하기 위한 고차 함수이고, `Array.map()` 메서드는 요소값을 다른 값으로 매핑한 새로운 배열을 생성하기 위한 고차 함수이다.
  - 콜백 함수에서 특정 값을 리턴하지 않으면 `undefined`가 리턴됨
- **반환 값**: 배열의 각 요소에 대해 실행한 callback의 결과를 모은 새로운 배열
- **구문**
  ```javascript
  arr.map(callback(currentValue[, index[, array]])[, thisArg])
  ```
- **매개변수**
  - `callback`: 각 요소에 대해 실행할 함수, 세 가지 매개변수를 받음
    - `currentvalue`: 처리할 현재 요소
    - `index`: 처리할 현재 요소의 인덱스
    - `array`: `map()`를 호출한 배열
  - `thisArg`: callback을 실행할 때 this로 사용할 값

예시를 통해 `Array.forEach()`메서드의 사용법을 알아보자.

---

### 3-1. 새로운 배열 만들기

새로운 배열을 만들 때는 `Array.forEach()` 메서드 보단 `Array.map()` 메서드를 사용한다. 이는 `Array.map()` 메서드가
새로운 배열을 반환하기 때문이다.

아래는 원본 배열의 요소(문자열)에 "Hello,"를 추가한 새로운 배열(hello)을 만드는 코드이다.

```javascript
const name = ["hd", "kh", "by", "gy"];
const hello = name.map((item) => `Hello, ${item}`);
```

![map](/image/JS/ArrayMethod/Repeat/map.png)

---

### 3-2. 배열 속 객체 재구성하기

순서대로 이름, 나이, 성별을 요소로 가지는 2차원 배열있다고 가정하자. 각 요소를 다른 형태로 재구성하여 새로운 배열로 만들 수 있다.

```javascript
const users = [
  ["hd", 29, "male"],
  ["kh", 27, "female"],
  ["gy", 30, "male"],
];
const users_info = users.map((item) => {
  const [name, age, gender] = item;
  return {
    name,
    age,
    gender,
  };
});
```

![map 2](/image/JS/ArrayMethod/Repeat/map2.png)

---

## 4. Array.some()

- **특징**
  - 배열 안의 어떤 요소라도 주어진 콜백 함수를 통과하는지 테스트
  - 콜백 함수가 `true`를 반환하는 요소를 찾을 때까지 배열에 있는 각 요소에 대해 한 번씩 콜백 함수를 실행
  - `true`에 해당하는 요소를 발견한 경우 즉시 `true`를 반환하고 순회를 종료
- **반환 값**: 콜백 함수의 반환값이 단 한 번이라도 참이면 `true`, 모두 거짓이라면 `false`를 반환
- **구문**
  ```javascript
   arr.some(callback[, thisArg])
  ```
- **매개변수**
  - `callback`: 각 요소에 대해 실행할 함수, 세 가지 매개변수를 받음
    - `currentvalue`: 처리할 현재 요소
    - `index`: 처리할 현재 요소의 인덱스
    - `array`: `map()`를 호출한 배열
  - `thisArg`: callback을 실행할 때 this로 사용할 값

예시를 통해 `Array.forEach()`메서드의 사용법을 알아보자.

---

### 4-1. 배열의 요소 테스트하기

배열의 요소가 중 하나라도 10보다 큰 요소가 존재하는지와 3보다 작은 요소가 존재하는지 테스트하는 코드이다.

```javascript
const number = [4, 5, 8, 11];
const isGreater = number.some((item) => item > 10);
const isLess = number.some((item) => item < 3);
```

10보다 큰 요소는 하나 존재하지만 모두 3보다 크다. 때문에 아래와 같은 결과가 나타난다.

![some 1](/image/JS/ArrayMethod/Repeat/some1.png)

---

### 4-2. 특정 조건을 만족한 경우 순회 종료하기

`Array.forEach()` 메서드는 순회하는 도중에 종료를 할 수 없다. 모든 요소에 대해 순회를 해야한다.
만약 중간에 순회를 그만두고 싶을 땐 `Array.some()` 메서드를 사용하면 된다. 그만두고 싶을 때 `true`를 리턴하면 되기 때문이다.

요소를 순회하면서 차례대로 더하고 있다. 그리고 더한 값은 `sum` 변수에서 관리하고 있다. 차례대로 더하다가 `sum`이 20보다 커지게 되면
요소의 순회를 종료한다.

```js
const number = [4, 5, 8, 2, 3, 7, 1];
let sum = 0;
number.some((item) => {
  sum += item;
  console.log(item, sum);
  return sum > 20;
});
```

`number` 배열의 요소인 7과 1은 호출되지 않는 것을 확인할 수 있다.

![some 2](/image/JS/ArrayMethod/Repeat/some2.png)

---

## 5. Array.every()

- **특징**
  - 배열 안의 모든 요소가 주어진 콜백 함수를 통과하는지 테스트
  - 콜백 함수가 `false`를 반환하는 요소를 찾을 때까지 배열에 있는 각 요소에 대해 한 번씩 콜백 함수를 실행
  - `false`에 해당하는 요소를 발견한 경우 즉시 `false`를 반환하고 순회를 종료
- **반환 값**: 콜백 함수의 반환값이 모두 참이면 `true`, 단 한 번이라도 거짓이면 `false`를 반환
- **구문**
  ```javascript
   arr.every(callback[, thisArg])
  ```
- **매개변수**
  - `callback`: 각 요소에 대해 실행할 함수, 세 가지 매개변수를 받음
    - `currentvalue`: 처리할 현재 요소
    - `index`: 처리할 현재 요소의 인덱스
    - `array`: `map()`를 호출한 배열
  - `thisArg`: callback을 실행할 때 this로 사용할 값

예시를 통해 `Array.every()`메서드의 사용법을 알아보자.

---

### 5-1. 배열의 요소 테스트하기

배열의 요소가 모두 10보다 큰 수인지 테스트하는 코드이다.

```javascript
const number1 = [11, 24, 52];
const number2 = [9, 12, 32];
const test1 = number1.every((item) => item > 10);
const test2 = number2.every((item) => item > 10);
```

`number1` 배열에는 10보다 큰 요소만 존재한다.

`number2` 배열에는 10보다 작은 요소가 존재한다.

![every 1](/image/JS/ArrayMethod/Repeat/every1.png)

---

### 5-2. 특정 조건을 만족한 경우 순회 종료하기

`Array.some()` 에서는 특정 조건이 만족한 이후 `true`를 리턴하여 요소의 순회를 중단한다. 반대로 `Array.every()` 메서드에서는
`false`를 리턴함으로써 요소의 순회를 중단할 수 있다.

```js
const number = [4, 5, 8, 2, 3, 7, 1];
let sum = 0;
number.every((item) => {
  sum += item;
  console.log(item, sum);
  return sum < 15;
});
```

`number` 배열의 요소인 2, 3, 7, 1은 호출되지 않는 것을 확인할 수 있다.

4, 5, 8을 더하면 17이므로 15보다 작아 `false`를 리턴하기 때문이다.

---

## 6. Conclusion

> `Array.forEach()` 메서드와 `Array.map()` 메서드는 정~~말 많이 사용하는 메서드이다. 그래서 지금은 상황에 맞게 잘 사용을 하고 있다.
> 하지만 처음 배웠을 땐 두 메서드가 별다른 차이가 없어보여 메서드의 의미를 생각하고 사용하느라 애좀 먹었었다.😭  
> `Array.some()` 메서드와 `Array.every()` 메서드는 있는 것만 알고 많이 사용은 하지 않았다. 그래도 요소의 순회를 중간에
> 중단하고 싶을 때 유용하다는 것을 잘 활용해야겠다. 특히 요즘엔 코딩 테스트 문제를 풀 때 상황에 맞게 잘 사용해야겠다.😀

---

## 참고

[MDN - Array.prototype.forEach()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)  
[MDN - Array.prototype.map()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)  
[MDN - Array.prototype.some()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/some)  
[MDN - Array.prototype.every()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/every)

---

---

[👆](#배열의-요소를-순회하기foreach-map-some-every)

📅 2022-08-18
