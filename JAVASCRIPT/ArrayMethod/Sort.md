# 배열 정렬하기(sort)

## 1. 개요

배열 요소의 순서를 정렬할 수 있는 메서드인 `Array.sort()`에 대해 다루어 본다.

---

## 2. Array.sort()

- **특징**
  - 배열의 요소를 정렬
  - 원본 배열을 직접 변경
  - 기본적으로 오름차순으로 요소를 정렬
- **반환 값**: 정렬한 배열
- **구문**
  ```javascript
  arr.sort([compareFunction]);
  ```
- **매개변수**
  - `compareFunction`
    - 요소의 값을 비교하는 함수
    - 정렬 순서를 정의하는 함수
    - 생략하면 배열은 각 요소의 문자열 변환에 따라 각 문자의 유니코드 코드 포인트 값에 따라 정렬

예시를 통해 `Array.sort()`의 사용법을 살펴보자.

---

### 2-1. 오름차순으로 정렬

```javascript
const color = ["black", "red", "yello", "blue"];
color.sort();
```

![sort 1](/image/JS/ArrayMethod/Sort/sort1.png)

```javascript
const lang = ["자바스크립트", "자바", "파이썬", "고"];
lang.sort();
```

![sort 2](/image/JS/ArrayMethod/Sort/sort2.png)

`Array.sort()` 메서드에 인자를 전달하지 않는다면 기본적으로 오름차순으로 정렬된다.

문자열 요소로 이루어진 배열은 문제가 없지만 숫자 요소로 이루어진 배열을 위와 같이 정렬하면 문제가 생기므로 주의해야 한다.

```javascript
const nums = [1, 5, 20, 100, 30, 7];
nums.sort();
```

![sort 3](/image/JS/ArrayMethod/Sort/sort3.png)

이러한 이유는 `Array.sort()` 메서드의 기본 정렬 순서는 유니코드 코드 포인트의 순서를 따르기 때문이다.
배열의 요소가 숫자 타입이라 할지라도 배열의 요소를 문자열로 변환한 후 유니코드 코드 포인트의 순서를 기준으로 정렬한다.

---

### 2-2. 비교 함수를 사용하여 숫자 요소 정렬

1. 오름차순

   ```javascript
   const nums = [1, 5, 20, 100, 30, 7];
   nums.sort((a, b) => a - b);
   ```

   ![sort 4](/image/JS/ArrayMethod/Sort/sort4.png)

2. 내림차순

   ```javascript
   const nums = [1, 5, 20, 100, 30, 7];
   nums.sort((a, b) => b - a);
   ```

   ![sort 5](/image/JS/ArrayMethod/Sort/sort5.png)

숫자 요소를 정렬할 때는 `Array.sort()` 메서드에 정렬 순서를 정의하는 비교 함수를 인수로 전달해야 한다.
비교 함수는 아래의 값을 반환해야 한다.

- 양수: 두 번째 인수를 우선하여 정렬
- 0: 정렬하지 않음
- 음수: 첫 번째 인수를 우선하여 정렬

> 비교 함수의 반환값이 0인 경우의 정렬 방식은 ECMAScript 사양에 명시되어 있지 않으므로 자바스크립트 엔진마다 동작이 다를 수 있음

---

### 2-3. 객체를 요소로 갖는 배열 정렬

```javascript
const user = [
  { name: "HD", age: 29 },
  { name: "KH", age: 27 },
  { name: "GY", age: 30 },
  { name: "KD", age: 26 },
  { name: "ABC", age: 25 },
];
user.sort((a, b) =>
  a["name"] > b["name"] ? 1 : a["name"] < b["name"] ? -1 : 0
);
```

![sort 6](/image/JS/ArrayMethod/Sort/sort6.png)

배열 요소의 "name" 프로퍼티를 비교하여 오름차순으로 정렬을 하였다. 프로퍼티 값이 문자열인 경우 산술 연산으로
비교할 수 없으므로 비교 연산을 사용한다.(비교 연산은 항상 양수, 0, 음수 중 하나를 반환한다.)

비교함수를 아래와 같이 정의하여 사용할 수도 있다.

```javascript
const compare = (key) => {
  return (a, b) => (a[key] > b[key] ? 1 : a[key] < b[key] ? -1 : 0);
};
const user = [
  { name: "HD", age: 29 },
  { name: "KH", age: 27 },
  { name: "GY", age: 30 },
  { name: "KD", age: 26 },
  { name: "ABC", age: 25 },
];
user.sort(compare("age"));
```

![sort 7](/image/JS/ArrayMethod/Sort/sort7.png)

배열 쇼오의 "age" 프로퍼티를 비교하여 오름차순으로 정렬을 하였다.

---

## 3. Conclusion

> `Array.sort()`는 많은 곳에서 사용하고 있다. 프로젝트를 하면서 게시물, 학생 번호, 학생 이름 순으로 정렬할 때 많이 사용하고
> 최근 코딩 테스트 문제를 풀 때에도 많이 사용한다. 지금까지는 요소가 숫자일 땐 비교 함수를 써야하는지 모르는
> 상태에서 사용을 했지만 오늘 공부한 내용에서 그 이유를 알게 되어 속이 시원한 느낌이 들었다. 그리고 `compare`함수를
> 잘 활용하여 `Array.sort()` 메서드를 적극적으로 사용해보도록 하자.

---

## 참고

도서 - 모던 자바스크립트 Deep Dive: 자바스크립트의 기본 개념과 동작 원리  
[MDN - Array.prototype.sort()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

---

📅 2022-08-31
