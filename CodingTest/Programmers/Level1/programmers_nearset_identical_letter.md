# 가장 가까운 같은 글자

## 1. 개요

- 프로그래머스
- Lv.1
- 연습문제
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/142086)

---

## 2. 문제 설명

문자열 s가 주어졌을 때, s의 각 위치마다 자신보다 앞에 나왔으면서, 자신과 가장 가까운 곳에 있는 같은 글자가 어디 있는지 알고 싶습니다.

예를 들어, s="banana"라고 할 때, 각 글자들을 왼쪽부터 오른쪽으로 읽어 나가면서 다음과 같이 진행할 수 있습니다.

- b는 처음 나왔기 때문에 자신의 앞에 같은 글자가 없습니다. 이는 -1로 표현합니다.
- a는 처음 나왔기 때문에 자신의 앞에 같은 글자가 없습니다. 이는 -1로 표현합니다.
- n은 처음 나왔기 때문에 자신의 앞에 같은 글자가 없습니다. 이는 -1로 표현합니다.
- a는 자신보다 두 칸 앞에 a가 있습니다. 이는 2로 표현합니다.
- n도 자신보다 두 칸 앞에 n이 있습니다. 이는 2로 표현합니다.
- a는 자신보다 두 칸, 네 칸 앞에 a가 있습니다. 이 중 가까운 것은 두 칸 앞이고, 이는 2로 표현합니다.

따라서 최종 결과물은 [-1, -1, -1, 2, 2, 2]가 됩니다.

문자열 s이 주어질 때, 위와 같이 정의된 연산을 수행하는 함수 solution을 완성해주세요.

---

### 2-1. 문제 설명 - 제한사항

- 1 ≤ s의 길이 ≤ 10,000
  - s은 영어 소문자로만 이루어져 있습니다.

---

### 2-2. 문제 설명 - 입출력 예

| s        | result                  |
| -------- | ----------------------- |
| "banana" | [-1, -1, -1, 2, 2, 2]   |
| "foobar" | [-1, -1, 1, -1, -1, -1] |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1

지문과 같습니다.

입출력 예 #2

설명 생략

---

## 3. 문제 풀이

```javascript
// 6) 가장 가까운 글자의 위치 찾기
function getNearString(stringBucket, char) {
  const copyStringBucket = [...stringBucket];
  const idx = copyStringBucket.reverse().indexOf(char);
  return idx + 1;
}

function solution(s) {
  // 1) 문자열을 배열로 변환
  s = [...s];

  // 2) 가장 가까운 같은 글자를 찾은 글자를 저장하는 배열
  let stringBucket = [];

  // 3) 가장 가까운 같은 글자의 위치를 저장하는 배열
  let nearString = [];

  s.forEach((char) => {
    // 4) 글자가 처음 나타난 경우
    if (stringBucket.indexOf(char) === -1) nearString.push(-1);
    // 5) 글자가 앞에 나타난 경우 -> 가장 가까운 같은 글자의 위치 찾기
    else nearString.push(getNearString(stringBucket, char));

    stringBucket.push(char);
  });

  return nearString;
}
```

---

### 1) 문자열을 배열로 변환

```javascript
s = [...s];
```

구조분해할당을 사용하여 문자열을 배열로 바꾼다.

---

### 2) 가장 가까운 같은 글자를 찾은 글자를 저장하는 배열

```javascript
let stringBucket = [];
```

`stringBucket` 배열에는 문자열 `s`의 글자들 중 자신과 가장 가까운 같은 글자를 찾은 것들이 들어가게 된다. 해당 배열은 아래의 `forEach()` 메서드에서 1차적으로 어떠한 글자가 처음 나왔는지 아닌지를 구분하는 역할로 쓰이고, `getNearString()` 함수에서 어떠한 글자와 가장 가까운 같은 글자를 찾는데 쓰인다.

---

### 3) 가장 가까운 같은 글자의 위치를 저장하는 배열

```javascript
let nearString = [];
```

`nearString` 배열에는 가장 가까운 같은 글자를 찾은 것들이 들어가게 된다. 해당 배열을 최종적으로 반환되는 값이 된다. 또한 문자열 `s`와 길이가 같아진다.

---

### 4) 글자가 처음 나타난 경우

```javascript
if (stringBucket.indexOf(char) === -1) nearString.push(-1);
```

`stringBucket` 배열에 `char`(특정 문자)가 존재하지 않는다면 `indexOf()`메서드는 `-1`이 된다. 이때에는 문제의 조건에 맞게 `-1`를 `nearString` 배열에 넣는다.

---

### 5) 글자가 앞에 나타난 경우 -> 가장 가까운 같은 글자의 위치 찾기

```javascript
else nearString.push(getNearString(stringBucket, char));
```

`char`(특정 문자)가 `stringBucket` 배열에 존재하면 `getNearString()` 함수가 실행된다. 해당 함수는 두 번째 인자로 받은 `char`(특정 문자)와 같고 가장 가까이 위치한 문자와의 거리를 반환한다. 더 자세한 내용은 아래에서 다룬다.

---

### 6) 가장 가까운 글자의 위치 찾기

```javascript
function getNearString(stringBucket, char) {
  const copyStringBucket = [...stringBucket];
  const idx = copyStringBucket.reverse().indexOf(char);
  return idx + 1;
}
```

`getNearString()` 함수는 두 개의 인자를 받는다. 첫 번째 인자로는 `stringBucket`으로 지금까지 조건에 맞는 위치를 찾은 문자들이 순서대로 담긴 배열이다. 두 번째 인자는 비교하기 위해 전달된 특정 문자이다.

해당 함수에서 첫 번째로 하는 것은 `stringBucket` 배열을 복사하는 것이다. 배열을 복사하는 이유는 `stringBucket`를 그대로 가져와 `reverse()` 메서드를 사용하면 원본 배열이 바뀌기 때문이다.

> 공부할 내용: 얕은 복사와 깊은 복사

두 번째로 하는 것은 `reverse()` 메서드를 사용하여 배열의 순서를 바꾸는 것이다. 그 이유는 새로운 문자열은 뒤에서 부터 차곡 차곡 쌓이기 때문에 앞이 아니라 뒤에서 부터 찾아야 가장 가까고 같은 문자를 찾을 수 있기 때문이다.

이후에는 `indexOf()` 메서드를 사용하여 인덱스값을 찾는 것이다. 최종적으로는 해당 인덱스값에 1를 더한 값을 반환하면 된다.

---

### 결과

![programmers_nearset_identical_letter_result1](/image/CodingTest/programmers_nearset_identical_letter/programmers_nearset_identical_letter_result1.png)

---

## 4. 다른 풀이

좀더 간단한 풀이가 없을까? 다른 사람들의 풀이를 참고 하던중 문자열의 위치를 저장한 어떠한 저장소가 있으면 좋을 거 같다는 생각이 들었다. 해당 저장소를 `map` 객체로 관리하고 같은 글자의 위치는 인덱스의 차이로 계산해보자.

```javascript
function solution(s) {
  const map = new Map();

  const nearPosition = s.split('').map((str, idx) => {
    const position = map.has(str) ? idx - map.get(str) : -1;
    map.set(str, idx);
    return position;
  });

  return nearPosition;
}
```

---

### 결과

전체적으로 빨라진 결과를 볼 수 있다.

![programmers_nearset_identical_letter_result2](/image/CodingTest/programmers_nearset_identical_letter/programmers_nearset_identical_letter_result2.png)

---

## 5. Conclusion

> 오랜만에 코딩테스트 문제를 풀고 풀이를 정리해 보았다. 사실 너무 쉽게 풀린 문제들은 따로 정리를 하지 않고 있다. 해당 문제도 어렵지 않았지만 다른 사람들의 풀이를 보며 좋은 풀이법이 떠올라 정리를 하였다. 취업을 위해선 프로젝트도 중요하지만 코딩테스트도 중요하다는 것을 잊지말고 꾸준히 하도록 하자. 강의도 꾸준히 듣고 파이팅이다.

---

📅 2023-01-02
