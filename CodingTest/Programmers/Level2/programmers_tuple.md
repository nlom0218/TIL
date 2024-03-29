# 튜플

## 1. 개요

- 프로그래머스
- Lv.2
- 2019 카카오 개발자 겨울 인턴십
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/64065)

---

## 2. 문제 설명

셀수있는 수량의 순서있는 열거 또는 어떤 순서를 따르는 요소들의 모음을 튜플(tuple)이라고 합니다. n개의 요소를 가진 튜플을 n-튜플(n-tuple)이라고 하며, 다음과 같이 표현할 수 있습니다.

- (a1, a2, a3, ..., an)

튜플은 다음과 같은 성질을 가지고 있습니다.

1. 중복된 원소가 있을 수 있습니다. ex : (2, 3, 1, 2)
2. 원소에 정해진 순서가 있으며, 원소의 순서가 다르면 서로 다른 튜플입니다. ex : (1, 2, 3) ≠ (1, 3, 2)
3. 튜플의 원소 개수는 유한합니다.

원소의 개수가 n개이고, 중복되는 원소가 없는 튜플 (a1, a2, a3, ..., an)이 주어질 때(단, a1, a2, ..., an은 자연수), 이는 다음과 같이 집합 기호 '{', '}'를 이용해 표현할 수 있습니다.

- {{a1}, {a1, a2}, {a1, a2, a3}, {a1, a2, a3, a4}, ... {a1, a2, a3, a4, ..., an}}

예를 들어 튜플이 (2, 1, 3, 4)인 경우 이는

- {{2}, {2, 1}, {2, 1, 3}, {2, 1, 3, 4}}

와 같이 표현할 수 있습니다. 이때, 집합은 원소의 순서가 바뀌어도 상관없으므로

- {{2}, {2, 1}, {2, 1, 3}, {2, 1, 3, 4}}
- {{2, 1, 3, 4}, {2}, {2, 1, 3}, {2, 1}}
- {{1, 2, 3}, {2, 1}, {1, 2, 4, 3}, {2}}

는 모두 같은 튜플 (2, 1, 3, 4)를 나타냅니다.

특정 튜플을 표현하는 집합이 담긴 문자열 s가 매개변수로 주어질 때, s가 표현하는 튜플을 배열에 담아 return 하도록 solution 함수를 완성해주세요.

---

### 2-1. 문제 설명 - 제한사항

- s의 길이는 5 이상 1,000,000 이하입니다.
- s는 숫자와 '{', '}', ',' 로만 이루어져 있습니다.
- 숫자가 0으로 시작하는 경우는 없습니다.
- s는 항상 중복되는 원소가 없는 튜플을 올바르게 표현하고 있습니다.
- s가 표현하는 튜플의 원소는 1 이상 100,000 이하인 자연수입니다.
- return 하는 배열의 길이가 1 이상 500 이하인 경우만 입력으로 주어집니다.

---

### 2-2. 문제 설명 - 입출력 예

| s                               | result       |
| ------------------------------- | ------------ |
| "{{2},{2,1},{2,1,3},{2,1,3,4}}" | [2, 1, 3, 4] |
| "{{1,2,3},{2,1},{1,2,4,3},{2}}" | [2, 1, 3, 4] |
| "{{20,111},{111}}"              | [111, 20]    |
| "{{123}}"                       | [123]        |
| "{{4,2,3},{3},{2,3,4,1},{2,3}}" | [3, 2, 4, 1] |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1

문제 예시와 같습니다.

입출력 예 #2

문제 예시와 같습니다.

입출력 예 #3

(111, 20)을 집합 기호를 이용해 표현하면 {{111}, {111,20}}이 되며, 이는 {{20,111},{111}}과 같습니다.

입출력 예 #4

(123)을 집합 기호를 이용해 표현하면 {{123}} 입니다.

입출력 예 #5

(3, 2, 4, 1)을 집합 기호를 이용해 표현하면 {{3},{3,2},{3,2,4},{3,2,4,1}}이 되며, 이는 {{4,2,3},{3},{2,3,4,1},{2,3}}과 같습니다.

---

## 3. 문제 풀이

```javascript
function solution(s) {
  // 1) 문자열을 배열로 바꾸기
  s = s.slice(1, s.length - 2);
  const tuple_arr = s
    .split("},")
    .map((item) => {
      item = item.slice(1);
      return item.split(",");
    })
    .sort((a, b) => a.length - b.length);

  let answer = [];
  // 2) 반복문을 순회하면서 answer 배열에 요소 추가하기
  tuple_arr.forEach((item) => {
    item = item.filter((num) => !answer.includes(num));
    answer.push(...item);
  });

  // 3) 문자열인 요소를 숫자로 바꾸어 리턴하기
  return answer.map((item) => Number(item));
}
```

---

### 1) 문자열을 배열로 바꾸기

```javascript
s = s.slice(1, s.length - 2);
const tuple_arr = s
  .split("},")
  .map((item) => {
    item = item.slice(1);
    return item.split(",");
  })
  .sort((a, b) => a.length - b.length);
```

매개변수로 주어진 문자열 `s`를 배열로 바꾸는 과정이다. 문자열 `s`는 아래와 같은 형식으로 받는다.

- {{2}, {2, 1}, {2, 1, 3}, {2, 1, 3, 4}}

우선 맨 앞의 `{` 와 맨 뒤의 `}}` 를 우선 잘라내기 위해 `slice()` 메서드를 사용한다. 해당 과정을 통해 문자열 `s`는 아래와 같이 바뀐다.

- {2}, {2, 1}, {2, 1, 3}, {2, 1, 3, 4

그 후 문자열을 배열로 바꿔주는 메서드인 `split()` 메서드를 사용한다.
인자로는 `},` 를 넘겨 `},`를 기준으로 배열을 만든다. 해당 과정을 통해 아래와 같은 배열로 바뀐다.

- [[{2], [{2, 1] [{2, 1, 3] [{2, 1, 3, 4]]

마지막으로 `map()` 메서드를 사용하여 각각의 요소의 첫 번째 문자열인 `{`를 제거하고 문자인 요소를 다시 `,`를 기준으로 나누어 배열을 만든다. 즉, 중첩 배열이 만들어 진다. 해당 과정을 통해 만들어진 배열은 아래와 같다.

- [[2], [2, 1], [2, 1, 3], [2, 1, 3, 4]]

마지막 과정은 `sort()` 메서드를 사용하여 요소의 길이가 작은 순서 대로 정렬을 하는 것이다. 예시에선 해당 순서대로 정렬이 되어 있기 때문에 배열 요소의 순서는 바꾸지 않는다.

---

### 2) 반복문을 순회하면서 answer 배열에 요소 추가하기

```javascript
tuple_arr.forEach((item) => {
  item = item.filter((num) => !answer.includes(num));
  answer.push(...item);
});
```

어떤 `튜플` 이었는지 구하는 과정이다. 위의 과정에서 만든 `truple_arr`를 순회를 돌며 `answer` 배열에 요소를 추가한다.

이때 이미 `answer` 배열에 포함되어 있는 요소는 추가하지 않는다.

이렇게 되면 `튜플`을 알 수 있지만 튜플의 요소가 문자열이기 때문에 마지막으로 아래의 과정을 거쳐야 한다.

---

### 3) 문자열인 요소를 숫자로 바꾸어 리턴하기

```javascript
return answer.map((item) => Number(item));
```

`map()` 메서드를 돌며 각각의 요소를 숫자로 변환한 후 반환한다.

---

## 4. Conclusion

> 정말 오랜만의 코딩 테스트 문제 정리이다. 코딩 테스트도 재밌는 학습이지만 현재로선 우선순위로는 우테코가 있기 때문에 우테코에 집중하여 준비하고 있다. 그래도 스터디를 통해 간간히 문제를 풀고 정리를 하니 오랜만에 만난 친구처럼 정겨웠다. 물론, 쉽게 푼 문제여서 더욱 그런 듯 하다:) 그리고 오늘 푼 문제는 부담감이 덜해 가벼운 마음가짐으로 풀 수 있어 더욱 쉽고 간결하게 풀 수 있었다. 다만 아쉬운 점은 문자열을 배열로 바꿀 때 정규식을 사용하면 더욱 편리할 것 같은 느낌이었지만 아직 정규식을 자유자재로 사용할 수 없어 메서드를 사용한 것이다. 정규식에 대한 지식을 한 번에 쌓는다는 느낌이 아니라 필요할 때 게속해서 보면서 스스로 익숙해질 수 있도록 노력하자.

---

📅 2022-10-22
