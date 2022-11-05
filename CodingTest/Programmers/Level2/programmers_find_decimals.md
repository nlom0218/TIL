# k진수에서 소수 개수 구하기

## 1. 개요

- 프로그래머스
- Lv.2
- 2022 KAKAO BLIND RECRUITMENT
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/92335)

---

## 2. 문제 설명

양의 정수 `n`이 주어집니다. 이 숫자를 k진수로 바꿨을 때, 변환된 수 안에 아래 조건에 맞는 소수(Prime number)가 몇 개인지 알아보려 합니다.

- 0P0처럼 소수 양쪽에 0이 있는 경우
- P0처럼 소수 오른쪽에만 0이 있고 왼쪽에는 아무것도 없는 경우
- 0P처럼 소수 왼쪽에만 0이 있고 오른쪽에는 아무것도 없는 경우
- P처럼 소수 양쪽에 아무것도 없는 경우
- 단, P는 각 자릿수에 0을 포함하지 않는 소수입니다.
  - 예를 들어, 101은 P가 될 수 없습니다.

예를 들어, 437674을 3진수로 바꾸면 211020101011입니다. 여기서 찾을 수 있는 조건에 맞는 소수는 왼쪽부터 순서대로 211, 2, 11이 있으며, 총 3개입니다. (211, 2, 11을 k진법으로 보았을 때가 아닌, 10진법으로 보았을 때 소수여야 한다는 점에 주의합니다.) 211은 P0 형태에서 찾을 수 있으며, 2는 0P0에서, 11은 0P에서 찾을 수 있습니다.

정수 n과 k가 매개변수로 주어집니다. n을 k진수로 바꿨을 때, 변환된 수 안에서 찾을 수 있는 위 조건에 맞는 소수의 개수를 return 하도록 solution 함수를 완성해 주세요.

---

### 2-1. 제한사항

- 1 ≤ n ≤ 1,000,000
- 3 ≤ k ≤ 10

---

### 2-2. 입출력 예

| n      | k   | result |
| ------ | --- | ------ |
| 437674 | 3   | 3      |
| 110011 | 10  | 2      |

---

### 2-3. 입출력 예 설명

입출력 예 #1

문제 예시와 같습니다.

입출력 예 #2

110011을 10진수로 바꾸면 110011입니다. 여기서 찾을 수 있는 조건에 맞는 소수는 11, 11 2개입니다. 이와 같이, 중복되는 소수를 발견하더라도 모두 따로 세어야 합니다.

---

## 3. 문제 풀이

```javascript
// 1) 매개변수로 받은 숫자가 소수인지 판별하는 함수 만들기
function isPrime(n) {
  if (n === "1") return false;

  if (n % 2 === 0) {
    return n === "2" ? true : false;
  }

  const sqrt = parseInt(Math.sqrt(n));

  for (let divider = 3; divider <= sqrt; divider += 2) {
    if (n % divider === 0) {
      return false;
    }
  }

  return true;
}

// 2) 0를 포함하지 않고 k진수가 소수인지 판별하기
function isP(n) {
  let result = false;
  const isContainZero = n.includes("0");
  if (isPrime(n) && !isContainZero) result = true;

  return result;
}

// 3) k진수에서 0이 위치한 자리를 배열로 만들기
function findZeroIdx(n) {
  let idxArr = [];

  for (let idx = 0; idx < n.length; idx++) {
    if (n[idx] === "0") idxArr.push(idx);
  }

  return idxArr;
}

// 4) P0, 0P의 경우를 가지는 소수 개수 구하기
function countP0Or0P(n) {
  let primeP0 = [];
  let prime0P = [];
  findZeroIdx(n).forEach((idx) => {
    const left = n.slice(0, idx);
    const right = n.slice(idx + 1);
    if (isPrime(left)) primeP0.push(left);
    if (isPrime(right)) prime0P.push(right);
  });

  primeP0 = primeP0.map((num) => isPrime(num));
  prime0P = prime0P.map((num) => isPrime(num));

  return [...new Set([...primeP0])].length + [...new Set([...prime0P])].length;
}

// 5) 양쪽에 0이 있는 경우의 소수 개수 구하기
function count0P0(n) {
  const zeroIdx = findZeroIdx(n);

  if (zeroIdx < 2) return 0;

  let number = [];
  zeroIdx.forEach((item, idx) => {
    if (idx === zeroIdx.length - 1) return;
    number.push(n.slice(item + 1, zeroIdx[idx + 1]));
  });

  number = number.filter((num) => isPrime(num) && num !== "");

  return number.length;
}

function solution(n, k) {
  n = n.toString(k);

  let count = 0;
  if (isP(n)) count++;

  count += countP0Or0P(n);
  count += count0P0(n);

  return count;
}
```

이번 문제는 필요한 함수를 생각하여 이를 만들어 풀어보았다. 필요한 함수는 아래와 같다.

- 소수를 판별하는 함수
- k진수에서 0이 위치한 자리를 요소로 가지는 배열을 만드는 함수
- 0를 포함하지 않고 k진수가 소수인지 판별하는 함수 만들기
- 양쪽에 0이 있는 경우의 소수 개수를 구하는 함수 만들기
- P0, 0P의 경우를 가지는 소수 개수를 구하는 함수 만들기

---

### 1) 매개변수로 받은 숫자가 소수인지 판별하는 함수 만들기

```javascript
function isPrime(n) {
  if (n === "1") return false;

  if (n % 2 === 0) {
    return n === "2" ? true : false;
  }

  const sqrt = parseInt(Math.sqrt(n));

  for (let divider = 3; divider <= sqrt; divider += 2) {
    if (n % divider === 0) {
      return false;
    }
  }

  return true;
}
```

위의 과정을 통해 매개변수로 받은 `n`이 소수인지 아닌지 판별을 한다. 먼저 `1`인 경우는 소수가 아니므로 `false`를 반환하고 `n`이 2로 나누어지면 `false`를 반환한다. 이때 주의해야 할 점은 `2`는 짝수 중 유일한 소수이라는 것이다.

이제 남은 것은 홀수일 때 소수인지 아닌지를 판별하는 것이다. 이를 반복문을 통해 구현을 하였다. 먼저 첫 시작을 3으로 한다. 그리고 매개변수로 받은 `n`의 제곱근 보다 커지기 전까지 반복을 하고 반복을 할 때 마다 `divider`는 2씩 커진다.(짝수는 제외하기 때문)

---

### 2) 0를 포함하지 않고 k진수가 소수인지 판별하기

```javascript
function isP(n) {
  let result = false;
  const isContainZero = n.includes("0");
  if (isPrime(n) && !isContainZero) result = true;

  return result;
}
```

문제 설명에 나온 `P`의 조건은 `0`를 포함해서는 안된다. 그렇기 때문에 k진수로 변환한 `n`이 `0`를 포함하는지 검사를 해야한다. `0`이 포함되어 있지 않고 소수라면 `true`를 반환한다.

---

### 3) k진수에서 0이 위치한 자리를 배열로 만들기

```javascript
function findZeroIdx(n) {
  let idxArr = [];

  for (let idx = 0; idx < n.length; idx++) {
    if (n[idx] === "0") idxArr.push(idx);
  }

  return idxArr;
}
```

`P0`, `0P`, `0P0`는 모두 `0`의 위치를 알아야 한다. 그렇기 때문에 `k진수`에서 0이 위치한 자리를 배열로 만들어 이를 활용한다.

위의 함수에서 `idxArr`에는 `0`이 위치한 인덱스 번호가 요소로 들어있다.

---

### 4) P0, 0P의 경우를 가지는 소수 개수 구하기

```javascript
function countP0Or0P(n) {
  let primeP0 = [];
  let prime0P = [];
  findZeroIdx(n).forEach((idx) => {
    const left = n.slice(0, idx);
    const right = n.slice(idx + 1);
    if (isPrime(left)) primeP0.push(left);
    if (isPrime(right)) prime0P.push(right);
  });

  primeP0 = primeP0.map((num) => isPrime(num));
  prime0P = prime0P.map((num) => isPrime(num));

  return [...new Set([...primeP0])].length + [...new Set([...prime0P])].length;
}
```

위의 함수에서는 `P0`, `0P`의 경우를 가지는 소수를 찾는다. 중복할 수 있는 값이 있을 수 있으므로 `Set()` 객체를 사용하여 중복된 값을 제거한다.

중복이 발생할 수 있다고 생각한 이유는 아래와 같다.

하지만 스터디를 진행하면서 중복되는 값은 발생할 수 없다는 것을 알게 되었고 위의 코드를 아래와 같이 수정하였다.

```javascript
function countP0Or0P(n) {
  const zeroIdx = findZeroIdx(n);
  if (zeroIdx.length === 0) return 0;

  let primeP0 = n.slice(0, zeroIdx[0]);
  let prime0P = n.slice(zeroIdx[zeroIdx.length - 1] + 1);

  let count = 0;
  if (isPrime(primeP0)) count++;
  if (isPrime(prime0P)) count++;

  return count;
}
```

주의해야 할 점은 `findZeroIdx()`의 반환값인 배열의 길이가 `0`일 땐 `0`를 반환해야 한다는 것이다.

---

### 5) 양쪽에 0이 있는 경우의 소수 개수 구하기

```javascript
function count0P0(n) {
  const zeroIdx = findZeroIdx(n);

  if (zeroIdx < 2) return 0;

  let number = [];
  zeroIdx.forEach((item, idx) => {
    if (idx === zeroIdx.length - 1) return;
    number.push(n.slice(item + 1, zeroIdx[idx + 1]));
  });

  number = number.filter((num) => isPrime(num) && num !== "");

  return number.length;
}
```

위의 함수에서는 `0P0`의 경우를 가지는 소수의 개수를 찾는다. `findZeroIdx()` 함수의 반환값은 0이 위치한 자리이다. 이를 반복문을 통해 중간에 있는 숫자를 `slice()` 메서드를 사용하여 `number` 배열에 넣는다.

빈문자열도 추가될 수 있으니 `filter()` 메서드를 통해 걸러내야 한다. 이때, `isPrime()` 함수를 통해 소수가 아닌 것도 걸러낸다.

---

### 결과

![programmers_find_decimals_result1](/image/CodingTest/programmers_find_decimals/programmers_find_decimals_result1.png)

---

## 4. Refactoring

문제의 접근을 달리해보았다. `n`를 `k진수`로 바꾼 수를 `0`을 기준으로 배열을 만들었다.

이렇게 된다면 맨 앞과 맨 뒤의 요소는 각각 `P0`, `0P`에 해당하고 중간에 있는 요소들은 `0P0`에 해당한다.

그리고 만약 요소가 단 하나라면 이는 `P`에 해당한다.

여기서 주의해야할 점은 `00`이렇게 연속으로 `0`이 두 번 이상 등장하면 빈문자열도 요소로 추가되기 때문에 이를 `filter()` 메서드로 제거를해야 한다.

아래의 코드이다. 훨씬 간단해졌다.

```javascript
function isPrime(n) {
  if (n === "1") return false;

  if (n % 2 === 0) {
    return n === "2" ? true : false;
  }

  const sqrt = parseInt(Math.sqrt(n));

  for (let divider = 3; divider <= sqrt; divider += 2) {
    if (n % divider === 0) {
      return false;
    }
  }

  return true;
}

function solution(n, k) {
  n = n.toString(k);
  const numberArr = n.split("0").filter((number) => number !== "");

  let count = 0;
  numberArr.forEach((number) => {
    if (isPrime(number)) count++;
  });

  return count;
}
```

### 결과

![programmers_find_decimals_result2](/image/CodingTest/programmers_find_decimals/programmers_find_decimals_result2.png)

실행 속도도 더 빨라진 것을 확인할 수 있다.

---

## 5. Conclusion

> 현재 우테코 프리코스를 진행하고 있다. 프리코스에서 함수를 분리하는 것을 연습하고 있어서 그런지, 특정한 기능이 보이면 함수로 분리하는 것 부터 생각을 하는 듯 하다. 물론 틀린 방법은 아니지만 어떻게 보면 더 어렵게 접근을 하는 것 같다. 코딩테스트는 정확성도 중요하지만 속도도 중요하기 때문에 무작성 함수를 분리하는 것이 아니라 효율적인 흐름을 생각하도록 하자. 그래도 함수를 분리하니 공부는 많이 된 듯하여 뿌듯한 기분이 든다.

---

📅 2022-11-05
