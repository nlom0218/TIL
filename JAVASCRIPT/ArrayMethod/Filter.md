# 조건을 만족하는 요소로만 구성된 배열 만들기(filter)

## 1. 개요

`Array.filter()` 메서드는 배열의 요소를 순회하는 메서드이다. 해당 메서드를 사용하면 원하는 요소만 가지는 배열을
쉽게 만들 수 있어 웹 개발, 코딩 테스트에서 유용하게 사용된다.

---

## 2. Array.filter()

- **특징**
  - 주어진 콜백 함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열을 반환
  - 원본 배열은 변경되지 않음
  - 자신을 호출한 배열에서 필터링 조건을 만족하는 특정 요소만 출추하여 새로운 배열을 만들 때 사용
  - `break`, `continue`을 사용할 수 없음
- **반환 값**: 테스트를 통과한 요소로 이루어진 새로운 배열, 어떤 요소도 테스트를 통과하지 못했으면 빈 배열을 반환
- **구문**
  ```javascript
  arr.filter(callback(element[, index[, array]])[, thisArg])
  ```
- **매개변수**
  - `callback`: 각 요소에 대해 실행할 함수, 세 가지 매개변수를 받음, `true`를 반환하면 요소를 유지, `false`를 반환하면 요소를 버림
    - `currentvalue`: 처리할 현재 요소
    - `index`: 처리할 현재 요소의 인덱스
    - `array`: `forEach()`를 호출한 배열
  - `thisArg`: callback을 실행할 때 this로 사용할 값

몇가지의 예시를 통해 `Array.filter()`메서드의 사용법을 알아보자.

---

### 2-1. 특정 수보다 큰 요소만 걸러내기

```javascript
const nums = [1, 2, 3, 4, 5, 6];
const bigNums = nums.filter((item) => item > 3);
```

`nums` 배열의 요소 중 3보다 큰 요소로만 이루어진 새로운 배열을 만든다.

![filter 1](/image/JS/ArrayMethod/Filter/filter1.png)

---

### 2-2. 특정 문자열을 포함한 요소만 걸러내기

```javascript
const fruits = ["apple", "banana", "grapes", "mango", "orange"];
const fruits_ap = fruits.filter((item) => item.includes("ap"));
```

`fruits` 배열의 요소 중 문자열 "ap"을 포함한 요소로만 이루어진 새로운 배열을 만든다.

![filter 2](/image/JS/ArrayMethod/Filter/filter2.png)

---

### 2-3. 특정 유저가 작성한 글만 걸러내기

```javascript
const contents = [
  { userId: 1, content: "안녕하세요?" },
  { userId: 5, content: "뭐하세요?" },
  { userId: 2, content: "치킨을 먹고 싶어요" },
  { userId: 1, content: "코로나가 얼른 끝났으면 좋겠어" },
];
const user1_contents = contents.filter((item) => item.userId === 1);
```

위의 경우가 개인적으로 웹 개발을 진행하면서 자주 사용했던 코드이다.

![filter 3](/image/JS/ArrayMethod/Filter/filter3.png)

---

## 3. Conclusion

> `Array.filter()`를 맨 처음 공부했을 땐 이것 또한 잘 이해가 되지 않았었다. 하지만 계속 공부도 하고 프로젝트도
> 진행을 하다보니 생각보다. 해당 메서드를 사용할 일이 많아서 자연스럽게 이해가 되었고 언제 활용을 하면 좋은지도 체득한 것 같다.
> 역시 이론도 중요하지만 실전에서의 적용도 중요해 보인다. 아마 실전없이 계속 이론만 공부했으면 아직도 이해가 되지 않았겠지?

---

[👆](#조건을-만족하는-요소로만-구성된-배열-만들기filter)

📅 2022-08-18
