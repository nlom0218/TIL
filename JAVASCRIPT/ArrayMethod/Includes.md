# 배열에 특정 요소가 있는지 확인하기(includes)

## 1. 개요

이번 챕터에서는 배열에 특정 요소가 포함되어 있는지 확인하는 방법에 대해 알아본다.

---

## 2. Array.includes()

- **특징**
  - 배열에 특정 요소를 포함하고 있는지 판별
- **반환 값**: 특정 요소가 배열에 있으면 `true` 없으면 `false`를 반환
- **구문**
  ```javascript
  arr.includes(valueToFind[, fromIndex])
  ```
- **매개변수**
  - `valueToFind`: 탐색할 요소, 대소문자를 구분
  - `fromIndex`: 검색을 시작할 배열의 위치, 음의 값일 경우 `array.length + fromIndex`부터 검색을 시작함

예시를 통해 `Array.includes()`의 사용법을 살펴보자.

### 2-1. 첫 번째 매개변수가 주어진 경우

```javascript
const array = ["red", "blue", "green", "black"];
const result1 = array.includes("blue");
const result2 = array.includes("Green");
```

![includes 1](/image/JS/ArrayMethod/ArrayIncludes/array_includes1.png)

`blue`은 `array` 배열에 존재하지만 `Green`은 존재하지 않다. `Array.includes()` 메서드는 대소문자를 구분한다.
두 번째 매개변수는 검색을 시작할 배열의 위치이며 이를 생략할 기본값 0이 적용된다.

---

### 2-2. 두 번째 매개변수가 양수인 경우

```javascript
const array = ["red", "blue", "green", "black"];
const result1 = array.includes("blue", 2);
const result2 = array.includes("green", 2);
```

![includes 2](/image/JS/ArrayMethod/ArrayIncludes/array_includes2.png)

두 번째 매개변수로 모두 2를 전달받았다. 그러면 인덱스가 2인 위치부터 첫 번쩨 매개변수로 주어진 값을 찾기 시작한다.
`blue`은 인덱스 번호가 1이기 때문에 찾는 대상에서 제외된다.

두 번째 매개변수가 배열의 길이보다 크다면 항상 `false`를 반환한다.

---

### 2-3. 두 번째 매개변수가 음수인 경우

```javascript
const array = ["red", "blue", "green", "black"];
const result1 = array.includes("blue", -1);
const result2 = array.includes("green", -3);
const result2 = array.includes("blue", -10);
```

![includes 3](/image/JS/ArrayMethod/ArrayIncludes/array_includes3.png)

두 번재 매개변수가 음수인 경우 `array.length + 두 번째 매개변수의 값`부터 첫 번째 매개변수로 주어진 값을 찾기 시작한다.
위의 `result1`은 인덱스가 3(4-3)인 위치부터 검색을 시작하고 `reulst2`은 인덱스가 1(4-3)인 위치부터 검색을 시작한다.

두 번째 매개변수가 음수이고 절대값이 배열의 길이보다 같거나 크다면 전체 배열이 검색된다.

---

## 3. Conclusion

> `Array.includes()`메서드는 코딩 테스트 문제를 풀 때 많이 사용하는 메서드이다. 하지만 두 번째 매개변수는 아직
> 한 번도 사용하지 않았다. 이후에 필요한 상황이 오면 잘 사용해봐야겠다.

---

## 참고

[MDN - Array.prototype.includes()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)

---

📅 2022-08-26
