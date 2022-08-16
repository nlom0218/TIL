# 원하는 인덱스 찾기(indexOf, lastIndexOf, findIndex)

## 1. 개요

배열에는 인덱스와 요소가 있다. 배열을 구성하는 각각의 값들이 요소이고 배열에서 위치를 가리키는 숫자가 인덱스이다. 즉, 배열의 요소는 각각의 인덱스를 가지고 있다. 이번 챕터에서는 특정 요소가 몇 번째 인덱스에 위치하는지 알 수 있는 메서드에 대해 살펴보자.

---

## 2. Array.indexOf()

- **특징**: 배열에 특정 요소가 존재하는지 확인할 때 유용하고 몇 번째 인덱스에 있는지 알 수 있음
- **반환 값**: 원본 배열에 매개변수로 전달한 요소와 중복되는 요소가 여러 개 있다면 첫 번째로 검색된 요소의 인덱스를 반환, 요소가 없으면 -1을 반환
- **구문**
  ```javascript
  arr.indexOf(searchElement[, fromIndex])
  ```
- **매개변수**
  - `searchElement`: 배열에서 찾을 요소
  - `fromIndex`(Optional): 검색을 시작할 인덱스, 생략하면 처음부터 검색

예시를 통해 `Array.indexOf()`메서드의 사용법을 알아보자.

```javascript
const color = ["black", "red", "blue", "green", "blue"];
const index1 = color.indexOf("red");
const index2 = color.indexOf("blue");
const index3 = color.indexOf("blue", 3);
const index4 = color.indexOf("yellow");
```

![indexOf_1](/image/JS/ArrayMethod/FindIndex/find_index_indexOf_1.png)

만약 특정 요소가 위치하는 모든 인덱스를 찾고 싶다면 아래와 같은 과정을 통해 찾을 수 있다.

```javascript
const color = ["black", "blue", "red", "blue", "green", "blue"];
const blueIndex = [];
let index = color.indexOf("blue");
while (index !== -1) {
  blueIndex.push(index);
  index = color.indexOf("blue", index + 1);
}
```

![indexOf_2](/image/JS/ArrayMethod/FindIndex/find_index_indexOf_2.png)

---

## 3. Array.lastIndexOf()

- **특징**: 쉽게 생각하면 `Array.indexOf()` 메서드와 같지만 역순으로 검색
- **반환 값**: 주어진 값과 일치하는 마지막 요소의 인덱스, 요소가 없으면 -1을 반환
- **구문**
  ```javascript
  arr.lastIndexOf(searchElement[, fromIndex])
  ```
- **매개변수**
  - `searchElement`: 배열에서 찾을 요소
  - `fromIndex`(Optional): 역순으로 검색을 시작할 인덱스, 생략하면 맨 뒤 부터 검색

예시를 통해 `Array.indexOf()`메서드의 사용법을 알아보자.

```javascript
const color = ["black", "red", "blue", "green", "blue"];
const indexOf = color.indexOf("blue");
const lastIndexOf = color.lastIndexOf("blue");
```

![lastIndexOf](/image/JS/ArrayMethod/FindIndex/find_index_lastIndexOf.png)

---

## 4. Array.findIndex()

- **특징**: 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소의 인덱스를 반환
- **반환 값**: 요소가 테스트를 통과하면 배열의 인덱스 그렇지 않으면 -1를 반환
- **구문**
  ```javascript
  arr.findIndex(callback(element[, index[, array]])[, thisArg])
  ```
- **매개변수**
  - `callback`: 3개의 인수를 가지며 배열의 각 요소에 대해 실행할 함수
    - `element`: 배열에서 처리중인 현재 요소
    - `index`: 배열에서 처리중인 현재 요소의 인덱스
    - `array`: findIndex() 메서드가 호출된 배열
  - `thisArg`(Optional): 콜백을 실행할 때 this로 사용할 객체

예시를 통해 `Array.findIndex()`메서드의 사용법을 알아보자.

```javascript
const number = [12, 6, 8, 29, 99, 13, 31, 2, 3, 5, 21];
const index1 = number.findIndex((item) => item > 30);
const index2 = number.findIndex((item) => item < 5);
const index3 = number.findIndex((item) => item > 100);
```

![findIndex](/image/JS/ArrayMethod/FindIndex/find_index_findIndex.png)

---

## 5. Conclusion

> 오늘 정리한 세 개의 메서드는 모두 이해하는 데는 큰 어려움이 없었다. 다만 `Array.indexOf()` 메서드에서 `특정 요소가 위치하는 모든 인덱스를 찾는 과정`은 조금 이해가 필요했다. 하지만 엄청 어려운 내용은 아니었기에 금방 이해했다. 이런 내용은 언젠가 한 번쯤 코딩 테스트에서 나올 것 같기도 하다. 오늘 공부한 내용이 언제가는 큰 도움이 되길 바라며 마무리 한다.

---

## 참고

[MDN - Array.prototype.indexOf()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)  
[MDN - Array.prototype.lastIndexOf()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/lastIndexOf)  
[MDN - Array.prototype.findIndex()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)

---

[👆](#원하는-인덱스-찾기indexof-lastindexof-findindex)

📅 2022-08-16
