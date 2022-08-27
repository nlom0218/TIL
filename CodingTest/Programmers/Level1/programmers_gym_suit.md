# 예산

## 1. 개요

- 프로그래머스
- Lv.1
- 탐욕법(Greedy)
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42862)

---

## 2. 문제 설명

점심시간에 도둑이 들어, 일부 학생이 체육복을 도난당했습니다. 다행히 여벌 체육복이 있는 학생이 이들에게 체육복을 빌려주려 합니다. 학생들의 번호는 체격 순으로 매겨져 있어, 바로 앞번호의 학생이나 바로 뒷번호의 학생에게만 체육복을 빌려줄 수 있습니다. 예를 들어, 4번 학생은 3번 학생이나 5번 학생에게만 체육복을 빌려줄 수 있습니다. 체육복이 없으면 수업을 들을 수 없기 때문에 체육복을 적절히 빌려 최대한 많은 학생이 체육수업을 들어야 합니다.

전체 학생의 수 n, 체육복을 도난당한 학생들의 번호가 담긴 배열 lost, 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열 reserve가 매개변수로 주어질 때, 체육수업을 들을 수 있는 학생의 최댓값을 return 하도록 solution 함수를 작성해주세요.

---

### 2-1. 문제 설명 - 제한사항

- 전체 학생의 수는 2명 이상 30명 이하입니다.
- 체육복을 도난당한 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌의 체육복을 가져온 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
- 여벌 체육복이 있는 학생만 다른 학생에게 체육복을 빌려줄 수 있습니다.
- 여벌 체육복을 가져온 학생이 체육복을 도난당했을 수 있습니다. 이때 이 학생은 체육복을 하나만 도난당했다고 가정하며, 남은 체육복이 하나이기에 다른 학생에게는 체육복을 빌려줄 수 없습니다.

---

### 2-2. 문제 설명 - 입출력 예

| n   | lost   | reserve   | return |
| --- | ------ | --------- | ------ |
| 5   | [2, 4] | [1, 3, 5] | 5      |
| 5   | [2, 4] | [3]       | 4      |
| 3   | [3]    | [1]       | 2      |

---

### 2-3. 문제 설명 - 입출력 예 설명

예제 #1
1번 학생이 2번 학생에게 체육복을 빌려주고, 3번 학생이나 5번 학생이 4번 학생에게 체육복을 빌려주면 학생 5명이 체육수업을 들을 수 있습니다.

예제 #2
3번 학생이 2번 학생이나 4번 학생에게 체육복을 빌려주면 학생 4명이 체육수업을 들을 수 있습니다.

---

## 3. 문제 풀이

```javascript
function solution(n, lost, reserve) {
  // 1) 여벌의 체육복을 가져왔지만 체육복 한 벌이 도난당한 경우
  const new_lost = lost.filter((item) => !reserve.includes(item));
  const new_reserve = reserve.filter((item) => !lost.includes(item));

  // 2) 전체 학생 배열 생성
  let student = [];
  for (let i = 1; i < n + 1; i++) {
    student.push(i);
  }

  // 3) 체육복을 도난당하지 않는 학생들만 배열에 남기기
  student = student.filter((item) => !new_lost.includes(item));

  // 4) 여벌의 체육복이 있는 학생들의 배열(new_reserve)을 번호 순으로 정렬하기
  new_reserve.sort((a, b) => a - b);

  // 5) 여벌의 체육복이 있는 학생들의 배열을 반복문 돌리기
  new_reserve.forEach((item) => {
    // 6) 1번이라면 2번에게만 빌려줄 수 있음
    if (item === 1) {
      if (!student.includes(2)) student.push(2);
      return;
    }

    // 7) 1번이 아니라면 먼저 자신보다 앞에 있는 친구에게 빌려주기
    if (!student.includes(item - 1)) {
      student.push(item - 1);
      return;

      // 8) 자신의 앞 번호의 학생이 체육복이 있다면 뒤에 있는 친구에게 빌려주기
    } else if (!student.includes(item + 1)) {
      student.push(item + 1);
    }
  });

  // 9) 정원 수보다 1큰 요소 삭제한 후 배열의 길이 리턴하기
  student = student.filter((item) => item !== n + 1);
  return student.length;
}
```

---

### 1) 여벌의 체육복을 가져왔지만 체육복 한 벌이 도난당한 경우

```javascript
const new_lost = lost.filter((item) => !reserve.includes(item));
const new_reserve = reserve.filter((item) => !lost.includes(item));
```

`lost` 배열과 `reserve` 배열에 동일한 요소가 있다는 것은 해당 요소(번호)를 가지는 학생은
체육복을 도난당했지만 여벌의 옷이 있어 체육복이 하나 남아있다는 것이다. 이때 여벌의 체육복은
도난당했기 때문에 앞, 뒤 번호의 친구에게 체육복을 빌려줄 수 없다. 즉, `lost` 배열과 `reserve` 배열에
해당 학생을 제거해야 한다.

---

### 2) 전체 학생 배열 생성

```javascript
let student = [];
for (let i = 1; i < n + 1; i++) {
  student.push(i);
}
```

`n`은 전체 학생의 수를 의미한다. 그러므로 `n`번 반복하여 `student` 배열을 생성한다. `student` 배열의 요소는
1부터 `n`까지의 수 이다.

---

### 3) 체육복을 도난당하지 않는 학생들만 배열에 남기기

```javascript
student = student.filter((item) => !new_lost.includes(item));
```

전체 학생이 저장되어 있는 `student` 배열을 체육복을 도난당한 학생들을 제외한 학생들만 남기게 한다.
예를들어, `3`, `6` 학생이 체육복을 도난 당했으면 `3`, `6`를 제외한 요소만 `student` 배열에
남게된다.

---

### 4) 여벌의 체육복이 있는 학생들의 배열(new_reserve)을 번호 순으로 정렬하기

```javascript
new_reserve.sort((a, b) => a - b);
```

여벌의 체육복이 있는 학생은 자신의 앞, 뒤 번호의 학생에게만 체육복을 빌려줄 수 있다. 이때 가장 최선인 방법은
앞, 뒤 번호의 학생이 모두 없다면 앞 번호의 학생에게 먼저 빌려주는 것이다. 뒤 번호의 학생은 그 다음 번호의
학생이 빌려줄 수 있기 때문이다.(단 1번 학생은 앞 학생이 없기 때문에 무조건 뒤 번호 학생에게만 빌려준다.)

하지만 이때, 체육복을 빌려줄 수 있는 학생들의 번호가 들어있는 `new_reserve` 배열의 순서가 뒤죽박죽이면
안되기 때문에 번호순으로 정렬한다.

---

### 5) 여벌의 체육복이 있는 학생들의 배열을 반복문 돌리기

```javascript
new_reserve.forEach((item) => {});
```

`new_reverse`의 요소를 기준으로 체육복을 빌려주기 때문에 `new_reverse` 배열을 `Array.forEach()` 메서드를
사용하여 반복문을 실행한다.

---

### 6) 1번이라면 2번에게만 빌려줄 수 있음

```javascript
if (item === 1) {
  if (!student.includes(2)) student.push(2);
  return;
}
```

1번 학생이 여벌의 체육복이 있다면 1번 학생은 2번 학생에게만 체육복을 빌려줄 수 있다. 그러므로
`student` 배열에 `2`가 포함되어 있지 않다면 `Array.push()` 메서드를 사용하여 `2`을 추가한다.

---

### 7) 1번이 아니라면 먼저 자신보다 앞에 있는 친구에게 빌려주기

```javascript
if (!student.includes(item - 1)) {
  student.push(item - 1);
  return;
}
```

1번이 아닌 학생이 여벌의 체육복이 있다면 먼저 앞 번호 학생이 체육복이 있는지 확인하여 없으면
빌려준다. 이때 체육복을 받은 학생의 번호는 `item - 1`이고 이를 `student 배열에 추가한다.
그리고 앞 번호 학생에게 체육복을 빌려주었다면 더 이상 여벌의 체육복이 없기 때문에 다음 반복문으로 넘어가야 한다.

---

### 8) 자신의 앞 번호의 학생이 체육복이 있다면 뒤에 있는 친구에게 빌려주기

```javascript
else if (!student.includes(item + 1)) {
      student.push(item + 1);
    }
```

위의 조건문으로 넘어왔다는 것은 자신의 앞 번호의 학생은 체육복이 있어 빌려주지 않았다는 것이다.
그렇다면 그 다음으로 짚고 넘어가야 할 부분이 자신의 뒤 번호의 학생이 체육복이 있는지 없는지 확인하는 것이다.
만약 없다면 체육복을 빌려주게 된다. 이때 체육복을 받은 학생의 번호는 `item + 1`이고 이를 `student` 배열에
추가한다.

---

### 9) 정원 수보다 1큰 요소 삭제한 후 배열의 길이 리턴하기

```javascript
student = student.filter((item) => item !== n + 1);
return student.length;
```

위의 반복문을 통해 `student` 배열에는 체육복을 도난당했지만 앞 또는 뒤 번호의 학생이 체육복을 빌려주어
체육복을 입을 수 있는 학생들도 포함되어 있다.

하지만 만약 마지막 번호의 친구가 여벌의 체육복이 있고 앞 번호의 친구는 체육복을 도난당하지 않았다면 마지막 번호
친구는 `n + 1`에게 체육복을 빌려주게 된다. 하지만 해당 번호의 학생은 없기 때문에 `student` 배열에서 제거해야 한다.

그리고 마지막으로 `student` 배열의 길이를 반환한다.

---

### 결과

![programmers_gym_suit result1](/image/CodingTest/programmers_gym_suit/programmers_gym_suit_result1.png)

---

## 4. Refactoring

### 1) new_lost 배열, new_reserve 배열을 정의하지 않고 기존 배열 사용하기

체육복을 도난당했지만 여벌의 체육복이 있는 학생들로 인해 `new_lost` 배열, `new_reserve` 배열을
생성했다. 하지만 새로운 배열을 생성하지 않고 기존에 있던 `lost` 배열과 `reserve` 배열의 요소를
비교하여 같은 요소만 빼내고 싶었다.

그래서 `luck_student` 라는 새로운 배열을 만들어 해당 배열에 `lost` 배열과 `reserve` 배열에 모두
존재하는 요소를 저장하였다. 그 후 `Array.filter()`를 사용하여 `lost` 배열과 `reserve` 배열에 모두
존재하는 요소를 제거하였다.

```javascript
const luck_student = lost.filter((item) => reserve.includes(item));
lost = lost.filter((item) => !luck_student.includes(item));
reserve = reserve.filter((item) => !luck_student.includes(item));
```

추가적으로 맨 처음엔 아래와 같이 코드를 작성하였다.

```javascript
lost = lost.filter((item) => !reserve.includes(item));
reserve = reserve.filter((item) => !lost.includes(item));
```

이렇게 되면 이미 `lost` 배열이 바뀐 상태에서 `reserve` 배열과 비교하기 때문에 중복되는 요소를 걸러내지 못한다.

---

### 2) 마지막 번호만을 위한 조건문 작성

기존 코드에서 아래의 부분을 삭제하고 싶었다.

```javascript
student = student.filter((item) => item !== n + 1);
```

이를 위해 반복문에서 마지막 번호를 위한 조건문을 만들었다.

```javascript
if (item === n) {
  if (!student.includes(n - 1)) student.push(n - 1);
  return;
}
```

---

### 결과

![programmers_gym_suit_result2](/image/CodingTest/programmers_gym_suit/programmers_gym_suit_result2.png)

---

## 5. 다른 사람 풀이

가장 많은 좋아요와 댓글이 달린 풀이를 가져오려 했으나 테스트 케이스가 추가된 이후엔
해당 풀이가 더 이상 통과되지 않아 그 다음으로 많은 좋아요와 댓글이 달린 풀이를 가져왔다.

```javascript
function solution(n, lost, reserve) {
  const students = {};
  let answer = 0;

  // 1) 전체 학생 객체 생성
  for (let i = 1; i <= n; i++) {
    students[i] = 1;
  }

  // 2) 체육복을 도난당했으면 -1, 여벌의 체육복이 있으면 +1
  lost.forEach((number) => (students[number] -= 1));
  reserve.forEach((number) => (students[number] += 1));

  for (let i = 1; i <= n; i++) {
    // 3) 여벌의 체육복이 있고 앞 번호의 학생이 도난당했으면 빌려주기
    if (students[i] === 2 && students[i - 1] === 0) {
      students[i - 1]++;
      students[i]--;

      // 4) 여벌의 체육복이 있고 뒤 번호의 학생이 도난당했으면 빌려주기
    } else if (students[i] === 2 && students[i + 1] === 0) {
      students[i + 1]++;
      students[i]--;
    }
  }

  // 5) 값이 1이상이 요소를 발견할 때 마다 answer를 1씩 증가시키기
  for (let key in students) {
    if (students[key] >= 1) {
      answer++;
    }
  }
  return answer;
}
```

---

### 1) 전체 학생 객체 생성

```javascript
for (let i = 1; i <= n; i++) {
  students[i] = 1;
}
```

학생의 수인 `n`만큼 요소가 생성된다. 각각의 키는 1부터 `n`까지이면 값은 모두 1이다.

---

### 2) 체육복을 도난당했으면 -1, 여벌의 체육복이 있으면 +1

```javascript
lost.forEach((number) => (students[number] -= 1));
reserve.forEach((number) => (students[number] += 1));
```

`lost` 배열과 `reserve` 배열에 `Array.forEach()` 메서드를 사용하여 반복문을 실행한다.

`lost` 배열의 반복문에서는 요소에 해당하는 키를 찾아 해당 키의 값에 `-1`를 하고 `reserve` 배열의
반복문에서는 요소에 해당하는 키를 찾아 해당 키의 값에 `+1`를 한다.

---

### 3) 여벌의 체육복이 있고 앞 번호의 학생이 도난당했으면 빌려주기

```javascript
if (students[i] === 2 && students[i - 1] === 0) {
  students[i - 1]++;
  students[i]--;
}
```

요소의 값에 따라 아래와 같이 상황을 설명할 수 있다.

- 요소의 값이 `2`: 여벌의 체육복이 있음
- 요소의 값이 `0`: 체육복을 도난당함

위의 조건문에서는 `i` 번째 학생이 여벌의 체육복이 있고 `i-1` 번째 학생이 체육복을
도난당했으면 즉시 키가 `i-1`인 요소의 값을 1증가시키고 키가 `i`인 요소의 값을 1감소시킨다.
해당 부분에서 키가 `i`인 요소의 값은 1감소시켜 1로 만들었기 때문에 아래의 조건문은 실행되지 않는다.
(`students[i]`가 1이기 때문)

해당 부분을 분석하면서 들었던 의문점은 `i`가 1일 때이다. `i-1`은 0이기 때문에 키가 0인 요소는
`students` 객체에 존재하지 않는다.
즉, `students[0]`은 `undefined`이므로 위의 조건문은 실행되지 않는다.

---

### 4) 여벌의 체육복이 있고 뒤 번호의 학생이 도난당했으면 빌려주기

```javascript
else if (students[i] === 2 && students[i + 1] === 0) {
      students[i + 1]++;
      students[i]--;
    }
```

위의 조건문에서는 `i` 번째 학생이 여벌의 체육복이 있고 `i+1` 번째 학생이 체육복을
도난당했을 때 `i` 번째 학생이 `i+1` 번째 학생에게 체육복을 빌려주는 상황이다.

만약 `i` 번째 학생이 이미 `i-1` 번째 학생에게 체육복을 빌려주었으면 `students[i]`는
1이 되기 때문에 위의 조건문은 실행되지 않는다.

---

### 5) 값이 1이상이 요소를 발견할 때 마다 answer를 1씩 증가시키기

```javascript
for (let key in students) {
  if (students[key] >= 1) {
    answer++;
  }
}
```

객체 `studens`를 `for...in` 문을 통해 반복문을 실행하고 있다. 각각의 키를
바탕으로 값이 1이상(체육복이 있고 또는 여벌의 체육복까지 있는 학생에 해당) 요소면
`answer` 값을 1증가시킨다.

---

### 결과

테스트 속도는 비슷하다.

![programmers_gym_suit_result3](/image/CodingTest/programmers_gym_suit/programmers_gym_suit_result3.png)

---

## 6. Conclusion

> 다른 사람 풀이의 방법으로 처음엔 생각을 했지만 구체적인 방법이 떠오르지 않아 배열을 사용하였다.
> 그래서 찝찝한 마음이 있었는데 다른 사람 풀이에서 깔끔한 내용의 풀이를 볼 수 있어 많은
> 공부가 되었다. 키를 학생 번호, 값을 체육복의 개수로 설정하는 것이 멋진 접근법이라고 생각한다.
> 1번 학생, 마지막 학생에 대해 크게 신경을 쓰지 않아도 된다. 또한 `reverse` 함수를 정렬하지 않아도
> 이미 `students` 객체에서 키로 학생 번호를 관리하고 있기 때문에 학생의 순서를 생각하지 않아도 된다.
> 이러한 이유로 인해 더 많은 곳에서 잘 활용할 수 있는 코드가 될 수 있을거 같다.

---

---

📅 2022-08-27
