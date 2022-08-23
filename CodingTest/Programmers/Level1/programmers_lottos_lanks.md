# 로또의 최고 순위와 최저 순위

## 1. 개요

- 프로그래머스
- Lv.1
- 2021 Dev-Matching
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/77484)

---

## 2. 문제 설명

로또 6/45(이하 '로또'로 표기)는 1부터 45까지의 숫자 중 6개를 찍어서 맞히는 대표적인 복권입니다. 아래는 로또의 순위를 정하는 방식입니다.

| 순위    | 당첨 내용            |
| ------- | -------------------- |
| 1       | 6개 번호가 모두 일치 |
| 2       | 5개 번호가 일치      |
| 3       | 4개 번호가 일치      |
| 4       | 3개 번호가 일치      |
| 5       | 2개 번호가 일치      |
| 6(낙첨) | 그 외                |

로또를 구매한 민우는 당첨 번호 발표일을 학수고대하고 있었습니다. 하지만, 민우의 동생이 로또에 낙서를 하여, 일부 번호를 알아볼 수 없게 되었습니다. 당첨 번호 발표 후, 민우는 자신이 구매했던 로또로 당첨이 가능했던 최고 순위와 최저 순위를 알아보고 싶어 졌습니다.

알아볼 수 없는 번호를 0으로 표기하기로 하고, 민우가 구매한 로또 번호 6개가 44, 1, 0, 0, 31 25라고 가정해보겠습니다. 당첨 번호 6개가 31, 10, 45, 1, 6, 19라면, 당첨 가능한 최고 순위와 최저 순위의 한 예는 아래와 같습니다.

| 당첨 번호      | 31  | 10   | 45  | 1   | 6      | 19                 | 결과 |
| -------------- | --- | ---- | --- | --- | ------ | ------------------ | ---- |
| 최고 순위 번호 | 31  | 0→10 | 44  | 1   | 0→6 25 | 4개 번호 일치, 3등 |
| 최저 순위 번호 | 31  | 0→11 | 44  | 1   | 0→7 25 | 2개 번호 일치, 5등 |

- 순서와 상관없이, 구매한 로또에 당첨 번호와 일치하는 번호가 있으면 맞힌 걸로 인정됩니다.
- 알아볼 수 없는 두 개의 번호를 각각 10, 6이라고 가정하면 3등에 당첨될 수 있습니다.
  - 3등을 만드는 다른 방법들도 존재합니다. 하지만, 2등 이상으로 만드는 것은 불가능합니다.
- 알아볼 수 없는 두 개의 번호를 각각 11, 7이라고 가정하면 5등에 당첨될 수 있습니다.
  - 5등을 만드는 다른 방법들도 존재합니다. 하지만, 6등(낙첨)으로 만드는 것은 불가능합니다.

민우가 구매한 로또 번호를 담은 배열 lottos, 당첨 번호를 담은 배열 win_nums가 매개변수로 주어집니다. 이때, 당첨 가능한 최고 순위와 최저 순위를 차례대로 배열에 담아서 return 하도록 solution 함수를 완성해주세요.

---

### 2-1. 문제 설명 - 제한사항

- lottos는 길이 6인 정수 배열입니다.
- lottos의 모든 원소는 0 이상 45 이하인 정수입니다.
  - 0은 알아볼 수 없는 숫자를 의미합니다.
  - 0을 제외한 다른 숫자들은 lottos에 2개 이상 담겨있지 않습니다.
  - lottos의 원소들은 정렬되어 있지 않을 수도 있습니다.
- win_nums은 길이 6인 정수 배열입니다.
- win_nums의 모든 원소는 1 이상 45 이하인 정수입니다.
  - win_nums에는 같은 숫자가 2개 이상 담겨있지 않습니다.
  - win_nums의 원소들은 정렬되어 있지 않을 수도 있습니다.

---

### 2-2. 문제 설명 - 입출력 예

| lottos                | win_nums                 | result |
| --------------------- | ------------------------ | ------ |
| [44, 1, 0, 0, 31, 25] | [31, 10, 45, 1, 6, 19]   | [3, 5] |
| [0, 0, 0, 0, 0, 0]    | [38, 19, 20, 40, 15, 25] | [1, 6] |
| [45, 4, 35, 20, 3, 9] | [20, 9, 3, 45, 4, 35]    | [1, 1] |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1  
문제 예시와 같습니다.

입출력 예 #2  
알아볼 수 없는 번호들이 아래와 같았다면, 1등과 6등에 당첨될 수 있습니다.

| 당첨 번호      | 38   | 19   | 20   | 40   | 15   | 25   | 결과               |
| -------------- | ---- | ---- | ---- | ---- | ---- | ---- | ------------------ |
| 최고 순위 번호 | 0→38 | 0→19 | 0→20 | 0→40 | 0→15 | 0→25 | 6개 번호 일치, 1등 |
| 최저 순위 번호 | 0→21 | 0→22 | 0→23 | 0→24 | 0→26 | 0→27 | 0개 번호 일치, 6등 |

입출력 예 #3  
민우가 구매한 로또의 번호와 당첨 번호가 모두 일치하므로, 최고 순위와 최저 순위는 모두 1등입니다.

---

## 3. 문제 풀이

```javascript
function solution(lottos, win_nums) {
  // 1) 알 수 있는 로또 번호와 알아볼 수 없는 로또 번호
  const know_lottos = lottos.filter((item) => item !== 0);
  const dont_lottos = lottos.filter((item) => item === 0);

  // 2) 알 수 있는 로또 번호에서 당첨된 번호
  const my_win_nums = know_lottos.filter((item) => win_nums.includes(item));

  // 3) 최저 순위 구하기
  const min_lank = 7 - my_win_nums.length > 5 ? 6 : 7 - my_win_nums.length;

  // 4) 최고 순위 구하기
  const max_lank =
    7 - my_win_nums.length - dont_lottos.length > 5
      ? 6
      : 7 - my_win_nums.length - dont_lottos.length;

  return [max_lank, min_lank];
}
```

---

### 1) 알 수 있는 로또 번호와 알아볼 수 없는 로또 번호

```javascript
const know_lottos = lottos.filter((item) => item !== 0);
const dont_lottos = lottos.filter((item) => item === 0);
```

`lottos` 배열에에는 0부터 45까지의 숫자가 있다. 이 중 0이 의미하는 것은 알아볼 수 없는 번호이다.
`Array.filter()` 메서드를 사용하여 배열의 요소가 0인 것과 0이 아닌 것을 구분한다.

---

### 2) 알 수 있는 로또 번호에서 당첨된 번호

```javascript
const my_win_nums = know_lottos.filter((item) => win_nums.includes(item));
```

로또의 순위는 당첨 번호의 수에 따라 달라진다. 먼저 알 수 있는 로또 번호에서 당첨된 번호를 찾는다.

알 수 있는 로또 번호들 중 얼마만큼 `win_nums` 배열에 포함되어 있는지 확인하면 된다.
이를 위해 `Array.filter()` 메서드와 `Array.includes()` 메서드를 사용하였다.

---

### 3) 최저 순위 구하기

```javascript
const min_lank = 7 - my_win_nums.length > 5 ? 6 : 7 - my_win_nums.length;
```

최저 순위는 알아볼 수 없는 번호들이 모두 낙첨된 경우이다.
즉, 당첨된 번호의 개수는 `2) 알 수 있는 로또 번호에서 당첨된 번호`에서 구한 `my_win_nums` 배열의 `length`이다.

여기서 하나 더 생각을 해야할 것이 있다.

- `my_win_nums.length`가 2이상이면 7에서 해당 값을 빼 최저 순위를 구한다.
- `my_win_nums.length`가 1또는 0이라면 낙첨인 6등을 최저 순위로 해야한다.

---

### 4) 최고 순위 구하기

```javascript
const max_lank =
  7 - my_win_nums.length - dont_lottos.length > 5
    ? 6
    : 7 - my_win_nums.length - dont_lottos.length;
```

최고 순위는 알아볼 수 없는 번호들이 모두 당첨된 번호인 경우이다.
`min_lank`에서 구한 최저 순위에 `dont_lottos.length`를 더 빼 순위를 구한다.

여기도 마찬가지로 뺀 값이 5보다 크면 낙첨인 6등을 최저 순위로 한다.

---

### 결과

![programmers_lottos_lanks_result](/image/CodingTest/programmers_lottos_lanks/programmers_lottos_lanks_result.png)

---

## 4. Refactoring

전체적인 흐름은 같다. `min_lank`와 `max_lank`를 구하는 것을 가독성있도록 바꾸었다.
이를 위해 `max_win_nums`라는 새로운 변수도 선언하여 당첨될 수 있는 가장 많은 번호의 수를 할당하였다.

```javascript
function solution(lottos, win_nums) {
  const know_lottos = lottos.filter((item) => item !== 0);
  const dont_lottos = lottos.filter((item) => item === 0);

  const my_win_nums = know_lottos.filter((item) =>
    win_nums.includes(item)
  ).length;
  const max_win_nums = my_win_nums + dont_lottos.length;

  const min_lank = my_win_nums === 0 ? 6 : 7 - my_win_nums;
  const max_lank = max_win_nums === 0 ? 6 : 7 - max_win_nums;

  return [max_lank, min_lank];
}
```

### 결과

비슷한 실행 결과를 보인다.

![programmers_lottos_lanks_result](/image/CodingTest/programmers_lottos_lanks/programmers_lottos_lanks_result2.png)

---

## 5. 다른 사람 풀이

가장 많은 좋아요를 받은 풀이이다.

```javascript
function solution(lottos, win_nums) {
  const rank = [6, 6, 5, 4, 3, 2, 1];

  let minCount = lottos.filter((v) => win_nums.includes(v)).length;
  let zeroCount = lottos.filter((v) => !v).length;

  const maxCount = minCount + zeroCount;

  return [rank[maxCount], rank[minCount]];
}
```

`minCount`, `zeroCount`, `maxCount`를 구하는 방법은 내가 했던 방법과 같다. 다만 중간 과정을 생략하여
의미없는 변수를 만드는 과정이 더 생략되었을 뿐이다.

사실 위에서도 `zeroCount`와 `maxCount`를 하나로 묶어도 된다.(아래코드참고)

```javascript
let maxCount = lottos.filter((v) => !v).length + minCount;
```

그리고 `rank` 배열을 사용하여 인덱스를 통해 순위를 찾는 것이 기발한 아이디어로 생각한다.

## 6. Conclusion

> 이번 문제도 어렵지 않게 풀었다. 마지막 순위를 정할 때만 살짝 고민이 있었을 뿐 순조롭게 문제를 풀었다.
> 그리고 항상 다른 사람의 풀이에서 좋은 아이디어를 하나씩 가져가는 것 같다.👍

---

📅 2022-08-19
