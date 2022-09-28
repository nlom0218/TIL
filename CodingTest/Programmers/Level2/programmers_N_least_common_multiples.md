# N개의 최소공배수

## 1. 개요

- 프로그래머스
- Lv.2
- 연습문제
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/12953)

---

## 2. 문제 설명

두 수의 최소공배수(Least Common Multiple)란 입력된 두 수의 배수 중 공통이 되는 가장 작은 숫자를 의미합니다. 예를 들어 2와 7의 최소공배수는 14가 됩니다. 정의를 확장해서, n개의 수의 최소공배수는 n 개의 수들의 배수 중 공통이 되는 가장 작은 숫자가 됩니다. n개의 숫자를 담은 배열 arr이 입력되었을 때 이 수들의 최소공배수를 반환하는 함수, solution을 완성해 주세요.

---

### 2-1. 문제 설명 - 제한 사항

- arr은 길이 1이상, 15이하인 배열입니다.
- arr의 원소는 100 이하인 자연수입니다.

---

### 2-2. 문제 설명 - 입출력 예

| arr        | result |
| ---------- | ------ |
| [2,6,8,14] | 168    |
| [1,2,3]    | 6      |

---

## 3. 문제 풀이

```javascript
function solution(arr) {
  // 1) arr 배열의 요소가 하나라면 해당 요소를 반환하기
  if (arr.length === 1) return arr[0];

  // 2) 두 개 수를 매개변수로 받아 최소공배수를 만드는 함수
  function getLeastCommonMultiple(a, b) {
    // 3) 큰수와 작은수 파악하기
    let max = a > b ? a : b;
    let min = a < b ? a : b;
    let i = 1;

    // 4) 큰수에 1부터 곱하여 나온 수를 작은수로 나누었을 때 나머지가 0이 될 때까지 반복문 실행하기
    while ((max * i) % min !== 0) {
      i++;
    }

    // 5) 더 이상 배열에서 숫자를 가져올 수 없으면 재귀함수 탈출하기
    if (arr.length === 0) return max * i;

    // 6) 구한 최소공배수와 또 다른 수의 최소공배수를 구하기
    return getLeastCommonMultiple(max * i, arr.pop());
  }
  return getLeastCommonMultiple(arr.pop(), arr.pop());
}
```

---

### 1) arr 배열의 요소가 하나라면 해당 요소를 반환하기

```javascript
if (arr.length === 1) return arr[0];
```

`arr` 배열의 요소가 하나 뿐이라면 최소공배수는 해당 요소의 값이 된다.

---

2. 두 개 수를 매개변수로 받아 최소공배수를 만드는 함수

```javascript
function getLeastCommonMultiple(a, b) {}
```

`getLeastCommonMultiple` 함수는 두 개의 매개변수를 받는다. 모두 숫자이며 두 숫자의
최소공배수를 구하는 역할을 한다.

위의 풀이에서는 재귀함수로 사용하였다. 즉, 최소공배수를 더 만들어야 하는 경우 `getLeastCommonMultiple` 함수 내에서
다시 `getLeastCommonMultiple`를 호출한다.

---

### 3) 큰수와 작은수 파악하기

```javascript
let max = a > b ? a : b;
let min = a < b ? a : b;
```

매개변수로 받은 `a`와 `b` 중 큰 값을 `max` 변수에, 작은 값을 `min` 변수에 할당한다.

사실 `max`와 `min`으로 나누지 않다도 된다. 하지만 반복문을 최소화하기 위해 나누었다.
`max`와 `min`으로 나눈 이유는 만약 3과 3000의 최소공배수를 구해야 하는 경우를 생각하면 쉽게 이해할 수 있다.

3과 3000의 최소공배수는 3000인데 3이 3000이 되기 위해서는 1000을 곱해야 하지만 3000은 1만 곱하면 된다.
즉 반복문을 1000번 실행해야 하는야 1번을 실행해야 하느냐의 차이이다.

---

### 4) 큰수에 1부터 곱하여 나온 수를 작은수로 나누었을 때 나머지가 0이 될 때까지 반복문 실행하기

```javascript
while ((max * i) % min !== 0) {
  i++;
}
```

`max`에 `i`를 곱한다. 곱한 수가 `min`으로 나누었을 때 나머지가 `0`이라면 반복문을 종료한다.
즉, 곱한수가 `max`와 `min`의 최소공배수다.

나머지가 0이 아니라면 아직 최소공배수를 구하지 못했으므로 `i`의 값을 1씩 증가시키며 최소공배수를 구한다.

---

### 5) 더 이상 배열에서 숫자를 가져올 수 없으면 재귀함수 탈출하기

```javascript
if (arr.length === 0) return max * i;
```

재귀함수는 반드시 탈출하는 조건이 필요하다. 위의 코드가 바로 탈출하는 조건이다. 배열에 더 이상 요소가 없으면
더 이상 최소공배수를 구하지 않다고 되므로 재귀함수를 종료하고 그때의 최소공배수인 `max * i`를 반환한다.

---

### 6) 구한 최소공배수와 또 다른 수의 최소공배수를 구하기

```javascript
return getLeastCommonMultiple(max * i, arr.pop());
```

다음 숫자와의 최소공배수를 구하기 위해 다시 `getLeastCommonMultiple` 함수를 호출한다.
이때 `arr` 배열의 요소는 `Array.pop()` 메서드를 사용하여 가져온다.

---

### 7) 결과

![programmers_N_least_common_multiples_result1](/image/CodingTest/programmers_N_least_common_multiples/programmers_N_least_common_multiples_result1.png)

---

## 4. Conclusion

> 이 문제를 풀이로 정리할까 말까 고민을 하였다. 그 이유는 정말 어렵지 않게 풀었기 때문이다. 하지만 가져온 이유는
> 재귀함수를 사용했기 때문이다. 정확히 말하면 꼬리 재귀를 사용했다. 함수 내에서 리턴하는 값이 자기 자신의 함수!  
> 약 2달 전 난 재귀함수에 대해 많이 부족하였다. 그 땐 재귀함수라는 말을 듣기만해도 어려운 것이라 생각하여
> 피하려고 했지만 지금은 "이건 재귀함수로 풀 수 있을 것 같은데?"라는 생각과 코드로 구현을 하고 있다. 아직
> 많은 문제를 풀지 못하였고 어려운 문제를 만났을 때 긴 시간이 걸리긴 하지만 조금씩 발전하는 모습이 들어 뿌듯하기도 하다.
> 단, 알고리즘, 자료구조 특히 그래프 개념은 아직도 너무나도 약하다...ㅠㅠ

---

📅 2022-09-28
