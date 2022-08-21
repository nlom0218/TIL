# 모의고사

## 1. 개요

- 프로그래머스
- Lv.1
- 완전탐색
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42840)

---

## 2. 문제 설명

수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.

1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...
2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...
3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.

---

### 2-1. 문제 설명 - 제한 조건

- 시험은 최대 10,000 문제로 구성되어있습니다.
- 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
- 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.

---

### 2-2. 문제 설명 - 입출력 예

| answers     | return  |
| ----------- | ------- |
| [1,2,3,4,5] | [1]     |
| [1,3,2,4,2] | [1,2,3] |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1

- 수포자 1은 모든 문제를 맞혔습니다.
- 수포자 2는 모든 문제를 틀렸습니다.
- 수포자 3은 모든 문제를 틀렸습니다.

따라서 가장 문제를 많이 맞힌 사람은 수포자 1입니다.

입출력 예 #2

모든 사람이 2문제씩을 맞췄습니다.

---

## 3. 문제 풀이

```javascript
function solution(answers) {
  const a = [];
  const b = [];
  const c = [];

  // 1) answers의 길이 만큼 수포자들이 찍은 번호 배열 만들기
  for (let i = 0; i < answers.length; i++) {
    a.push((i + 1) % 5 === 0 ? 5 : (i + 1) % 5);
    if ((i + 1) % 2 === 1) b.push(2);
    if ((i + 1) % 8 === 2) b.push(1);
    if ((i + 1) % 8 === 4) b.push(3);
    if ((i + 1) % 8 === 6) b.push(4);
    if ((i + 1) % 8 === 0) b.push(5);
    if ((i + 1) % 10 === 1 || (i + 1) % 10 === 2) c.push(3);
    if ((i + 1) % 10 === 3 || (i + 1) % 10 === 4) c.push(1);
    if ((i + 1) % 10 === 5 || (i + 1) % 10 === 6) c.push(2);
    if ((i + 1) % 10 === 7 || (i + 1) % 10 === 8) c.push(4);
    if ((i + 1) % 10 === 9 || (i + 1) % 10 === 0) c.push(5);
  }

  // 2) answers의 값과 수포자들이 찍은 번호를 비교하여 점수 올리기
  const answerNum = [0, 0, 0];
  answers.forEach((item, index) => {
    if (item === a[index]) answerNum[0] += 1;
    if (item === b[index]) answerNum[1] += 1;
    if (item === c[index]) answerNum[2] += 1;
  });

  // 3) 가장 높은 점수 구하기
  const best = [...answerNum].sort((a, b) => b - a)[0];

  // 4) 가장 높은 점수를 가진 수포자들만 반환하기
  return answerNum
    .map((item, index) => {
      if (item === best) return index + 1;
    })
    .filter((item) => item);
}
```

---

### 1) answers의 길이 만큼 수포자들이 찍은 번호 배열 만들기

수포자들은 규칙에 따라 문제를 찍는다.

1. 1번 수포자의 규칙
   - 문제 번호를 5로 나눈 나머지로 찍는다.
2. 2번 수포자의 규칙
   - 홀수 번호는 모두 2번으로 찍는다.
   - 짝수 번호는 8로 나눈 나머지에 따라 다르다.
   - 짝수 번호를 8로 나눈 나머지가 2이면 1, 4이면 3, 6이면 4, 0이면 5를 찍는다.
3. 3번 수포자의 규칙
   - 문제 번호를 10으로 나눈 나머지에 따라 다르다.
   - 문제 번호를 10으로 나눈 나머지가 1또는 2라면 3, 3또는 4라면 1, 5또는 6이라면 2, 7또는 8이라면 4, 9또는 0이라면 5를 찍는다.

위의 규칙을 바탕으로 각 수포자들이 찍은 번호 배열을 `answers` 배열의 길이 만큼 만든다.

```javascript
for (let i = 0; i < answers.length; i++) {
  a.push((i + 1) % 5 === 0 ? 5 : (i + 1) % 5);
  if ((i + 1) % 2 === 1) b.push(2);
  if ((i + 1) % 8 === 2) b.push(1);
  if ((i + 1) % 8 === 4) b.push(3);
  if ((i + 1) % 8 === 6) b.push(4);
  if ((i + 1) % 8 === 0) b.push(5);
  if ((i + 1) % 10 === 1 || (i + 1) % 10 === 2) c.push(3);
  if ((i + 1) % 10 === 3 || (i + 1) % 10 === 4) c.push(1);
  if ((i + 1) % 10 === 5 || (i + 1) % 10 === 6) c.push(2);
  if ((i + 1) % 10 === 7 || (i + 1) % 10 === 8) c.push(4);
  if ((i + 1) % 10 === 9 || (i + 1) % 10 === 0) c.push(5);
}
```

---

### 2) answers의 값과 수포자들이 찍은 번호를 비교하여 점수 올리기

```javascript
const answerNum = [0, 0, 0];
answers.forEach((item, index) => {
  if (item === a[index]) answerNum[0] += 1;
  if (item === b[index]) answerNum[1] += 1;
  if (item === c[index]) answerNum[2] += 1;
});
```

`answerNum` 배열은 순서대로 1번, 2번, 3번 수포자들의 점수를 나타낸다.

반복문을 통해 `answers` 배열의 각 요소와 수포자들이 찍은 번호을 비교한다. 같은 인덱스의 요소를 비교하여 같은 값이면 정답이다.

---

### 3) 가장 높은 점수 구하기

```javascript
const best = [...answerNum].sort((a, b) => b - a)[0];
```

3명의 수포자들 받은 점수에서 가장 큰 값을 `best` 변수에 할당한다. 이때 `answerNum` 배열을 얇은 복사를 하여 원본 배열이 바뀌지 않게 한다.

---

### 4) 가장 높은 점수를 가진 수포자들만 반환하기

```javascript
return answerNum
  .map((item, index) => {
    if (item === best) return index + 1;
  })
  .filter((item) => item);
```

`answerNum` 배열의 요소 중 `best`와 값이 같다면 해당 인덱스에 1을 더한 값을 리턴한다. 만약 같지 않으면 `null`를 리턴하게 되므로
`Array.filter()` 메서드를 사용하여 `null`값을 지운다.

---

### 결과

![programmers_mock_exam_result1](/image/CodingTest/programmers_mock_exam/programmers_mock_exam_result1.png)

---

## 4. Refactoring

### 1) 배열의 요소 중 최대값을 찾는 방법

가장 높은 점수 구하는 과정을 아래와 같이 수정하였다.

```javascript
const best = Math.max(...answerNum);
```

스프레드 연산자를 사용하여 `answerNum`를 개별 요소로 분리하여 `Math.max` 메서드의 인수로 전달한다. `Math.max` 메서드는 인수로 주어지는 숫자들 중 가장 큰 값을 리턴하기 때문에
`answerNums` 요소 중 가장 큰 값을 `best` 변수에 할당하게 된다.

---

### 2) 각 수포자들이 찍은 번호의 배열

나의 풀이에서 `a`, `b`, `c` 배열을 만드는 과정이 너무 길고 조건문이 너무 많이 쓰여 보기에도 좋지 않다. 그래서 아래와 같이 수정하였다.

```javascript
const a = [1, 2, 3, 4, 5];
const b = [2, 1, 2, 3, 2, 4, 2, 5];
const c = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5];

const answerNum = [0, 0, 0];

answers.forEach((item, index) => {
  if (item === a[index % 5]) answerNum[0] += 1;
  if (item === b[index % 8]) answerNum[1] += 1;
  if (item === c[index % 10]) answerNum[2] += 1;
});
```

각 수포자들는 반복되는 패턴으로 문제를 찍는다. 이를 활용하여 우선 반복되는 패턴을 가져와 배열을 만들었다.

그 후

- 문제 번호의 1작은 수를 배열의 길이로 나눈 나머지는 해당 배열의 인덱스 번호이다.

위와 같은 논리를 바탕으로 정답과 수포자들이 찍은 번호를 비교한다.

---

### 결과

실행 속도가 빨라진 것을 확인할 수 있다.

![programmers_mock_exam_result2](/image/CodingTest/programmers_mock_exam/programmers_mock_exam_result2.png)

---

## 5. 다른 사람 풀이

가장 많은 좋아요를 얻은 풀이이다.

```javascript
function solution(answers) {
  var answer = [];
  var a1 = [1, 2, 3, 4, 5];
  var a2 = [2, 1, 2, 3, 2, 4, 2, 5];
  var a3 = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5];

  var a1c = answers.filter((a, i) => a === a1[i % a1.length]).length;
  var a2c = answers.filter((a, i) => a === a2[i % a2.length]).length;
  var a3c = answers.filter((a, i) => a === a3[i % a3.length]).length;
  var max = Math.max(a1c, a2c, a3c);

  if (a1c === max) {
    answer.push(1);
  }
  if (a2c === max) {
    answer.push(2);
  }
  if (a3c === max) {
    answer.push(3);
  }

  return answer;
}
```

전체적인 코드가 나의 풀이보다 훨씬 간단하여 가독성이 뛰어나 보인다.

특히 `if` 문을 사용하여 조건이 맞는다면 수포자의 번호를 `answer` 배열에 넣는 과정이 눈에 확 들어온다.. 해당 코드를 보니
난 왜 복잡하게 `Array.map()` 메서드와 `Array.filter()` 메서드를 사용했을까라는 생각을 했다.

---

## 6. Conclusion

> 배열 메서드를 사용하는 것에 집착을 하지 말아야겠다. 간단하게 `if` 문을 통해 구현할 수 있는데 굳이 메서드를 사용하니 실행속도가
> 느려지는 것 같다. 그리고 가독성 또한 좋진 않다. 직관적이지 않다는 것이다. 최대한 간단하고 쉽게 생각하면서 문제를 풀어보자.

---

[👆](#예산)

📅 2022-08-22
