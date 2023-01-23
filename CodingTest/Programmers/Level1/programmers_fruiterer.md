# 과일 장수

## 1. 개요

- 프로그래머스
- Lv.1
- 연습문제
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/135808)

---

## 2. 문제 설명

과일 장수가 사과 상자를 포장하고 있습니다. 사과는 상태에 따라 1점부터 k점까지의 점수로 분류하며, k점이 최상품의 사과이고 1점이 최하품의 사과입니다. 사과 한 상자의 가격은 다음과 같이 결정됩니다.

- 한 상자에 사과를 m개씩 담아 포장합니다.
- 상자에 담긴 사과 중 가장 낮은 점수가 p (1 ≤ p ≤ k)점인 경우, 사과 한 상자의 가격은 p \* m 입니다.

과일 장수가 가능한 많은 사과를 팔았을 때, 얻을 수 있는 최대 이익을 계산하고자 합니다.(사과는 상자 단위로만 판매하며, 남는 사과는 버립니다)

예를 들어, k = 3, m = 4, 사과 7개의 점수가 [1, 2, 3, 1, 2, 3, 1]이라면, 다음과 같이 [2, 3, 2, 3]으로 구성된 사과 상자 1개를 만들어 판매하여 최대 이익을 얻을 수 있습니다.

- (최저 사과 점수) x (한 상자에 담긴 사과 개수) x (상자의 개수) = 2 x 4 x 1 = 8

사과의 최대 점수 k, 한 상자에 들어가는 사과의 수 m, 사과들의 점수 score가 주어졌을 때, 과일 장수가 얻을 수 있는 최대 이익을 return하는 solution 함수를 완성해주세요.

### 2-1. 문제 설명 - 제한사항

- 3 ≤ k ≤ 9
- 3 ≤ m ≤ 10
- 7 ≤ score의 길이 ≤ 1,000,000
- 1 ≤ score[i] ≤ k
- 이익이 발생하지 않는 경우에는 0을 return 해주세요.

### 2-2. 문제 설명 - 입출력 예

| k   | m   | score                                | result |
| --- | --- | ------------------------------------ | ------ |
| 3   | 4   | [1, 2, 3, 1, 2, 3, 1]                | 8      |
| 4   | 3   | [4, 1, 2, 2, 4, 4, 4, 4, 1, 2, 4, 2] | 33     |

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1

- 문제의 예시와 같습니다.

입출력 예 #2

- 다음과 같이 사과 상자를 포장하여 모두 팔면 최대 이익을 낼 수 있습니다.

| 사과 상자 | 가격       |
| --------- | ---------- |
| [1, 1, 2] | 1 x 3 = 3  |
| [2, 2, 2] | 2 x 3 = 6  |
| [4, 4, 4] | 4 x 3 = 12 |
| [4, 4, 4] | 4 x 3 = 12 |

따라서 (1 x 3 x 1) + (2 x 3 x 1) + (4 x 3 x 2) = 33을 return합니다.

---

## 3. 문제 풀이

```javascript
function solution(k, m, score) {
  // 1) 내림차순으로 정렬
  score.sort((a, b) => b - a);

  // 2) 마지막 요소의 인댁스와 현재 바라보고 있는 인덱스를 바탕으로 반복문 실행하기
  const lastIndex = score.length - 1;
  let idx = m - 1;
  let result = 0;
  while (idx <= lastIndex) {
    // 3) 결과에 현재 사과 상자의 가격 더하기
    result += score[idx] * m;
    // 4) 다음 사과 상자의 마지막 인덱스 구하기
    idx += m;
  }

  return result;
}
```

### 1) 내림차순으로 정렬

```javascript
score.sort((a, b) => b - a);
```

최대 이익을 남기기 위해선 점수가 높은 사과 순으로 사과 상자에 담아야 하기 때문에 내림차순으로 정렬을 한다.

### 2) 마지막 요소의 인댁스와 현재 바라보고 있는 인덱스를 바탕으로 반복문 실행하기

```javascript
const lastIndex = score.length - 1;
let idx = m - 1;
while (idx <= lastIndex) {}
```

`idx`의 초기값은 한 상자에 담을 수 있는 사과 개수에 1를 뺀 수이다. 이는 배열의 입장에서 봤을 때 첫 상자에 담을 수 있는 사과들 중 마지막 요소에 해당된다. `idx`는 반복문 내에서 점점 커진다.

`lastIndex`는 단순히 `idx`와 비교하기 위함이다. `lastIndex`를 넘어서는 사과 상자에 `m`개의 사과를 담을 수 없기 때문이다.

### 3) 결과에 현재 사과 상자의 가격 더하기

```javascript
result += score[idx] * m;
```

사과의 점수가 담긴 배열인 `score`의 `idx`번 째 요소의 점수를 `m`으로 곱한 값을 `result`에 추가한다.

### 4) 다음 사과 상자의 마지막 인덱스 구하기

```javascript
idx += m;
```

다음 사과 상자의 가격을 구하기 위해서 `idx`에 한 상자에 담을 수 있는 사과 개수인 `m`만큼을 더한다.

### 결과

![programmers_fruiterer_result](/image/CodingTest/programmers_fruiterer/programmers_fruiterer_result.png)

---

## 4. 이전 문제 풀이

```javascript
function solution(k, m, score) {
  score.sort((a, b) => b - a);

  let result = 0;
  while (score.length >= m) {
    const box = score.splice(0, m);
    const minScore = Math.min(...box);
    result += minScore * m;
  }

  return result;
}
```

이전 문제 풀이에서는 `splice()` 메서드를 사용하여 앞에서 부터 `m`개 씩 잘라 한 상자의 가격을 구했다. 하지만 이런 풀이는 아래와 같은 실행 결과가 나타났다.

![programmers_fruiterer_fail](/image/CodingTest/programmers_fruiterer/programmers_fruiterer_fail.png)

이유는 `score`의 길이에 따라 실행되는 속도가 달라지기 때문이다. 즉, `O(n^2)`만큼의 시간으로는 시간 초과가 나타나 실패를 한다. 이를 해결하기 위해 `splice()` 메서드를 사용하지 않고 단지 배열에 접근하는 방법으로 바꾸었다. 이렇게 되면 시간 복잡도는 `O(n)`이 되므로 전체적인 실행 속도도 빨라지고 시간 초과로 통과하지 못했던 테스트도 통과할 수 있게 되었다.

---

## 5. Conclusion

> 해당 문제는 어렵지 않는 문제였다. 하지만 문제 풀이에 넣은 이유는 시간 복잡도에 대한 내용 때문이었다. 만약 처음 공부하는 입장이었다면 왜 시간 초과가 났는지 고민하면서 많은 시간을 보냈지만 시간 복잡도를 계산하고 이를 줄일 수 있는 방법을 고민하니 쉽게 풀이가 되었다. 처음 코딩 테스트 문제를 풀고, 알고리즘, 자료구조를 공부했었던 작년 7월 보다 많이 성장한 느낌이 들어 뿌듯하다. 물론 쉬운 문제이긴 하지만 말이다. 앞으로 더 발전하고 성정하자.

---

📅 2023-01-08