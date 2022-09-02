# 올바른 괄호

## 1. 개요

- 프로그래머스
- Lv.2
- 스택/큐
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/12909)

---

## 2. 문제 설명

괄호가 바르게 짝지어졌다는 것은 '(' 문자로 열렸으면 반드시 짝지어서 ')' 문자로 닫혀야 한다는 뜻입니다. 예를 들어

- "()()" 또는 "(())()" 는 올바른 괄호입니다.
- ")()(" 또는 "(()(" 는 올바르지 않은 괄호입니다.

'(' 또는 ')' 로만 이루어진 문자열 s가 주어졌을 때, 문자열 s가 올바른 괄호이면 true를 return 하고, 올바르지 않은 괄호이면 false를 return 하는 solution 함수를 완성해 주세요.

---

### 2-1. 문제 설명 - 제한 사항

- 문자열 s의 길이 : 100,000 이하의 자연수
- 문자열 s는 '(' 또는 ')' 로만 이루어져 있습니다.

---

### 2-2. 문제 설명 - 입출력 예

| s        | answer |
| -------- | ------ |
| "()()"   | true   |
| "(())()" | true   |
| ")()("   | false  |
| "(()("   | false  |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1,2,3,4  
문제의 예시와 같습니다.

---

## 3. 문제 풀이

```javascript
function solution(s) {
  let answer;

  // 1) "("와 ")"의 수를 세는 객체 선언하기
  const obj = { "(": 0, ")": 0 };

  // 2) 문자열을 배열로 만든 후 배열의 순서를 역순으로 하기
  const array = [...s].reverse();

  // 3) 배열의 길이 만큼 반복하기
  for (let i = 0; i < s.length; i++) {
    // 4) 배열의 마지막 요소를 가져오기 및 open, close 변수 선언하기
    const item = array.pop();
    const open = obj["("];
    const close = obj[")"];

    // 5) close가 open보다 많아지게 되면 올바르지 않는 형태의 괄호이다.
    if (close > open) {
      answer = false;
      break;
    }

    // 6) 키(괄호)에 해당하는 프로퍼티 값을 1증가시키기
    obj[item]++;

    // 7) 만약 마지막 순서의 반복문이라면 open의 값과 close의 값을 비교하기
    if (i === s.length - 1) {
      const open = obj["("];
      const close = obj[")"];
      answer = open === close ? true : false;
    }
  }
  return answer;
}
```

### 1) "("와 ")"의 수를 세는 객체 선언하기

```javascript
const obj = { "(": 0, ")": 0 };
```

문자열은 "("와 ")"로 구성되어있다. 문자열에 "("와 ")"가 얼마만큼 있는지 수로 나타내기
위한 객체인 `obj`를 선언하고 초기값으로 "("와 ")"을 키로 지정하고 각각 0을 프로퍼티로 한다.

---

### 2) 문자열을 배열로 만든 후 배열의 순서를 역순으로 하기

```javascript
const array = [...s].reverse();
```

문자열을 구조분해할당을 사용하여 배열로 바꾼다. 그리고 `Array.reverse()` 메서드를 사용하여 배열의
순서를 역순으로 한다. 순서를 역순으로 하는 이유는 배열의 요소를 뒤에서 부터 제거하게 되면 시간복잡도 `O(1)`이기 때문이다.
만약 배열에서 요소를 앞에서 부터 빼게 된다면 시간복잡도는 `O(1)`를 가지기 때문에 보다 효율적인 방법을 선택하였다.

즉, 배열을 역순으로 하여 뒤에서 부터 요소를 빼는 것은 결국 원본 문자열에서 앞의 문자부터 제거하는 것과 같다.

---

### 3) 배열의 길이 만큼 반복하기

```javascript
for (let i = 0; i < s.length; i++) {}
```

배열의 길이만큼 반복문을 실행한다. 배열의 길이 만큼 실행함으로써 배열에 있는 모든 요소를 검사할 수 있다.

---

### 4) 배열의 마지막 요소를 가져오기 및 open, close 변수 선언하기

```javascript
const item = array.pop();
const open = obj["("];
const close = obj[")"];
```

배열의 마지막 요소를 `Array.pop()` 메서드를 통해 가져오고 있다.

그리고 `open`과 `close` 변수를 선언하였는데 이는 `obj`의 "("와 ")"의
프로퍼티를 각각 가져와 할당한다.

---

### 5) close가 open보다 많아지게 되면 올바르지 않는 형태의 괄호이다.

```javascript
if (close > open) {
    answer = false;
    break;
}
```

만약 `close`가 `open`보다 커지게 되면 올바르지 않는 괄호의 문자열이기 때문에 그 즉시
`answer`를`false`로 바꾸고 반복문을 종료한다.

여는 괄호보다 닫는 괄호가 많아지면 안되기 때문이다.

---

### 6) 키(괄호)에 해당하는 프로퍼티 값을 1증가시키기

```javascript
obj[item]++;
```

반복문을 실행하면서 `item`에 해당하는 키의 프로퍼티 값을 1증가시킨다.

---

### 7) 만약 마지막 순서의 반복문이라면 open의 값과 close의 값을 비교하기

```javascript
if (i === s.length - 1) {
  const open = obj["("];
  const close = obj[")"];
  answer = open === close ? true : false;
}
```

만약 마지막 순서의 반복문이라면 최종적으로 저장된 "(와 ")"의 수를 비교한다.
이때 수가 같다면 `true`를 수가 다르다면 `false`를 반환한다.

---

### 결과

![programmers_correct_parenthesis_result](/image/CodingTest/programmers_correct_parenthesis/programmers_correct_parenthesis_result1.png)

---

## 4. 스택을 이용한 문제 풀이

```javascript
function solution(s) {
  const stack = [];

  for (let i = 0; i < s.length; i++) {
    if (s[i] === "(") stack.push(1);
    else {
      if (stack.length === 0) return false;
      stack.pop();
    }
  }
  return stack.length === 0;
}
```

위의 방법은 `stack` 배열에 문자열이 "("인 경우 1를 추가하고 ")"인 경우 배열의 마지막 요소를 제거하는 형식이다.

`stack.length`가 0인 상태는 열린 괄호가 없다는 것이다. 하지만 이 상태에서 닫는 괄호가 들어온다는 것은
올바르지 않는 괄호의 문자열이기 때문에 그 즉시 `false`를 반화한다.

> 여기서 재밌는 점은 `stack.push(1)`를 하면 효율성 테스트를 통과하지만 `stack.push("(")`를
> 하게 되면 효율성 테스트를 통과하지 못한다.

---

### 결과

![programmers_correct_parenthesis_result2](/image/CodingTest/programmers_correct_parenthesis/programmers_correct_parenthesis_result2.png)

---

## 5. 숫자의 카운팅을 활용한 문제 풀이

```javascript
function solution(s) {
  let count = 0;

  for (let i = 0; i < s.length; i++) {
    if (s[i] === "(") count++;
    else {
      if (count === 0) return false;
      count--;
    }
  }
  return count === 0;
}
```

이번에는 굳이 배열을 선언해서 배열에 값을 추가, 삭제하는 방법이 아니라
단순히 `count` 변수에 숫자를 1씩 더하거나 빼 올바른 괄호의 문자열인지
확인하는 풀이이다.

- `count`가 양수면 여는 괄호가 더 많이 있다는 것이다. 이후에 닫아야 하는 괄호로 열린 괄호를 닫아야 한다.
- `count`가 0이면 여는 괄호와 닫는 괄호가 모두 있어 올바른 괄호이다.
- `count`가 음수면 닫는 괄호가 더 많아 올바르지 않는 괄호이다.

---

### 결과

![programmers_correct_parenthesis_result3](/image/CodingTest/programmers_correct_parenthesis/programmers_correct_parenthesis_result3.png)

---

## 6. Conclusion

> `스택`을 이용한 풀이와 숫자의 카운팅을 활용한 문제 풀이는 자료구조 강의를 보면서 공부를 하였다. 개인적인 생각으론 해당 부분이
> `큐` 파트에 있는데 `스택`의 자료구조이지 않나 싶다. 왜냐하면 FILO의 형태로 보이기 때문이다. 스택, 큐, 연결 리스트에 대한
> 공부를 어느정도 하고 문제를 보니 문제가 눈에 더 잘 들어오는 것 같다. 앞으로 자료구조, 알고리즘 공부를 꾸준히 해야겠다.

---

📅 2022-09-02
