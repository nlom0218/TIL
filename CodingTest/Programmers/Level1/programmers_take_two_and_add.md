# 두 개 뽑아서 더하기

---

## 1. 개요

- 프로그래머스
- Lv.1
- 월간 코드 챌린지 시즌1
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/68644)

---

## 2. 문제 설명

정수 배열 numbers가 주어집니다. numbers에서 서로 다른 인덱스에 있는 두 개의 수를 뽑아 더해서 만들 수 있는 모든 수를 배열에 오름차순으로 담아 return 하도록 solution 함수를 완성해주세요.

---

### 2-1. 문제 설명 - 제한사항

- numbers의 길이는 2 이상 100 이하입니다.
  - numbers의 모든 수는 0 이상 100 이하입니다.

---

### 2-2. 문제 설명 - 입출력 예

| numbers     | result        |
| ----------- | ------------- |
| [2,1,3,4,1] | [2,3,4,5,6,7] |
| [5,0,2,7]   | [2,5,7,9,12]  |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1

- 2 = 1 + 1 입니다. (1이 numbers에 두 개 있습니다.)
- 3 = 2 + 1 입니다.
- 4 = 1 + 3 입니다.
- 5 = 1 + 4 = 2 + 3 입니다.
- 6 = 2 + 4 입니다.
- 7 = 3 + 4 입니다.
- 따라서 [2,3,4,5,6,7] 을 return 해야 합니다.

입출력 예 #2

- 2 = 0 + 2 입니다.
- 5 = 5 + 0 입니다.
- 7 = 0 + 7 = 5 + 2 입니다.
- 9 = 2 + 7 입니다.
- 12 = 5 + 7 입니다.
- 따라서 [2,5,7,9,12] 를 return 해야 합니다.

---

## 3. 문제 풀이

```javascript
function getCombinations(array, size) {
  function p(t, i) {
    if (t.length === size) {
      result.push(t);
      return;
    }
    if (i + 1 > array.length) {
      return;
    }
    p(t.concat(array[i]), i + 1);
    p(t, i + 1);
  }

  var result = [];
  p([], 0);
  return result;
}

function solution(numbers) {
  // 1) 요소를 2개 가지는 2차원 배열 만들기(조합)
  const combi = getCombinations(numbers, 2);

  // 2) 각각의 요소를 더해 모든 합의 경우의 수가 있는 배열 만들기
  const sum = combi.map((item) => item[0] + item[1]);

  // 3) 중복된 요소를 제거하고 오름차순으로 정렬 후 리턴하기
  return [...new Set(sum)].sort((a, b) => a - b);
}
```

---

### 1) 요소를 2개 가지는 2차원 배열 만들기(조합)

아주 잘 활용하고 있는 `getCombinations()`함수이다. 해당 함수를 통해 `numbers`의 요소를 가지는 길이 2의 2차원 배열을 만든다.

```javascript
const combi = getCombinations(numbers, 2);
```

예시

![programmers_take_two_and_add getCombinations](/image/CodingTest/programmers_take_two_and_add/programmers_take_two_and_add_getCombinations.png)

---

### 2) 각각의 요소를 더해 모든 합의 경우의 수가 있는 배열 만들기

```javascript
const sum = combi.map((item) => item[0] + item[1]);
```

`Array.map()`메서드를 사용하여 모든 조합의 합을 구한다. 여기서 `item`또한 배열이며 `numbers`배열의 요소를 2개씩 가지고 있다.

---

### 3) 중복된 요소를 제거하고 오름차순으로 정렬 후 리턴하기

2)의 과정을 통해 모든 조합의 합을 구했지만 중복되는 합이 있을 수 있다. 이를 제거하기 위해 `셋(Set)` 객체를 사용하였다.

그 후 `Array.sort()`메서드를 이용해 숫자를 오름차순으로 정렬하였다.

```javascript
return [...new Set(sum)].sort((a, b) => a - b);
```

---

### 결과

![programmers_take_two_and_add_result](/image/CodingTest/programmers_take_two_and_add/programmers_take_two_and_add_result1.png)

---

## 4. Refactoring

요소의 개수가 2인 조합이다. 그렇기 때문에 굳이 `getCombinations()`를 사용하지 않고 코드를 다시 구현하고 싶었다.

```javascript
function solution(numbers) {
  // 1) Set 객체 생성
  const sumSet = new Set();

  for (let i = 0; i < numbers.length - 1; i++) {
    for (let j = i + 1; j < numbers.length; j++) {
      // 2) 인덱스가 겹치지 않는 값을 더한후 sumSet에 추가하기
      sumSet.add(numbers[i] + numbers[j]);
    }
  }

  // 3) 배열로 바꾼 후 오름차순으로 정렬한 후 리턴하기
  return [...sumSet].sort((a, b) => a - b);
}
```

---

### 1) Set 객체 생성

```javascript
const sumSet = new Set();
```

`Set` 객체를 생성하여 아래의 반복문을 통해 `sumSet` 객체의 요소를 채우고자 한다.

---

### 2) 인덱스가 겹치지 않는 값을 더한후 sumSet에 추가하기

```javascript
sumSet.add(numbers[i] + numbers[j]);
```

반복문을 실행하게 되면 인덱스가 서로 겹치지 않는 두 개의 요소를 모두 가져올 수 있다. 이러한 요소들의 합을 `Set.add()`메서드를 통해 `sumSet` 객체에 추가한다. 이때 중복되는 요소는 하나만 추가한 후 더 이상 추가되지 않는다.

---

### 3) 배열로 바꾼 후 오름차순으로 정렬한 후 리턴하기

```javascript
return [...sumSet].sort((a, b) => a - b);
```

스프레드 연산자를 활용하여 `Set` 객체를 배열로 바꾼 후 `Array.sort()`메서드를 사용하여 오름차순으로 정렬한다.

---

### 결과

![programmers_take_two_and_add_result2](/image/CodingTest/programmers_take_two_and_add/programmers_take_two_and_add_result2.png)

실행 속도가 빨라진 것을 확인할 수 있다.

---

## 5. 다른 사람 풀이

가장 많은 좋아요와 유사한 풀이가 많은 문제를 가져왔다. 내가 리팩토링한 풀이와 중복되는 합을 언제 제거할지만 다를 뿐 전체적인 로직은 같다.

```javascript
function solution(numbers) {
  const temp = [];

  for (let i = 0; i < numbers.length; i++) {
    for (let j = i + 1; j < numbers.length; j++) {
      temp.push(numbers[i] + numbers[j]);
    }
  }

  const answer = [...new Set(temp)];

  return answer.sort((a, b) => a - b);
}
```

---

## 6. Conclusion

> 2주 동안 여러 문제를 풀었고 새로운 개념을 학습했기 때문에 이번 문제는 쉽게 풀 수 있었다. 물론 새로운 개념이 등장한다면 시간이 오래 걸리고 고민도 많이 하겠지만 어려움 없이 풀어서 뿌듯했다. `Map`, `Set`이 정말 웹 프로젝트에서는 사용하지 않아서 잘 몰랐는데 코딩 테스트를 풀며 공부하다보니 유용하게 사용하는 곳이 많은 듯 하다. 아마... 웹 프로젝트에서도 많이 사용되겠지?

---

[👆](#두-개-뽑아서-더하기)

📅 2022-08-12
