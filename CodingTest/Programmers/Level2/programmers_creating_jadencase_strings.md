# JadenCase 문자열 만들기

## 1. 개요

- 프로그래머스
- Lv.2
- 연습문제
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/12951)

---

## 2. 문제 설명

JadenCase란 모든 단어의 첫 문자가 대문자이고, 그 외의 알파벳은 소문자인 문자열입니다. 단, 첫 문자가 알파벳이 아닐 때에는 이어지는 알파벳은 소문자로 쓰면 됩니다. (첫 번째 입출력 예 참고)

문자열 s가 주어졌을 때, s를 JadenCase로 바꾼 문자열을 리턴하는 함수, solution을 완성해주세요.

---

### 2-1. 문제 설명 - 제한 조건

- s는 길이 1 이상 200 이하인 문자열입니다.
- s는 알파벳과 숫자, 공백문자(" ")로 이루어져 있습니다.
  - 숫자는 단어의 첫 문자로만 나옵니다.
  - 숫자로만 이루어진 단어는 없습니다.
  - 공백문자가 연속해서 나올 수 있습니다.

---

### 2-2. 문제 설명 - 입출력 예

| s                       | return                  |
| ----------------------- | ----------------------- |
| "3people unFollowed me" | "3people Unfollowed Me" |
| "for the last week"     | "For The Last Week"     |

---

## 3. 문제 풀이

```javascript
function solution(s) {
  let answer = "";

  // 1) 문자열을 배열로 바꾸기
  const s_arr = [...s];

  // 2) 이전 문자가 공백인지 확인하기
  let is_blank = true;
  s_arr.forEach((item) => {
    // 3) 이전 문자가 공백이면 대문자로 바꾼후 answer에 더하기
    if (is_blank) answer += item.toUpperCase();
    // 4) 이전 문자가 공백이 아니라면 소문자로 바꾼후 answer에 더하기
    else answer += item.toLowerCase();

    // 5) 현재 문자열에 따라 is_blank값을 바꾸기
    if (item === " ") is_blank = true;
    else is_blank = false;
  });
  return answer;
}
```

---

### 1) 문자열을 배열로 바꾸기

```javascript
const s_arr = [...s];
```

`spread operator`을 사용하여 문자열을 모두 나눈 후 각각의 문자를 요소로
가지는 배열을 만든다.

---

### 2) 이전 문자가 공백인지 확인하기

```javascript
let is_blank = true;
```

`is_blank` 변수는 반복문에서 현재 처리하고 있는 문자의 앞 문자의 상태를 나타낸다.

초기값은 `true`이며 이는 0번째 인덱스의 앞 부분은 공백과 마찬가지이기 때문이다.

---

### 3) 이전 문자가 공백이면 대문자로 바꾼후 answer에 더하기

```javascript
if (is_blank) answer += item.toUpperCase();
```

앞 문자가 공백이면 대문자로 바꾼 후 `answer`값에 더한다.

---

### 4) 이전 문자가 공백이 아니라면 소문자로 바꾼후 answer에 더하기

```javascript
else answer += item.toLowerCase();
```

앞 문자가 공백이 아니라면 소문자로 바꾼 후 `answer`값에 더한다.

---

### 5) 현재 문자열에 따라 is_blank값을 바꾸기

```javascript
if (item === " ") is_blank = true;
else is_blank = false;
```

만약 문자가 빈 문자열(공백)이면 `is_blank`을 `true`로, 그렇지 않으면 `false`로
바꾼다.

---

### 결과

![programmers_creating_jadencase_strings_result1](/image/CodingTest/programmers_creating_jadencase_strings/programmers_creating_jadencase_strings_result1.png)

---

## 4. Refactoring

매개 변수로 받은 문자열 `s`를 배열로 바꾸지 않고 풀이를 작성해보았다.

```javascript
function solution(s) {
  let answer = "";
  let is_blank = true;
  for (let i = 0; i < s.length; i++) {
    if (is_blank) answer += s[i].toUpperCase();
    else answer += s[i].toLowerCase();

    if (s[i] === " ") is_blank = true;
    else is_blank = false;
  }
  return answer;
}
```

`for`문 사용하여 문자열 `s`의 각 문자를 순회한다.

---

### 결과

![programmers_creating_jadencase_strings_result2](/image/CodingTest/programmers_creating_jadencase_strings/programmers_creating_jadencase_strings_result2.png)

---

## 5. 다른 사람 풀이

나와 풀이가 비슷하지만 좀 더 간단한 풀이를 가져왔다.

```javascript
function solution(s) {
  var answer = "";
  for (let i = 0; i < s.length; i++) {
    if (i === 0 || s[i - 1] === " ") {
      answer += s[i].toUpperCase();
    } else {
      answer += s[i].toLowerCase();
    }
  }
  return answer;
}
```

나의 풀이에서는 `is_blank`라는 변수를 사용하여 이전 문자의 공백을 확인하였지만
다른 사람의 풀이에서는 단지 이전 인덱스의 값을 찾아서 공백을 확인한다.

---

### 결과

![programmers_creating_jadencase_strings_result3](/image/CodingTest/programmers_creating_jadencase_strings/programmers_creating_jadencase_strings_result3.png)

---

## 6. Conclusion

> Level2인 문제이지만 어렵지 않게 풀었다. 연습문제여서 그런 듯 하다.
> 해당 문제를 풀면서 느꼈던 점은 문제를 풀 때 꼭 배열이나 다른 자료 구조로 바꾸어
> 풀지 않고 최대한 매개변수로 주어진 자료값을 이용하여 풀어야 겠다는 것이다. 처음 나의 풀이에선
> 배열로 바꾸었지만 굳이 배열로 바꾸지 않아도 풀 수 있는 문제이다. 또한 꼭 필요한
> 변수가 사용하도록 하자. 변수가 많아지면 가독성은 떨어질 뿐 아니라 문제를 더
> 복잡하게 하는 상황을 만들 수 있기 때문이다.

---

📅 2022-09-10
