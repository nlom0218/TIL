# 숫자 짝꿍

## 1. 개요

- 프로그래머스
- Lv.1
- 연습문제
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/131128)

---

## 2. 문제 설명

두 정수 X, Y의 임의의 자리에서 공통으로 나타나는 정수 k(0 ≤ k ≤ 9)들을 이용하여 만들 수 있는 가장 큰 정수를 두 수의 짝꿍이라 합니다(단, 공통으로 나타나는 정수 중 서로 짝지을 수 있는 숫자만 사용합니다). X, Y의 짝꿍이 존재하지 않으면, 짝꿍은 -1입니다. X, Y의 짝꿍이 0으로만 구성되어 있다면, 짝꿍은 0입니다.

예를 들어, X = 3403이고 Y = 13203이라면, X와 Y의 짝꿍은 X와 Y에서 공통으로 나타나는 3, 0, 3으로 만들 수 있는 가장 큰 정수인 330입니다. 다른 예시로 X = 5525이고 Y = 1255이면 X와 Y의 짝꿍은 X와 Y에서 공통으로 나타나는 2, 5, 5로 만들 수 있는 가장 큰 정수인 552입니다(X에는 5가 3개, Y에는 5가 2개 나타나므로 남는 5 한 개는 짝 지을 수 없습니다.)
두 정수 X, Y가 주어졌을 때, X, Y의 짝꿍을 return하는 solution 함수를 완성해주세요.

### 2-1. 문제 설명 - 제한사항

- 3 ≤ X, Y의 길이(자릿수) ≤ 3,000,000입니다.
- X, Y는 0으로 시작하지 않습니다.
- X, Y의 짝꿍은 상당히 큰 정수일 수 있으므로, 문자열로 반환합니다.

### 2-2. 문제 설명 - 입출력 예

| X       | Y        | result |
| ------- | -------- | ------ |
| "100"   | "2345"   | "-1"   |
| "100"   | "203045" | "0"    |
| "100"   | "123450" | "10"   |
| "12321" | "42531"  | "321"  |
| "5525"  | "1255"   | "552"  |

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1

- X, Y의 짝꿍은 존재하지 않습니다. 따라서 "-1"을 return합니다.

입출력 예 #2

- X, Y의 공통된 숫자는 0으로만 구성되어 있기 때문에, 두 수의 짝꿍은 정수 0입니다. 따라서 "0"을 return합니다.

입출력 예 #3

- X, Y의 짝꿍은 10이므로, "10"을 return합니다.

입출력 예 #4

- X, Y의 짝꿍은 321입니다. 따라서 "321"을 return합니다.

입출력 예 #5

- 지문에 설명된 예시와 같습니다.

---

## 3. 문제 풀이

```javascript
function solution(X, Y) {
  // 1) X에 포함된 숫자들을 나누기
  const numberMap = new Map();
  X.split("").forEach((num) => {
    numberMap.set(num, numberMap.has(num) ? numberMap.get(num) + 1 : 1);
  });

  // 2) X, Y에 공통적으로 포함된 숫자 구하기
  let commonNumber = [];
  Y.split("").forEach((num) => {
    if (!numberMap.has(num) || numberMap.get(num) === 0) return;
    numberMap.set(num, numberMap.get(num) - 1);
    commonNumber.push(num);
  });

  // 3) 0만 공통적으로 있다면 "0"를 반환하기
  if ((new Set(commonNumber).size === 1) & new Set(commonNumber).has("0"))
    return "0";

  // 4) 공통으로 포함된 숫자가 없다면 "-1" 반환하기
  if (commonNumber.length === 0) return "-1";

  // 5) 내림차순으로 정렬한 후 문자열로 바꾸어 반환하기
  commonNumber.sort((a, b) => b - a);

  return commonNumber.join("");
}
```

### 1) X에 포함된 숫자들을 나누기

```javascript
const numberMap = new Map();
X.split("").forEach((num) => {
  numberMap.set(num, numberMap.has(num) ? numberMap.get(num) + 1 : 1);
});
```

`map` 객체를 활용하여 `X`에 포함된 숫자들을 나눈다. `map` 객체의 `key`는 숫자이고 `value`는 해당 숫자가 포함된 개수이다.

### 2) X, Y에 공통적으로 포함된 숫자 구하기

```javascript
let commonNumber = [];
Y.split("").forEach((num) => {
  if (!numberMap.has(num) || numberMap.get(num) === 0) return;
  numberMap.set(num, numberMap.get(num) - 1);
  commonNumber.push(num);
});
```

`X`와 `Y`에 공통적으로 포함된 숫자를 `commonNumber` 배열에 담는과정이다.

`Y`에 대해 반복문을 실시하며 `Y`의 어느 한 숫자가 `numberMap`에 `key`로 존재하지 않거나 `value`가 0이라면 다음 반복문으로 넘어간다.
만약 그렇지 않다면 `value`를 1를 빼고 `commonNumber`에 해당 숫자를 추가한다.

1를 빼는 과정과 `value`가 0일 때를 생각하는 이유는 `Y`에 중복되는 특정 숫자의 개수가 더 많을 수도 있기 때문이다.

예를들어 아래와 같은 두 개의 정수가 있다고 생각해보자.

- X: "1223"
- Y: "2223"

"2"는 두 번이 겹쳐 `commonNumber` 배열에 두 번 들어가야 한다. 하지만 단순히 `numberMap`에 "2"의 유무만 파악을 하게 된다면 "2"는 세 번들어가게 된다.

### 3) 0만 공통적으로 있다면 "0"를 반환하기

```javascript
if ((new Set(commonNumber).size === 1) & new Set(commonNumber).has("0"))
  return "0";
```

위의 과정은 "00", "000"과 같은 결과를 반환하는 것을 방지하기 위한 것이다.

`set` 객체를 이용하여 중복되는 숫자를 제거하여 길이가 1이고 해당 요소가 "0"인 경우에는 "0"를 반환한다.

### 4) 공통으로 포함된 숫자가 없다면 "-1" 반환하기

```javascript
if (commonNumber.length === 0) return "-1";
```

`commonNumber`배열의 길이가 0이라는 것은 공통으로 포함된 숫자가 없다는 것이기 때문에 "-1"를 반환해야 한다.

### 5) 내림차순으로 정렬한 후 문자열로 바꾸어 반환하기

```javascript
commonNumber.sort((a, b) => b - a);

return commonNumber.join("");
```

가장 큰 정수를 만들어야 하기 때문에 `commonNumber`배열을 내림차순으로 정렬을 해야 한다. 그 후 `join()` 메서드를 사용하여 문자열로 바꿔 반환한다.

### 결과

![programmers_number_partner_result](/image/CodingTest/programmers_number_partner/programmers_number_partner_result.png)

테스트 11 ~ 15는 시간이 많이 걸렸다.

---

## 4. 다른 사람 풀이

```javascript
function solution(X, Y) {
  let result = "";
  const numObj = {};

  for (const char of X) {
    numObj[char] = (numObj[char] || 0) + 1;
  }

  for (const char of Y) {
    if (!numObj[char]) continue;
    result += char;
    numObj[char]--;
  }

  if (result === "") return "-1";
  if (+result === 0) return "0";
  return [...result]
    .map((num) => +num)
    .sort((a, b) => b - a)
    .join("");
}
```

`map`이 아닌 일반 객체를 활용하여 문제를 해결한 다른 사람의 풀이이다. 개인적으로 `map` 객체를 다루는 것이 편해서 많이 활용하고 있지만 객체를 활용하는 것을 보니 더욱 깔끔한 것이 눈에 띈다.

배울 점은 `0`이 `Boolean`으로 사용되면 `false`가 되는 것과 마지막 반환하는 값을 위해 여러 메서드를 사용했다는 것이다. 또한 `result`를 숫자로 바꾸어 `0`과 비교하는 것도 배울 점이다.

나의 코드를 간단히 리팩터링을 한다면 아래와 같이 할 수 있다.

```javascript
// 전
// 1. Boolean(0) -> false
if (!numberMap.has(num) || numberMap.get(num) === 0) return;
// 2. Number
if ((new Set(commonNumber).size === 1) & new Set(commonNumber).has("0"))
  return "0";

// 후
// 1. Boolean(0) -> false
if (!numberMap.get(num)) return;
// 2. Number
if (Number(commonNumber.join("")) === 0) return "0";
```

---

## 5. Conclusion

> 지금까지 계속 쉬운 문제만 풀다가 오랜만에 한 번에 성공하지 못하는 문제를 풀었다. 돌이켜 생각해보면 이번 문제 또한 그리 어렵지는 않았지만 예외를 놓치면 그대로 실패를 하는 것이었기 때문에 꼼꼼하게만 봤어도 더욱 빠르고 정확하게 풀 수 있었을 것이다. 구현하는 것 자체는 어렵지 않았다. 복잡한 알고리즘, 자료구조가 필요없는 단순 구현 문제였기 때문이라고 생각한다. Lv1을 얼른 정복해자.

---

📅 2022-01-11
