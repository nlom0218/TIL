# 실패율

## 1. 개요

- 프로그래머스
- Lv.1
- 2019 KAKAO BLIND RECRUITMENT
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42889)

---

## 2. 문제 설명

![문제 설명 - 이미지](/image/CodingTest/programmers_failure_rate/programmers_failure_rate_cover.png)

슈퍼 게임 개발자 오렐리는 큰 고민에 빠졌다. 그녀가 만든 프랜즈 오천성이 대성공을 거뒀지만, 요즘 신규 사용자의 수가 급감한 것이다. 원인은 신규 사용자와 기존 사용자 사이에 스테이지 차이가 너무 큰 것이 문제였다.

이 문제를 어떻게 할까 고민 한 그녀는 동적으로 게임 시간을 늘려서 난이도를 조절하기로 했다. 역시 슈퍼 개발자라 대부분의 로직은 쉽게 구현했지만, 실패율을 구하는 부분에서 위기에 빠지고 말았다. 오렐리를 위해 실패율을 구하는 코드를 완성하라.

- 실패율은 다음과 같이 정의한다.
  - 스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수 / 스테이지에 도달한 플레이어 수

전체 스테이지의 개수 N, 게임을 이용하는 사용자가 현재 멈춰있는 스테이지의 번호가 담긴 배열 stages가 매개변수로 주어질 때, 실패율이 높은 스테이지부터 내림차순으로 스테이지의 번호가 담겨있는 배열을 return 하도록 solution 함수를 완성하라.

---

### 2-1. 문제 설명 - 제한사항

- 스테이지의 개수 N은 1 이상 500 이하의 자연수이다.
- stages의 길이는 1 이상 200,000 이하이다.
- stages에는 1 이상 N + 1 이하의 자연수가 담겨있다.
  - 각 자연수는 사용자가 현재 도전 중인 스테이지의 번호를 나타낸다.
  - 단, N + 1 은 마지막 스테이지(N 번째 스테이지) 까지 클리어 한 사용자를 나타낸다.
- 만약 실패율이 같은 스테이지가 있다면 작은 번호의 스테이지가 먼저 오도록 하면 된다.
- 스테이지에 도달한 유저가 없는 경우 해당 스테이지의 실패율은 0 으로 정의한다.

---

### 2-2. 문제 설명 - 입출력 예

| N   | stages                   | result      |
| --- | ------------------------ | ----------- |
| 5   | [2, 1, 2, 6, 2, 4, 3, 3] | [3,4,2,1,5] |
| 4   | [4,4,4,4,4]              | [4,1,2,3]   |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1

1번 스테이지에는 총 8명의 사용자가 도전했으며, 이 중 1명의 사용자가 아직 클리어하지 못했다. 따라서 1번 스테이지의 실패율은 다음과 같다.

- 1 번 스테이지 실패율 : 1/8

2번 스테이지에는 총 7명의 사용자가 도전했으며, 이 중 3명의 사용자가 아직 클리어하지 못했다. 따라서 2번 스테이지의 실패율은 다음과 같다.

- 2 번 스테이지 실패율 : 3/7

마찬가지로 나머지 스테이지의 실패율은 다음과 같다.

- 3번 스테이지 실패율 : 2/4
- 4번 스테이지 실패율 : 1/2
- 5번 스테이지 실패율 : 0/1

각 스테이지의 번호를 실패율의 내림차순으로 정렬하면 다음과 같다.

- [3,4,2,1,5]

입출력 예 #2

모든 사용자가 마지막 스테이지에 있으므로 4번 스테이지의 실패율은 1이며 나머지 스테이지의 실패율은 0이다.

- [4,1,2,3]

---

## 3. 문제 풀이

```javascript
function solution(N, stages) {
  // 1) 맵 객체를 활용하여 각각의 스테이지를 도전한 사용자의 수 저장하기
  const map = new Map();
  stages.forEach((item) => {
    for (let i = 1; i <= item; i++) {
      map.set(i, (map.get(i) | 0) + 1);
    }
  });

  // 2) stage와 rate로 이루어진 객체를 요소로 가지는 배열 만들기
  const arr = [];
  for (let i = 1; i <= N; i++) {
    const user_num = stages.filter((item) => item === i).length;
    arr.push({ stage: i, rate: user_num / map.get(i) });
  }

  // 3) 실패율에 따라 내림차순으로 정렬하기
  return arr
    .sort((a, b) => {
      if (a.rate === b.rate) return a.stage - b.stage;
      return b.rate - a.rate;
    })
    .map((item) => item.stage);
}
```

---

### 1) 맵 객체를 활용하여 각각의 스테이지를 도전한 사용자의 수 구하기

```javascript
const map = new Map();
stages.forEach((item) => {
  for (let i = 1; i <= item; i++) {
    map.set(i, (map.get(i) | 0) + 1);
  }
});
```

`stages` 배열은 각각의 사용자가 현재 도전하고 있는 스테이지를 요소로 가지고 있다. 각각의 요소의 수 만큼
반복문을 실행하여 맵 객체를 수정한다.

만약 사용자가 5단계에 있다면 1단계부터 5단계까지 모두 도전을 하였으면 키가 1에서 5까지인 요소의 값을
1증가시킨다. 여기서 요소의 키는 스테이지를 뜻하고 값은 해당 스테이지를 도전한 사용자의 수를 뜻한다.

이렇게 일단 각각의 스테이지를 도전한 사용자의 수를 구한다.

---

### 2) stage와 rate로 이루어진 객체를 요소로 가지는 배열 만들기

```javascript
const arr = [];
for (let i = 1; i <= N; i++) {
  const user_num = stages.filter((item) => item === i).length;
  arr.push({ stage: i, rate: user_num / map.get(i) });
}
```

`N`은 전체 스테이지의 수 이다. 반복문을 실행하여 각각의 스테이지의 실패율을 구하는 과정이다.

반복문 내의 변수에 대한 설명은 아래와 같다.

- `i`: 스테이지의 순서를 뜻한다.
- `user_num`: 해당 스테이지를 아직 통과하지 못한 사용자의 수를 뜻한다.
- `map.get(i)`: 해당 스테이지에 도전한 모든 사용자의 수를 뜻한다.
- `user_num / map.get(i)`: 도전한 전체 사용자 중 아직 통과하지 못한 사용자의 비율이다. 해당 비율이 바로 실패율이다.

---

### 3) 실패율에 따라 내림차순으로 정렬하기

```javascript
return arr
  .sort((a, b) => {
    if (a.rate === b.rate) return a.stage - b.stage;
    return b.rate - a.rate;
  })
  .map((item) => item.stage);
```

마지막으로 실패율을 내림차순으로 정렬하여 반환을 한다. 이때 실패율을 반환하는 것이 아니라
스테이지 번호를 반화해야 하기 때문에 마지막엔 `Array.map()` 메서드를 사용하여 `stage`의 값만
가져온다.

하지만 만약 실패율이 같다면 작은 번호의 스테이지가 먼저(오름차순) 오도록 한다. 정렬하는 과정은 `Array.sort()` 메서드를
사용하여 구현하였다.

---

### 결과

![programmers_failure_rate_result1](/image/CodingTest/programmers_failure_rate/programmers_failure_rate_result1.png)

---

## 4. Refactoring

풀다보니 굳이 Map(맵) 객체를 사용하지 않고 구현하고 싶었다.

```javascript
function solution(N, stages) {
  const challengers_num = new Array(N + 1).fill(0);
  stages.forEach((item, index) => {
    for (let i = 0; i < item; i++) {
      challengers_num[i] += 1;
    }
  });
  const failure_rate = [];
  for (let i = 1; i <= N; i++) {
    const user_num = stages.filter((item) => item === i).length;
    failure_rate.push({ stage: i, rate: user_num / challengers_num[i - 1] });
  }
  return failure_rate
    .sort((a, b) => {
      if (a.rate === b.rate) return a.stage - b.stage;
      return b.rate - a.rate;
    })
    .map((item) => item.stage);
}
```

맵(Map) 객체를 사용했느냐 아니면 배열을 사용했느냐에 따른 차이이다.

---

### 결과

![programmers_failure_rate_result2](/image/CodingTest/programmers_failure_rate/programmers_failure_rate_result2.png)
