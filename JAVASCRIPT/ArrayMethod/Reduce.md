# 배열의 요소를 가지고 하나의 결과값을 만들어 반환하기(reduce)

## 1. 개요

배열의 요소를 가지고 하나의 결과값을 만들어 하는 경우가 있을 수 있다. 예를 들어 배열의 모든 요소의 합, 곱 등
수학과 관련한 연산이 대표적이다. 이럴 경우 `Array.forEach()` 메서드, `for` 문을 사용할 수 있지만,
이번 챕터에서 다루게 될 `Array.reduce()` 메서드를 사용하면 보다 편리하게 구할 수 있다.

---

## 2. Array.reduce()

- **특징**
  - 자신을 호출한 배열의 모든 요소를 순회하며 인수로 전달받은 콜백 함수를 반복 호출
  - 콜백 함수의 반환값을 다음 순회 시에 콜백 함수의 첫 번째 인수로 전달
  - 하나의 결과값을 만들어 반환
  - 원본 배열은 변경되지 않음
- **반환 값**: 누적 계산의 결과 값을 반환
- **구문**
  ```javascript
  arr.filter(reduce(acc, cur[, inx[, src]])[, initialValue])
  ```
- **매개변수**
  - `callback`: 배열의 각 요소에 대해 실행할 함수, 리듀서 함수로 4개의 인수를 받음(아래참고)
    - `acc`: 누적된 콜백 함수의 반환값
    - `cur`: 처리할 현재 요소
    - `inx`: 처리할 현재 요소의 인덱스. `initialValue`를 제공한 경우 0, 아니면 1부터 시작
    - `src`: `reduce()`를 호출한 배열
  - `initialValue`: 콜백 함수의 최초 호출에서 첫 번째 인수에 제공하는 값. 초기값을 제공하지 않으면 배열의 첫 번째 요소를 사용

몇가지의 예시를 통해 `Array.reduce()`메서드의 사용법을 알아보자.

---

### 2-1. 배열의 합 구하기

```javascript
const num = [1, 4, 5, 7, 12];
const sum = num.reduce((acc, cur) => acc + cur);
```

- `initialValue` 을 제공하지 않아 첫 번째 호출 때 `acc`는 `num` 배열의 첫 번째 요소, `cur`는 `num` 배열의 두 번째 요소이다.

| 순서 | acc | cur | 반환값 |
| ---- | --- | --- | ------ |
| 1    | 1   | 4   | 5      |
| 2    | 5   | 5   | 10     |
| 3    | 10  | 7   | 17     |
| 4    | 17  | 12  | 29     |

![reduce 1](/image/JS/ArrayMethod/Reduce/reduce1.png)

---

### 2-2. 평균 구하기

```javascript
const num = [1, 4, 5, 7, 12];
const average = num.reduce((acc, cur, index, { length }) => {
  return index === length - 1 ? (acc + cur) / length : acc + cur;
});
```

- `initialValue` 을 제공하지 않아 첫 번째 호출 때 `acc`는 `num` 배열의 첫 번째 요소, `cur`는 `num` 배열의 두 번째 요소이다.
- 콜백함수의 4번재 인수를 위와 같이 가져오면 `reduce()`를 호출한 배열의 길이를 가져온다.

| 순서 | acc | cur | 반환값 |
| ---- | --- | --- | ------ |
| 1    | 1   | 4   | 5      |
| 2    | 5   | 5   | 10     |
| 3    | 10  | 7   | 17     |
| 4    | 17  | 12  | 5.8    |

![reduce 2](/image/JS/ArrayMethod/Reduce/reduce2.png)

---

### 2-3. 최대값 / 최소값 구하기

```javascript
const num = [3, 4, 9, 7, 2, 6];
const max = num.reduce((acc, cur) => {
  return acc > cur ? acc : cur;
}, 0);
const min = num.reduce((acc, cur) => {
  return acc > cur ? cur : acc;
});
```

- 최대값
  | 순서 | acc | cur | 반환값 |
  | ---- | --- | --- | ------ |
  | 1 | 0 | 3 | 3 |
  | 2 | 3 | 4 | 4 |
  | 3 | 4 | 9 | 9 |
  | 4 | 9 | 7 | 9 |
  | 5 | 9 | 2 | 9 |
  | 6 | 9 | 6 | 9 |
- 최소값
  | 순서 | acc | cur | 반환값 |
  | ---- | --- | --- | ------ |
  | 1 | 3 | 4 | 3 |
  | 2 | 3 | 9 | 3 |
  | 3 | 3 | 7 | 3 |
  | 4 | 3 | 2 | 2 |
  | 5 | 2 | 6 | 2 |

![reducd 3](/image/JS/ArrayMethod/Reduce/reduce3.png)

---

### 2-4. 요소의 중복 횟수 구하기

```javascript
const color = ["black", "blue", "blue", "red", "black", "red", "red", "green"];
const count = color.reduce((acc, cur) => {
  acc[cur] = (acc[cur] | 0) + 1;
  return acc;
}, {});
```

| 순서 | acc                         | cur   | 반환값                                |
| ---- | --------------------------- | ----- | ------------------------------------- |
| 1    | {}                          | black | {black: 1}                            |
| 2    | {black: 1}                  | blue  | {black: 1, blue: 1}                   |
| 3    | {black: 1, blue: 1}         | blue  | {black: 1, blue: 2}                   |
| 4    | {black: 1, blue: 2}         | red   | {black: 1, blue: 2, red: 1}           |
| 5    | {black: 1, blue: 2, red: 1} | black | {black: 2, blue: 2, red: 1}           |
| 6    | {black: 2, blue: 2, red: 1} | red   | {black: 1, blue: 2, red: 2}           |
| 7    | {black: 1, blue: 2, red: 2} | red   | {black: 1, blue: 2, red: 3}           |
| 8    | {black: 1, blue: 2, red: 3} | green | {black: 1, blue: 2, red: 3, green: 1} |

![reduce 4](/image/JS/ArrayMethod/Reduce/reudce4.png)

위의 과정은 맵(Map) 객체를 사용하여 구현할 수 있다.

```javascript
const color = ["black", "blue", "blue", "red", "black", "red", "red", "green"];
const count = new Map();
color.forEach((item) => {
  count.set(item, (count.get(item) | 0) + 1);
});
```

![reduce 5 - map](/image/JS/ArrayMethod/Reduce/reduce5%20-%20map.png)

---

### 2-5. 중복 요소 제거하기

```javascript
const color = ["black", "blue", "blue", "red", "black", "red", "red", "green"];
const result = color.reduce((acc, cur) => {
  if (!acc.includes(cur)) acc.push(cur);
  //   if (acc.indexOf(cur) === -1) acc.push(cur);
  return acc;
}, []);
```

- 이전 값에 현재 요소가 없으면 포함시킨다.
- `!acc.includes(cur)`는 `acc.indexOf(cur) === -1` 로 바꾸어 줄 수 있다.

| 순서 | acc                | cur   | 반환값                    |
| ---- | ------------------ | ----- | ------------------------- |
| 1    | []                 | black | [black]                   |
| 2    | [black]            | blue  | [black, blue]             |
| 3    | [black, blue]      | blue  | [black, blue]             |
| 4    | [black, blue]      | red   | [black, blue, red]        |
| 5    | [black, blue, red] | black | [black, blue, red]        |
| 6    | [black, blue, red] | red   | [black, blue, red]        |
| 7    | [black, blue, red] | red   | [black, blue, red]        |
| 8    | [black, blue, red] | green | [black, blue, red, green] |

![reduce 6](/image/JS/ArrayMethod/Reduce/reduce6.png)

위의 과정은 셋(Set) 객체를 사용하여 구현할 수 있다.

```javascript
const color = ["black", "blue", "blue", "red", "black", "red", "red", "green"];
const result = [...new Set(color)];
```

![reduce 7 - set](/image/JS/ArrayMethod/Reduce/reduce7%20-%20set.png)

---

## 3. Conclusion

> `Array.reduce()` 메서드는 얼추 사용법만 알고 있었지만 많이 사용하지는 않았었다. 최근 코딩 테스트 공부를
> 하면서 오히려 사용 빈도가 늘어난 경우이다. 단순히 배열 요소의 합을 구할 때 많이 사용했는데 해당 사용 뿐 아니라
> 다양한 상황에서 적재적소 사용할 수 있으니 생각을 많이 하며 코딩 테스트 문제를 풀어봐야겠다.

---

## 참고

[MDN - Array.prototype.reduce()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

---

[👆](#배열의-요소를-가지고-하나의-결과값을-만들어-반환하기reduce)

📅 2022-08-18
