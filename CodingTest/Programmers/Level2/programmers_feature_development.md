# 기능개발

## 1. 개요

- 프로그래머스
- Lv.2
- 스택/큐
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42586)

---

## 2. 문제 설명

프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

---

### 2-1. 문제 설명 - 제한 사항

- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

---

### 2-2. 문제 설명 - 입출력 예

| progresses               | speeds             | return    |
| ------------------------ | ------------------ | --------- |
| [93, 30, 55]             | [1, 30, 5]         | [2, 1]    |
| [95, 90, 99, 99, 80, 99] | [1, 1, 1, 1, 1, 1] | [1, 3, 2] |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1  
첫 번째 기능은 93% 완료되어 있고 하루에 1%씩 작업이 가능하므로 7일간 작업 후 배포가 가능합니다.
두 번째 기능은 30%가 완료되어 있고 하루에 30%씩 작업이 가능하므로 3일간 작업 후 배포가 가능합니다. 하지만 이전 첫 번째 기능이 아직 완성된 상태가 아니기 때문에 첫 번째 기능이 배포되는 7일째 배포됩니다.
세 번째 기능은 55%가 완료되어 있고 하루에 5%씩 작업이 가능하므로 9일간 작업 후 배포가 가능합니다.

따라서 7일째에 2개의 기능, 9일째에 1개의 기능이 배포됩니다.

입출력 예 #2  
모든 기능이 하루에 1%씩 작업이 가능하므로, 작업이 끝나기까지 남은 일수는 각각 5일, 10일, 1일, 1일, 20일, 1일입니다. 어떤 기능이 먼저 완성되었더라도 앞에 있는 모든 기능이 완성되지 않으면 배포가 불가능합니다.

따라서 5일째에 1개의 기능, 10일째에 3개의 기능, 20일째에 2개의 기능이 배포됩니다.

---

## 3. 문제 풀이

```javascript
function solution(progresses, speeds) {
  // 1) 미완료 된 작엽량 구하기
  const incomplete = progresses.map((item) => 100 - item);

  // 2) 각 기능이 완료되기 까지 필요한 작업 일수 구하기
  const need_days = [];
  incomplete.forEach((item, index) => {
    need_days.push(Math.ceil(item / speeds[index]));
  });

  // 3) 몇 개의 기능이 함께 배포되는지 구하기
  const deployable_number = [];
  let max = need_days[0];
  let count = 0;
  need_days.forEach((item) => {
    // 4) 작업 일수가 max와 같거나 작을 경우 함께 배포 가능
    if (item <= max) {
      count += 1;
    } else {
      // 5) 작업 일수가 max보다 더 많이 필요할 경우 함께 배포 불가능
      deployable_number.push(count);
      max = item;
      count = 1;
    }
  });
  deployable_number.push(count);

  return deployable_number;
}
```

---

### 1) 미완료 된 작엽량 구하기

```javascript
const incomplete = progresses.map((item) => 100 - item);
```

`progresses` 배열에는 배포되어야 하는 순서대로 작업의 진도가 저장되어 있다. 각각의 작업 진도는 100 미만의 수이다.

배포가 가능하기 위해서는 작업의 진도가 100이 되어야 한다. 그러면 얼마만큼 더 작업이 필요한지 구해야 하는데 100에서 현재 작업 진도를 빼면 쉽게 구할 수 있다. 예를들어 현재 작업 진도가 90인 작업은 배포가 가능하기 위해서는 10의 작업 진도가 더 필요하다.

`incomplete` 변수는 배열이며 각각의 요소는 어떤 작업이 배포가 되기 위해서 얼마 만큼의 작업이 더 필요한지를 나타낸다.

---

### 2) 각 기능이 완료되기 까지 필요한 작업 일수 구하기

```javascript
const need_days = [];
incomplete.forEach((item, index) => {
  need_days.push(Math.ceil(item / speeds[index]));
});
```

각각의 작업은 모두 다른 작업 속도를 가지고 있다. 이런 작업 속도는 `speeds` 배열에서 확인할 수 있다.

`incomplete` 배열과 `speeds` 배열을 통해 특정 작업이 완료 되기까지 얼마 만큼의 시간(day)이 필요한지 구할 수 있다. 해당 시간(day)을 구하는 코드는 아래와 같다.

```javascript
Math.ceil(item / speeds[index]);
```

`Math.ceil()` 메서드는 매개변수로 주어진 숫자가 소수값이 존재할 때 값을 올리는 역할을 한다. 쉽게 말해 수학에서 흔히 말하는 `올림`이다.
`item / speeds[index]`의 값을 올리는 이유 시간(day)은 정수이기 때문이다. 이를 예시를 바탕으로 살펴보자.

- 작업이 완료되기 까지 필요한 작업량: 20
- 하루에 작업할 수 있는 작업 속도: 7
- 20 / 7: 2.xxxx
- 2일 동안 작업을 해서는 완료할 수 없다.
- 3일 동안 작업을 해야 완료할 수 있다.

---

### 3) 몇 개의 기능이 함께 배포되는지 구하기

`need_days`는 각각의 기능이 개발 완료되기 까지 필요한 시간(day)을 요소로 가지는 배열이다. 앞선 기능보다 먼저 기능 개발이 끝났다 해도 앞선 기능이 배포가 되지 않으면 뒤에 있는 기능도 배포를 할 수 없다.

`need_days`의 요소를 비교하여 함께 배포할 수 있는 기능의 수를 구하는 코드를 살펴보기 전, 먼저 해당 코드에 필요한 변수를 선언한다.

```javascript
const deployable_number = [];
let max = need_days[0];
let count = 0;
```

- `deployable_number`: 함께 배포가 되는 기능의 수를 요소로 가지는 배열, 문제에서 원하는 답
- `max`: 작업 완료까지 가장 오래 걸린 시간(day)
- `count`: 함께 배포가 되는 기능의 수를 세는 변수

---

### 4) 작업 일수가 max와 같거나 작을 경우 함께 배포 가능

```javascript
if (item <= max) {
  count += 1;
}
```

만약 작업일수(item)가 `max`보다 작거나 같으면 해당 기능은 `max`에 해당 하는 기능이 완료 될 때 배포가 가능하다. 그래서 배포 하는 기능의 수 인 `count` 변수를 1증가시킨다.

---

### 5) 작업 일수가 max보다 더 많이 필요할 경우 함께 배포 불가능

```javascript
else {
    deployable_number.push(count);
    max = item;
    count = 1;
}
```

만약 작업일수(item)가 `max`보다 크다면 해당 기능의 배포는 다음 번에 이루어진다.

- `deployable_number.push(count)`: 지금까지 배포 가능한 기능의 수인 `count`를 `deployable_number` 배열에 넣는다.
- `max = item`: 작업 완료까지 가장 오래 걸린 시간을 갱신한다.
- `count = 1`: `count`를 1로 초기화 한다.(1은 들어온 item에 해당)

반복문이 끝나면 마지막 남은 count까지 `deployable_number` 배열에 넣어 해당 배열을 반환한다.

5, 6과정의 예시로 아래의 그림처럼 나타내봤다.

![programmers_feature_development_ex](/image/CodingTest/programmers_feature_development/programmers_feature_development_ex.jpg)

---

### 결과

![programmers_feature_development_result1](/image/CodingTest/programmers_feature_development/programmers_feature_development_result1.png)

---

## 4. 다른 사람 풀이

가장 많은 좋아요와 댓글이 달린 풀이이다.

```javascript
function solution(progresses, speeds) {
  let answer = [0];
  let days = progresses.map((progress, index) =>
    Math.ceil((100 - progress) / speeds[index])
  );
  let maxDay = days[0];

  for (let i = 0, j = 0; i < days.length; i++) {
    if (days[i] <= maxDay) {
      answer[j] += 1;
    } else {
      maxDay = days[i];
      answer[++j] = 1;
    }
  }

  return answer;
}
```

해당 풀이 중 재밌고 공부해야 할 부분은 `for`문이다. `i`와 `j`를 선언하고 `i`는 반복문을 돌면서 계속 1씩 증가하지만 `j`는 특정 조건에서만 증가한다. 특정 조건이라 함은 `maxDay`보다 큰 값이 들어오는 경우이다. 이렇게 `j`가 늘어난다는 것은 배열의 인덱스가 하나 씩 증가하는 것이고 배열의 인덱스가 증가하는 것은 서로 다른 날에 기능 배포를 한다는 것이다.

내가 작성한 코드보다 훨씬 가독성이 좋아보인다. 특히 배열 요소 자체의 값과 인덱스를 1씩 증가시켜 배열을 만드는 것이 멋져보인다.

---

## 5. 큐를 이용한 문제 풀이(2022-09-03 추가)

```javascript
function solution(progresses, speeds) {
  const incomplete = progresses.map((item) => 100 - item);

  const need_days = [];
  incomplete.forEach((item, index) => {
    need_days.push(Math.ceil(item / speeds[index]));
  });

  const answer = [];
  let index = 0;
  let max = need_days[0];

  for (let i = 0; i < need_days.length; i++) {
    if (need_days[i] <= max) {
      answer[index] = (answer[index] | 0) + 1;
    } else {
      max = need_days[i];
      index++;
      answer[index] = 1;
    }
  }
  return answer;
}
```

큐의 개념을 이용하여 문제를 다시 풀어봤다. `incomplete` 배열, `need_days` 배열,
`answer` 배열, `max`의 값은 기존과 동일하지만 `index`라는 변수를 선언하였다.

- `need_days` 배열의 길이 만큼 반복문을 실행한다.
- 만약 `need_days`의 값이 `max`과 같거나 작다면 앞의 기능과 함께 업데이트가 가능하다.
- 그래서 기존에 존재하는 값(`answer[index]`)에 1를 더한다. 만약 존재하지 않으면 0에 1를 더한다.
- 이런 식으로 함께 업데이트가 가능한 기능 수를 `answer` 배열의 값으로 저장한다.
- 하지만...
- 만약 `need_days`의 값이 `max`보다 크다면 앞의 기능과 함께 업데이트가 불가능한다.
- 그래서 `index`의 값을 1증가시킨 후 `answer[index]`의 값을 1로 정한다.
- 이런 식으로 `need_days` 배열의 길이만큼 반복을 한다.
- 마지막으론 `answer`를 반환한다.

배열에서 앞의 요소를 가져오기 위해서 `Array.shift()` 메서드를 사용 한다면 시간 복잡도는 O(n)이다.
하지만 위의 풀이에서는 직접적으로 배열의 첫 번째 요소를 가져오고 삭제하는 것이 아니라
단순히 배열의 요소만 검색해서 값을 가져오는 것(탐색)이기 때문에 시간 복잡도는 O(1)이다.

---

### 결과

![programmers_feature_development_result3](/image/CodingTest/programmers_feature_development/programmers_feature_development_result2.png)

---

> 사실 이 풀이가 자료구조 큐를 사용한 것은 아니다. 하지만 큐를 공부하면서
> 배열과 큐의 시간 복잡도에 대해 새로운 사실을 알게되었고 이를 활용한 풀이이다.
> 코드를 추가 작성하여 `need_days` 배열을 큐로 바꾸어 풀 수 있지만 오히려 그렇게
> 되면 가독성이 떨어지기 때문에 보기 좋은 코드는 아닐 것 같다.
> 앞으로 배열의 첫 번째 요소부터 값을 필요로 한다면 위의 풀이와 같은 과정을
> 풀지 않을까 싶다.

---

## 6. Conclusion

> 문제를 푸는 것과 푼 문제의 과정을 설명하는 것은 다른 차원의 문제로 보인다. 내가 푼 과정을 그대로 설명하는 것 뿐인데도 막상 설명을 적으려 하면 막히는 부분이 생긴다. 누군가에게 설명할 땐 더욱 힘들것 같다. 아마 내가 적을 풀이들은 다른 누군가가 봐도 이해를 못할 가능성이 충분히 있다. 그 만큼 아직 내가 알고 있는 것에 대한 깊이가 얇기 때문이라고 생각한다. 그래도 코드만 딱 적는 것이 아니라 나만의 언어로 풀어서 설명해보는 연습을 통해 코드 작성 실력과 설명 능력을 향샹시키자.  
> 문제 자체는 다른 Level2보다는 쉬웠던 편이었다. 개념 하나를 놓쳐서 생각보다 시간이 많이 소요되긴 하였지만 어렵지 않게 정답을 구할 수 있었다. 큐/스택을 더 잘 다룰 수 있는 방법에 대해 공부를 하는 좋은 문제였다. 그리고! 다른 사람 풀이에서 배열의 요소과 인덱스를 직접 바꾸는 방법도 알 수 있어 좋았다. 해당 방법은 알고 있었지만 문제에서 이렇게 쓰인다니 신기할 따름이다. 끝-

---

📅 2022-08-17  
📅 2022-09-03 - 5. 큐를 이용한 문제 풀이(2022-09-03 추가)
