# 재귀(Recursion)

## 1. 재귀 함수를 사용하는 이유

먼저, 재귀란? 자신 자신을 호출하는 함수를 말한다. 그렇다면 이런 재귀 함수를 사용하는 이유는 무엇일까?

1. 재귀 함수는 모든 곳에 사용된다.
   - 예를들어 자바스크립트의 `JSON.parse/JSON.stringify`는 재귀적으로 실행된다.
2. 보다 복잡한 데이터 구조에서 재귀를 사용하면 더 간단하게 이해할 수 있다.
3. 때로는 일반적인 순회보다 재귀가 더욱 깔끔해 보일 수 있다.

---

## 2. 재귀 함수의 기본 요소

재귀 함수를 사용하기 위해선 두 가지의 기본 요소를 가춰야 한다. 첫 번째는 `종료 조건`이다. 재귀 함수가 호출 스택에 무한정 쌓이게 된다면 컴퓨터 성능에 악영향을 미치기 때문에 반드시 종료를 할 수 있는 조건이 포함되어야 한다. 두 번째는 `다른 입력값`이다. 재귀 함수를 호출할 때 매번 같은 입력값을 받으며 호출하게 된다면 재귀 함수를 사용하는 의미가 없다. 때문에 매번 다른 입력값을 재귀 함수의 인자로 보내야 한다.

### 2-1. 첫 번째 예제

간단한 예제를 통해 알아보자.

```javascript
function countDown(num) {
  if (num < 1) {
    console.log('완료!');
    return;
  }

  console.log(num);
  num -= 1;
  countDown(num);
}

countDown(5);
```

위 `countDown` 함수는 내부에서 자기 자신을 다시 호출하는 재귀 함수이다. 이 재귀 함수를 바탕으로 기본 요소를 살펴보자.

1. 종료 조건: 인자(num)가 1보다 작을 때
2. 다른 입력값: 함수가 호출 될 때 마다 인자(num)가 1씩 감소한다.

이번엔 표로 `countDown` 함수가 어떻게 호출되는지 살펴보자. 매번 달라지는 인자(num)와 인자에 따른 종료 여부에 집중해보자.

| 순서 | 인자(num) | 콘솔 값 | 종료 여부 |
| ---- | --------- | ------- | --------- |
| 1    | 5         | 5       | x         |
| 2    | 4         | 4       | x         |
| 3    | 3         | 3       | x         |
| 4    | 2         | 2       | x         |
| 5    | 1         | 1       | x         |
| 6    | 0         | 완료!   | o         |

### 2-2. 두 번째 예제

재귀 함수의 또 다른 예제이다.

```javascript
function sumRange(num) {
  if (num === 1) return 1;
  return num + sumRange(num - 1);
}

sumRange(4);
```

`sumRange` 함수의 기본 요소를 살펴보자.

1. 종료 조건: 인자(num)이 1일 때
2. 다른 입력값: 함수가 호출 될 때 마다 인자(num)가 1씩 감소한다.

표로 `sumRange` 함수의 호출을 살펴보자.

| 순서 | 호출 함수   | 반환 값           | 종료 여부 |
| ---- | ----------- | ----------------- | --------- |
| 1    | sumRange(4) | `4 + sumRange(3)` | x         |
| 2    | sumRange(3) | `3 + sumRange(2)` | x         |
| 3    | sumRange(2) | `2 + sumRange(1)` | x         |
| 4    | sumRange(1) | `1`               | o         |

순서가 4일 때 `sumRange(1)`이 호출 되고 인자가 1이기 때문에 이는 `1`를 반환한다.
이어서 차례대로 `sumRange(2)`, `sumRange(3)`, `sumRange(4)`의 값이 반환되어 최종적으로 `10`이 반환된다.

---

### 2-3. 세 번째 예제 - 팩토리얼 구현하기

반복문으로 팩토리얼 구현하고 이어서 재귀 호출로 팩토리얼을 구현해보자.

먼저 반복문으로 구현한 코드이다.

```javascript
function factorial(num) {
  let total = 1;
  for (let i = num; i > 1; i--) {
    total *= i;
  }
  return total;
}
```

`num` 부터 시작하여 1씩 작아지는 반복문이다. 반복문은 1씩 작아지는 `num(i)`이 1보다 작아지면 종료한다. 반복문 내에서는 기존 값(total)에 현재의 `num(i)`를 곱하는 코드가 있다.

이번엔 재귀 호출로 구현한 코드이다.

```javascript
function factorial(num) {
  if (num === 1) return 1;
  return num * factorial(num - 1);
}
```

반복문으로 구현한 것보다 간결한 코드를 볼 수 있다. 내부적으로 자기 자신을 호출하고 있고 이때 인자는 매개변수로 받은 `num` 보다 1작은 수 이다. 즉, 이는 `다른 입력값` 조건에 맞다. 또한 `num`이 1일 때 종료되는 `종료 조건`도 만족하고 있다.

---

## 3. Helper 메소드 재귀

`Helper 메소드 재귀`에는 두 개의 함수가 있다. 외부 함수(outer function)과 재귀 함수가 그것이다. 외부 함수는 재귀적으로 동작하지 않고 단지 내부의 재귀 함수를 호출하는 역할을 한다. 또한 외부 함수에는 재귀 함수에서 실행하는 코드의 결과를 저장할 수 있는 저장 공간을 만들어 둔다.

간단한 예를 살펴보자.

```javascript
function outer(input) {
  let result = []; // 외부의 저장 공간

  // 재귀 함수
  function helper(input) {
    if (input === 0) return; // 종료 조건
    result.push(input); // 외부 함수의 result에 데이터 저장
    helper(input - 1); // 다른 입력값으로 자기 자신을 재 호출
  }
  helper(input);

  return result;
}
```

구체적인 예시를 하나 더 살펴보자. 양의 정수를 요소로 가지는 배열을 매개변수로 받고 해당 배열에서 홀수만 반환하는 함수를 만들어보자.

```javascript
function collectOddValues(arr) {
  let result = []; // 외부의 저장 공간

  // 재귀 함수
  function helper(arr) {
    if (arr.length === 0) return; // 종료 조건
    if (arr[0] % 2 !== 0) result.push(arr[0]); // 홀수일 때 외부의 저장 공간에 해당 숫자 저장
    helper(arr.slice(1)); // 다른 입력값으로 자기 자신을 재 호출
  }

  helper(arr);

  return result;
}
```

예제를 통해 살펴본 것과 같이 `helper 메소드 재귀`는 두 가지의 함수가 사용되고 각각 아래의 역할을 수행하게 된다.

1. 외부 함수: 재귀 함수를 처름으로 호출, 저장 공간을 만들어 재귀 함수의 결과에 따라 데이터 저장
2. 재귀 함수: 종료 조건과 다른 입력값이라는 조건을 만족하며, 상황에 맞는 역할을 수행

---

## 4. 순수 재귀

순수 재귀는 외부의 데이터 저장 공간을 활용하지 않는 재귀를 말한다. 즉, `helper 메소드 재귀`에서 외부 함수가 없고 단지 재귀 함수가 있는 형태를 말한다.

`helper 메소드 재귀`의 두 번째 예시인, 홀수만 뽑아내는 예제를 순수 재귀로 만들어 보자.

```javascript
function collectOddValues(arr) {
  let newArr = [];

  if (arr.length === 0) return newArr;
  if (arr[0] % 2 !== 0) newArr.push(arr[0]);

  newArr = [...newArr, ...collectOddValues(arr.slice(1))];
  return newArr;
}
```

외부의 데이터 저장 공간을 활용하지 않고 자기 자신과 계속 해서 재 호출하는 순수 재귀 함수이다. 확실히 `helper 메소드 재귀`의 방법보다 생각할 것이 더 늘어나 복잡해 보인다. 그러니 표로 정리하여 살펴보자.

| 순서 | 호출 함수                     | 반환 값                             | 종료 여부 |
| ---- | ----------------------------- | ----------------------------------- | --------- |
| 1    | collectOddValues([1,2,3,4,5]) | [1, ...collectOddValues([2,3,4,5])] | x         |
| 2    | collectOddValues([2,3,4,5])   | [...collectOddValues([3,4,5])]      | x         |
| 3    | collectOddValues([3,4,5])     | [3, ...collectOddValues([4,5])]     | x         |
| 4    | collectOddValues([4,5])       | [...collectOddValues([5])]          | x         |
| 5    | collectOddValues([5])         | [5, ...collectOddValues([])]        | x         |
| 6    | collectOddValues([])          | []                                  | o         |

6번째 순서에서 재귀 함수가 종료가 되고 값을 반환하며 위로 올라가게 된다. 거꾸로 올라가는 과정은 아래와 같다.

1. collectOddValues([5]) = [5, ...collectOddValues([])] -> [5]
2. collectOddValues([4, 5]) = [...collectOddValues([5])] -> [5]
3. collectOddValues([3, 4, 5]) = [3, ...collectOddValues([4, 5])] -> [3, 5]
4. collectOddValues([2, 3, 4, 5]) = [...collectOddValues([3, 4, 5 ])] -> [3, 5]
5. collectOddValues([1, 2, 3, 4, 5]) = [...collectOddValues([2, 3, 4, 5])] -> [1, 3, 5]

---

## 5. Conclusion

> 아주 간단한 예제를 통해 재귀 함수에 대해 살펴보았다. 이번에 살펴본 예제는 모두 반복문으로 코드를 작성해도 괜찮았지만 재귀를 공부한다는 마음가짐으로 코드를 작성해보고 표를 만들어 순서를 생각해 보았다. 처음 코딩 테스트에서 재귀 함수를 접했을 때 이 만큼 무서운 것이 없었다. 하지만 여러 문제를 풀어보니 재귀를 어느 정도 다룰 수 있게 되었다. 물론 끔찍하게 어려운 문제는 아직 부족하지만... 이번 공부에서 좋았던 점은 재귀 함수가 가져야 할 필수 조건을 정리할 수 있었던 것이다. 종료 조건과 다른 입력값, 이 두 개를 잘 기억하여 재귀 함수를 만들어보자.

---

## 참고

Udemy - JavaScript 알고리즘 & 자료구조 마스터클래스 / 섹션 7: 재귀(Recursion)

---

📅 2023-01-20
