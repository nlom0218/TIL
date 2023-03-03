# 둘만의 암호

## 1. 개요

- 프로그래머스
- Lv.1
- 연습문제
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/155652)

---

## 2. 문제 설명

두 문자열 s와 skip, 그리고 자연수 index가 주어질 때, 다음 규칙에 따라 문자열을 만들려 합니다. 암호의 규칙은 다음과 같습니다.

- 문자열 s의 각 알파벳을 index만큼 뒤의 알파벳으로 바꿔줍니다.
- index만큼의 뒤의 알파벳이 z를 넘어갈 경우 다시 a로 돌아갑니다.
- skip에 있는 알파벳은 제외하고 건너뜁니다.

예를 들어 s = "aukks", skip = "wbqd", index = 5일 때, a에서 5만큼 뒤에 있는 알파벳은 f지만 [b, c, d, e, f]에서 'b'와 'd'는 skip에 포함되므로 세지 않습니다. 따라서 'b', 'd'를 제외하고 'a'에서 5만큼 뒤에 있는 알파벳은 [c, e, f, g, h] 순서에 의해 'h'가 됩니다. 나머지 "ukks" 또한 위 규칙대로 바꾸면 "appy"가 되며 결과는 "happy"가 됩니다.

두 문자열 s와 skip, 그리고 자연수 index가 매개변수로 주어질 때 위 규칙대로 s를 변환한 결과를 return하도록 solution 함수를 완성해주세요.

### 2-1. 문제 설명 - 제한 사항

- 5 ≤ s의 길이 ≤ 50
- 1 ≤ skip의 길이 ≤ 10
- s와 skip은 알파벳 소문자로만 이루어져 있습니다.
  - skip에 포함되는 알파벳은 s에 포함되지 않습니다.
- 1 ≤ index ≤ 20

### 2-2. 문제 설명 - 입출력 예

| s       | skip   | index | result  |
| ------- | ------ | ----- | ------- |
| "aukks" | "wbqd" | 5     | "happy" |

### 2-3. 문제 설명 - 입출력 예

입출력 예 #1
본문 내용과 일치합니다.

---

## 3. 문제 풀이

```javascript
function solution(s, skip, index) {
  // 1) 알파벳(소문자) 배열 만들기
  let alphabet = new Array(26).fill().map((_, idx) => {
    return String.fromCharCode(idx + 97);
  });

  // 2) skip 하는 알바펫 제거하기
  alphabet = alphabet.filter((str) => ![...skip].includes(str));

  return s.split('').reduce((acc, cur) => {
    // 3) 현재 인덱스에서 index를 더한 값을 알바벳의 길이 만큼 나누기, 나눈 나머지를 새로운 인덱스로 정하기
    let newIdx = (alphabet.indexOf(cur) + index) % alphabet.length;

    // 4) 새로운 인덱스에 해당하는 값을 계속해서 더하기
    return (acc += alphabet[newIdx]);
  }, '');
}
```

### 1) 알파벳(소문자) 배열 만들기

```javascript
let alphabet = new Array(26).fill().map((_, idx) => {
  return String.fromCharCode(idx + 97);
});
```

`new Array()`를 통해 빈 값의 배열 26개를 생성하고 여기에 `String.fromCharCode()`를 활용해서 소문자를 가져와 배열에 담는다. 이는 아래와 같은 코드로도 생성이 가능하다.

```javascript
let alphabet = Array.from({ length: 26 }, (_, idx) =>
  String.fromCharCode(idx + 97)
);
```

`Array.from()` 메서드를 사용하는 것이 더 깔끔해 보인다:)

### 2) skip 하는 알바펫 제거하기

```javascript
alphabet = alphabet.filter((str) => ![...skip].includes(str));
```

문제의 조건을 보면 `alphabet` 배열에 제거해야 하는 문자열이 있다. 이것이 바로 `skip` 문자열이고 이를 `filter` 메서드를 사용하여 `alphabet` 배열에서 제거한다.

1), 2)과정은 아래과 같이 함께 작성할 수 있다.

```javascript
let alphabet = Array.from({ length: 26 }, (_, idx) =>
  String.fromCharCode(idx + 97)
).filter((str) => ![...skip].includes(str));
```

### 3) 현재 인덱스에서 index를 더한 값을 알바벳의 길이 만큼 나누기, 나눈 나머지를 새로운 인덱스로 정하기

```javascript
let newIdx = (alphabet.indexOf(cur) + index) % alphabet.length;
```

`reduce()` 메서드에서는 처음에 항상 `newIdx`를 찾는다. 찾는 과정은 아래와 같다.

1. 현재의 인덱스에 `index`를 더한다.
2. 위에서 구한 값이 `alphabet` 배열의 길이보다 길 수 있으므로 `alphabet` 배열의 길이로 나눈다.
3. 나눈 값의 나머지를 새로운 인덱스로 정한다.

### 4) 새로운 인덱스에 해당하는 값을 계속해서 더하기

```javascript
return (acc += alphabet[newIdx]);
```

이전 값에 `newIdx`에 해당하는 알파벳을 더한다.

---

## 4. Conclusion

> 코딩 테스트 풀이와 알고리즘, 자료구조 공부를 할 여유를 찾기 힘들다ㅠㅠ 이 문제도 정말 중간에 심심풀이로 푼 문제이다. 쉬운 문제였으나 오랜만에 TIL에 정리를 한 것 뿐이다. 아무튼 오랜만에 문제도 풀고 정리도 하니 재밌긴 하다. 물론 이전에도 재미있었지만! 레벨 1 문제가 계속 올라오니 올라올 때 마다 쭉 풀어보자. 그러다가 정리도 필요한 문제가 있으면 정리도 하고!

---

📅 2023-03-03
