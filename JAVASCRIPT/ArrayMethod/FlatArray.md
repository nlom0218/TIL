# 중첩된 배열 구조 평탄화하기(flat, flatMap)

## 1. 개요

ES10(ECMAScript 2019)에서 도입된 flat 메서드와 flatMap 메서드는 중첩 배열을 평탄화하는 역할을 한다.
이번 챕터에서는 해당 메서드에 대해 정리해 본다. 본격적인 내용에 들어가기 앞서 중첩 배열이란 배열안에 또 다른 배열이
속해 있는 것이다. 예를 들어 아래와 같은 배열을 중첩 배열이라고 할 수 있다.

```javascript
const arr1 = ["a", "b", ["c"]];
const arr2 = ["a", "b", ["c", ["d"]]];
const arr3 = ["a", "b", ["c", ["d"]], "e"];
```

---

## 2. Array.flat()

- **특징**
  - 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화
- **반환 값**: 하위 배열을 이어붙인 새로운 배열
- **구문**
  ```javascript
  const newArr = arr.flat([depth]);
  ```
- **매개변수**
  - `depth`
    - 중첩 배열 구조를 평탄화할 때 사용할 깊이 값
    - 기본값은 1
    - 인수로 Infinity를 전달하면 중첩 배열 모두를 평탄화

몇가지의 예시를 통해 `Array.flat()`메서드의 사용법을 알아보자.

---

### 2-1. 중첩 배열 평탄화

- 중첩 배열을 평탄화하기 위한 깊이 값의 기본값은 1이다.

  ```javascript
  const arr = [1, [2, [3, [4]]]];
  const flat1 = arr.flat();
  const flat2 = arr.flat(1);
  ```

  ![flat 1](/image/JS/ArrayMethod/FlatArray/flat1.png)

- 중첩 배열을 평탄화하기 위한 깊이 값을 2, 3으로 지정하여 2단계, 3단계 깊이까지 평탄화한다.

  ```javascript
  const arr = [1, [2, [3, [4]]]];
  const flat1 = arr.flat(2);
  const flat2 = arr.flat(3);
  ```

  ![flat 2](/image/JS/ArrayMethod/FlatArray/flat2.png)

- 중첩 배열을 평탄화하기 위한 깊이 값을 Infinity로 지정하여 중첩 배열을 모두 평탄화한다.

  ```javascript
  const arr = [1, [2, [3, [4]]], "a", ["b", ["c", "d", ["e"]]]];
  const flat = arr.flat(Infinity);
  ```

  ![flat 3](/image/JS/ArrayMethod/FlatArray/flat3.png)

---

### 2-2. 배열 구멍 제거

`Array.flat()` 메서드는 배열의 구멍을 제거한다.

```javascript
const arr = [1, 2, 3, , [5]];
const flat = arr.flat();
```

![flat 4](/image/JS/ArrayMethod/FlatArray/flat4.png)

---

## 3. Array.flatMap()

- **특징**
  - `Array.map()` 메서드를 통해 생성된 새로운 배열을 평탄화
  - 즉, 먼저 각 엘리먼트에 대해 map 수행 후, 결과를 새로운 배열로 평탄화
  - `Array.flat()` 메서드처럼 인수를 전달하여 평탄화 깊이를 지정할 수 없음
  - 1단계 평탄화만 가능
  - 깊이를 지정하기 위해서는 `Array.map()` 메서드와 `Array.flat()` 메서드를 각각 호출해야 함
- **반환 값**: 각 엘리먼트가 callback 함수의 결과이고, 깊이 1로 평탄화된 새로운 배열
- **구문**
  ```javascript
   arr.flatMap(callback(currentValue[, index[, array]])[, thisArg])
  ```
- **매개변수**
  - `callback`: 새로운 배열의 앨리먼트를 생성하는 함수
    - `currentValue`: 배열에서 처리되는 현재 앨리먼트
    - `index`: 배열에서 처리되고 있는 현재 엘리먼트의 인덱스
    - `array`: `map`이 호출된 배열
  - `thisArg`: callback 실행에서 this로 사용할 값

몇가지의 예시를 통해 `Array.flatMap()`메서드의 사용법을 알아보자.

---

### 3-1. 문장으로 부터 단어의 리스트를 생성

```javascript
const arr = ["저는 휼륭한 개발자가 되고 싶은", "HD입니다. 잘 부탁드립니다!"];
const word = arr.flatMap((item) => item.split(" "));
```

![flatMap 1](/image/JS/ArrayMethod/FlatArray/flatMap1.png)

만약 `Array.flatMap` 메서드가 아닌 `Array.map()` 메서드만 사용하면 결과는 아래와 같다.

![flatMap 2 - use map method](/image/JS/ArrayMethod/FlatArray/flatMap2.png)

---

### 3-2. 아이템 추가 및 제거

아래의 예시는 다음과 같은 과정을 거칩니다.

1. 기념품의 금액이 5만원 이상이면 기념품 품목에서 제거합니다.
2. 기념품의 금액이 2만원 이상이면 2만원에서 초과된 금액을 함께 표시합니다.
3. 기념품의 금액이 2만원 아래이면 그대로 다시 반환합니다.

```javascript
const arr = [
  ["컵", 6000],
  ["고등어", 40000],
  ["한라봉", 25000],
  ["향수", 80000],
];

arr.flatMap(([item, price]) =>
  price > 50000
    ? []
    : price > 20000
    ? [[item, price, price - 20000]]
    : [[item, price]]
);
```

![flatMap 3](/image/JS/ArrayMethod/FlatArray/flatMap3.png)

`Array.map()` 메서드와 `Array.filter()` 메서드를 사용하여 나타낼 수 도 있다.(아래 코드 참고)

```javascript
arr
  .map(([item, price]) =>
    price > 50000
      ? []
      : price > 20000
      ? [item, price, price - 20000]
      : [item, price]
  )
  .filter((item) => item.length !== 0);
```

---

## 4. Conclusion

> `Array.flat()` 메서드와 `Array.flatMap()` 메서드는 처음으로 공부한 메서드이다. 그래서 아직
> 어떤 상황에서 사용해야 할지 감이 잘 잡히지 않는다. 다만 `Array.flatMap()`을 사용해 자료를 효율적으로
> 정리할 수 있다는 느낌을 받았다. 요소의 제거 뿐 아니라 수정, 추가도 가능하니 `Array.map()` 메서드와
> `Array.filter()` 메서드를 함께 사용하는 느낌이 든다. 물론 아직 중첩을 잘 활용하진 못하지만 앞으로
> 많은 코드를 보면서 이번 챕터에서 정리한 두 개의 메서드는 적절히 그리고 적극적으로 활용해 봐야겠다. 시도할 수 있는
> 방법이 하나더 늘어난 것 같다.👍

---

## 출처

도서 - 모던 자바스크립트 Deep Dive: 자바스크립트의 기본 개념과 동작 원리  
[MDN - Array.prototype.flat()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)  
[MDN - Array.prototype.flatMap()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap)

---

📅 2022-08-29
