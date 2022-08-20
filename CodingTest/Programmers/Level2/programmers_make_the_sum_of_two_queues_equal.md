# 두 큐 합 같게 만들기

## 1. 개요

- 프로그래머스
- Lv.2
- 2022 KAKAO TECH INTERNSHIP
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/118667)

---

## 2. 문제 설명

길이가 같은 두 개의 큐가 주어집니다. 하나의 큐를 골라 원소를 추출(pop)하고, 추출된 원소를 다른 큐에 집어넣는(insert) 작업을 통해 각 큐의 원소 합이 같도록 만들려고 합니다. 이때 필요한 작업의 최소 횟수를 구하고자 합니다. 한 번의 pop과 한 번의 insert를 합쳐서 작업을 1회 수행한 것으로 간주합니다.

큐는 먼저 집어넣은 원소가 먼저 나오는 구조입니다. 이 문제에서는 큐를 배열로 표현하며, 원소가 배열 앞쪽에 있을수록 먼저 집어넣은 원소임을 의미합니다. 즉, pop을 하면 배열의 첫 번째 원소가 추출되며, insert를 하면 배열의 끝에 원소가 추가됩니다. 예를 들어 큐 [1, 2, 3, 4]가 주어졌을 때, pop을 하면 맨 앞에 있는 원소 1이 추출되어 [2, 3, 4]가 되며, 이어서 5를 insert하면 [2, 3, 4, 5]가 됩니다.

다음은 두 큐를 나타내는 예시입니다.

```javascript
queue1 = [3, 2, 7, 2];
queue2 = [4, 6, 5, 1];
```

두 큐에 담긴 모든 원소의 합은 30입니다. 따라서, 각 큐의 합을 15로 만들어야 합니다. 예를 들어, 다음과 같이 2가지 방법이 있습니다.

1. queue2의 4, 6, 5를 순서대로 추출하여 queue1에 추가한 뒤, queue1의 3, 2, 7, 2를 순서대로 추출하여 queue2에 추가합니다. 그 결과 queue1은 [4, 6, 5], queue2는 [1, 3, 2, 7, 2]가 되며, 각 큐의 원소 합은 15로 같습니다. 이 방법은 작업을 7번 수행합니다.
2. queue1에서 3을 추출하여 queue2에 추가합니다. 그리고 queue2에서 4를 추출하여 queue1에 추가합니다. 그 결과 queue1은 [2, 7, 2, 4], queue2는 [6, 5, 1, 3]가 되며, 각 큐의 원소 합은 15로 같습니다. 이 방법은 작업을 2번만 수행하며, 이보다 적은 횟수로 목표를 달성할 수 없습니다.

따라서 각 큐의 원소 합을 같게 만들기 위해 필요한 작업의 최소 횟수는 2입니다.

길이가 같은 두 개의 큐를 나타내는 정수 배열 queue1, queue2가 매개변수로 주어집니다. 각 큐의 원소 합을 같게 만들기 위해 필요한 작업의 최소 횟수를 return 하도록 solution 함수를 완성해주세요. 단, 어떤 방법으로도 각 큐의 원소 합을 같게 만들 수 없는 경우, -1을 return 해주세요.

---

### 2-1. 문제 설명 - 제한사항

- 1 ≤ queue1의 길이 = queue2의 길이 ≤ 300,000
- 1 ≤ queue1의 원소, queue2의 원소 ≤ 109
- 주의: 언어에 따라 합 계산 과정 중 산술 오버플로우 발생 가능성이 있으므로 long type 고려가 필요합니다.

---

### 2-2. 문제 설명 - 입출력 예

| queue1       | queue2        | result |
| ------------ | ------------- | ------ |
| [3, 2, 7, 2] | [4, 6, 5, 1]  | 2      |
| [1, 2, 1, 2] | [1, 10, 1, 2] | 7      |
| [1, 1]       | [1, 5]        | -1     |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1

문제 예시와 같습니다.

입출력 예 #2

두 큐에 담긴 모든 원소의 합은 20입니다. 따라서, 각 큐의 합을 10으로 만들어야 합니다. queue2에서 1, 10을 순서대로 추출하여 queue1에 추가하고, queue1에서 1, 2, 1, 2와 1(queue2으로부터 받은 원소)을 순서대로 추출하여 queue2에 추가합니다. 그 결과 queue1은 [10], queue2는 [1, 2, 1, 2, 1, 2, 1]가 되며, 각 큐의 원소 합은 10으로 같습니다. 이때 작업 횟수는 7회이며, 이보다 적은 횟수로 목표를 달성하는 방법은 없습니다. 따라서 7를 return 합니다.

입출력 예 #3

어떤 방법을 쓰더라도 각 큐의 원소 합을 같게 만들 수 없습니다. 따라서 -1을 return 합니다.

---

## 3. 문제 풀이

```javascript
// 1) 큐 생셩 클래스
class Queue {
  constructor() {
    this.storage = {};
    this.front = 0;
    this.rear = 0;
  }

  size() {
    if (this.storage[this.rear] === undefined) {
      return 0;
    } else {
      return this.rear - this.front + 1;
    }
  }

  add(value) {
    if (this.size() === 0) {
      this.storage["0"] = value;
    } else {
      this.rear += 1;
      this.storage[this.rear] = value;
    }
  }

  popleft() {
    let temp;
    if (this.front === this.rear) {
      temp = this.storage[this.front];
      delete this.storage[this.front];
      this.front = 0;
      this.rear = 0;
    } else {
      temp = this.storage[this.front];
      delete this.storage[this.front];
      this.front += 1;
    }
    return temp;
  }
}

// 2) 초기 큐 데이터 생성하기
function set_queue(arr, queue) {
  for (let i = 0; i < arr.length; i++) {
    queue.add(arr[i]);
  }
}

// 3) 요소의 모든 합 구하기
function sum_queue(arr) {
  let sum = 0;
  for (let i = 0; i < arr.length; i++) {
    sum += arr[i];
  }
  return sum;
}

function solution(queue1, queue2) {
  let queue1_sum = sum_queue(queue1);
  let queue2_sum = sum_queue(queue2);
  const q1 = new Queue();
  const q2 = new Queue();
  set_queue(queue1, q1);
  set_queue(queue2, q2);

  // 4) 두 배열에 담긴 모든 요소의 합의 절반구하기
  const helf = (queue1_sum + queue2_sum) / 2;

  // 5) 최대로 수행할 수 있는 작업 횟수
  const max = 3 * queue1.length - 3;
  let answer = 0;

  // 6) 큐1의 합이 helf가 될 때 까지 반복
  while (queue1_sum !== helf) {
    // 7) answer이 max보다 클 경우 -1를 리턴하고 종료
    if (answer > max) {
      answer = -1;
      break;
    }
    // 8) 반복문이 실행 될 때 마다 answer 값 1씩 올리기
    answer += 1;
    // 9) 큐1의 합이 더 클 경우 큐1의 요소를 하나 빼고 큐2에 추가하기, 해당 요소만큼 큐1의 합에 더하기
    if (queue1_sum > helf) {
      const num = q1.popleft();
      q2.add(num);
      queue1_sum -= num;
    } else {
      const num = q2.popleft();
      q1.add(num);
      queue1_sum += num;
    }
  }

  // 10) 작업 횟수 반환하기
  return answer;
}
```

---

### 1) 큐 생셩 클래스

`solution` 함수에는 두 개의 매개변수가 있다. 요소가 숫자로 이루어진 길이가 같은 두 배열이다.
두 배열의 합이 같게 되는 최소한의 작업 횟수를 구하는 것이 문제에서 원하는 답이다. 이때 각각의 배열에서
요소를 제거하고 추가하는 방벙이 바로 큐이다. 그렇기 때문에 매개변수로 받는 두 개의 배열을 큐로 바꾸어
문제를 푼다.

> 나중에 알게 된 사실이지만 배열의 요소를 추가, 삭제하는 `Array.shift()` 메서드, `Array.push()`
> 메서드는 시간 복잡도가 O(n)이고 큐에서 원소를 추가, 삭제하는 과정의 시간 복잡도는 O(1)이라고 한다.
> 큐라는 자료구조에 대해 자세히 알지 못하고 배열을 큐로 생각하여 풀었더니 3개의 테스트에서 계속 시간초과가
> 발생하였다.

---

### 2) 초기 큐 데이터 생성하기

```javascript
function set_queue(arr, queue) {
  for (let i = 0; i < arr.length; i++) {
    queue.add(arr[i]);
  }
}
```

배열과 큐를 받아서 배열의 요소를 순서대로 큐에 넣는 함수이다.

아래는 새로운 큐 두개를 생성하고 각각의 `queue1` 배열과 `queue2` 배열을 받아 초기 데이트를 생성하는 과정이다.

```javascript
const q1 = new Queue();
const q2 = new Queue();
set_queue(queue1, q1);
set_queue(queue2, q2);
```

---

### 3) 모든 요소의 합 구하기

```javascript
function sum_queue(arr) {
  let sum = 0;
  for (let i = 0; i < arr.length; i++) {
    sum += arr[i];
  }
  return sum;
}
```

초기 `queue1` 배열과 `queue2` 배열의 요소들의 합을 구할 때 사용한다.(아래 코드 참고)

```javascript
let queue1_sum = sum_queue(queue1);
let queue2_sum = sum_queue(queue2);
```

---

### 4) 두 배열에 담긴 모든 요소의 합의 절반구하기

```javascript
const helf = (queue1_sum + queue2_sum) / 2;
```

두 배열의 모든 요소의 합을 구한 뒤 나누기 2를 하여 절반값을 `helf` 변수에 저장한다. `helf`는
`queue1_sum`과 `queue2_sum`이 가져야 할 값이다.

---

### 5) 최대로 수행할 수 있는 작업 횟수

```javascript
const max = 3 * queue1.length - 3;
```

최대로 수행할 수 있는 작업 횟수이다. 한 쪽 큐에서 하나의 데이터를 제외하고 다른 큐로 이동하고 다시
반대편 큐에서 하나의 데이터를 제외하고 다른 큐로 이동하는 횟수이다. 해당 값을 넘기게 된다면 어떤 방법을
쓰더라도 각 큐의 원소 합을 같게 만들 수 없다.

---

### 6) 큐1의 합이 helf가 될 때 까지 반복

```javascript
while (queue1_sum !== helf) {}
```

`queue1_sum`의 값이 `helf`가 될 때 까지 반복을 한다. 두 값이 같다는 것은 각 큐의 원소 합이 서로
같은 뜻이므로 정답 조건을 만족한다. 이후의 반복은 무의미하다.

---

### 7) 큐1, 큐2의 길이가 0이거나 answer이 max보다 클 경우 -1를 리턴하고 종료

```javascript
if (answer > max) {
    answer = -1;
    break;
}
```

반복문이 실행될 때 마다 `answer`가 1씩 증가하게 되는데(8)참고) `answer`는 큐에 있는 데이터가 이동하는 작업 횟수를 의미한다.
해당 `answer`가 최대 작업 횟수를 의미하는 `max`보다 클 경우 `answer`를 -1로 정한 후 반복문을 종료한다.

---

### 8) 반복문이 실행 될 때 마다 answer 값 1씩 올리기

```javascript
answer += 1;
```

반복문이 실행될 때 마다 `answer`을 1씩 증가시키기

---

### 9) 큐1의 합이 더 클 경우 큐1의 요소를 하나 빼고 큐2에 추가하기, 해당 요소만큼 큐1의 합에 더하기

```javascript
if (queue1_sum > helf) {
  const num = q1.popleft();
  q2.add(num);
  queue1_sum -= num;
}
```

기본적으로 더 큰 합을 가진 큐에서 데이터를 빼내 다른 큐로 데이터를 보낸다.
이때 `queue1_sum`이 `helf`보다 더 크다는 것은 `q1` 데이터의 합이 `q2` 데이터의 합 보다 크다는 것을 의미한다.

그렇기 때문에 `q1` 큐에서 데이터를 하나 빼내고 해당 데이터를 `q2` 큐에 추가를 한다. 그리고 빼낸 데이터의 값만큼
`queue1_sum`에서 빼준다.

만약 `qeueu1_sum`이 `helf`보다 작다면 반대의 작업이 이루어진다.

---

### 10) 작업 횟수 반환하기

```javascript
return answer;
```

`answer`는 반복문이 실행될 때마다 1씩 증가가 되는 변수이다. 그리고 이는 얼마만큼의 작업 횟수가 필요한지를
나타내므로 반환하는 값이 된다.

---

### 결과

![programmers_make_the_sum_of_two_queues_equal_result](/image/CodingTest/programmers_make_the_sum_of_two_queues_equal/programmers_make_the_sum_of_two_queues_equal_result1.png)

---

## 4. Refactoring

나의 풀이에서 `2) 초기 큐 데이터 생성하기` 과정과 `3) 모든 요소의 합 구하기` 과정을 생략하고 싶었다.

---

### 4-1. `2) 초기 큐 데이터 생성하기` 과정 생략

해당 과정을 위해 `Queue class`의 `constructor` 함수를 수정하였다.
해당 함수가 배열을 받아 배열의 값을 바탕으로 초기 데이터를 구성할 수 있도록 하였다.(아래 코드 참고)

```javascript
 constructor(arr) {
     this.storage = {};
     this.front = 0;
     this.rear = arr.length - 1;
     arr.forEach((item, index) => {
         this.storage[index] = item
     })
 }
```

또한 새로운 큐를 생성할 때 배열을 인수로 전달한다.(아래 코드 참고)

```javascript
const q1 = new Queue(queue1);
const q2 = new Queue(queue2);
```

---

### 4-2. `3) 모든 요소의 합 구하기` 과정 생략

해당 과정을 위해 `Array.reduce()` 메서드를 사용하였다.

```javascript
let queue1_sum = queue1.reduce((acc, cur) => acc + cur);
let queue2_sum = queue2.reduce((acc, cur) => acc + cur);
```

---

### 결과

![programmers_make_the_sum_of_two_queues_equal_result2](/image/CodingTest/programmers_make_the_sum_of_two_queues_equal/programmers_make_the_sum_of_two_queues_equal_result2.png)

---

## 5. 다른 사람 풀이

다른 사람의 풀이 중 큐 자료구조를 사용하지 않았던 풀이를 가져왔다.

```javascript
function solution(queue1, queue2) {
  let answer = 0;
  const max = queue1.length * 2;

  // 1) `value` 값은 무엇을 나타내는 것일까?
  let value = queue1.reduce((acc, cur, idx) => acc + cur - queue2[idx], 0) / 2;

  // 2) 두 개의 인데스를 관리
  let [i, j] = [0, 0];
  while (value !== 0 && answer < max * 2) {
    if (value > 0) {
      // 3) 값을 가져오기만 할 뿐 배열에서 제거하지 않는다.
      const v = queue1[i];
      i++;
      value -= v;
      queue2.push(v);
    } else {
      const v = queue2[j];
      j++;
      value += v;
      queue1.push(v);
    }
    answer++;
  }
  return value !== 0 ? -1 : answer;
}
```

---

### 5-1. `value` 값은 무엇을 나타내는 것일까?

- `value` 은 두 배열의 각각의 요소를 뺀 뒤 나온 값들을 더한 후 2로 나눈 값이다.
- 해당 값은 아래와 같은 방법으로도 구할 수 있다.
  ```javascript
  let queue1_sum = queue1.reduce((acc, cur) => acc + cur);
  let queue2_sum = queue2.reduce((acc, cur) => acc + cur);
  let value = (queue1_sum - queue2_sum) / 2;
  ```
- `value` 값이 양수일 경우 `queue1` 배열의 요소들의 합이 더 크고 음수일 경우 `queue2` 배열의 요소들의 합이 더 크다.
- 나누기 2를 하지 않을 경우엔 수순하게 두 배열의 요소들의 합의 차이이다.
- 그렇다면 나누기 2를 하는 이유는 무엇일까? 직관적으로 이해가 되지 않을 뿐 더러
  어떤 이유인지 확답이 생기지 않는다. 그래서 전체적인 코드를 아래와 같이 수정하였다.
  ```javascript
  function solution(queue1, queue2) {
    let answer = 0;
    const max = queue1.length * 2;
    let value = queue1.reduce((acc, cur) => acc + cur);
    let queue2_sum = queue2.reduce((acc, cur) => acc + cur);
    const helf = (value + queue2_sum) / 2;
    let [i, j] = [0, 0];
    while (value !== helf) {
      if (answer > max * 2) {
        answer = -1;
        break;
      }
      if (value > helf) {
        const v = queue1[i];
        i++;
        value -= v;
        queue2.push(v);
      } else {
        const v = queue2[j];
        j++;
        value += v;
        queue1.push(v);
      }
      answer++;
    }
    return answer;
  }
  ```

---

### 5-2. 두 개의 인데스를 관리

- `i`와 `j`는 각 배열의 인덱스를 관리하기 위해 반든 변수이다.
  ```javascript
  const [i, j] = [0, 0];
  ```
- 반복문을 통해 해당 변수는 `value` 값이 어떤지에 따라 유지되거나 증가된다.
- 내가 수정한 코드를 기준으로 `value` 값이 `helf` 보다 크다면 `i`값이 증가 그렇지 않으면 `j`값이 증가한다.

---

### 5-3. 값을 가져오기만 할 뿐 배열에서 제거하지 않는다.

```javascript
const v = queue1[i];
i++;
value -= v;
queue2.push(v);
```

- 해당 부분이 가장 인상깊었다.
- `i` 번째 위치한 요소를 가져오기만 할 뿐 해당 요소를 배열에서 제거하지 않는다.
- 이렇게 되면 시간 복잡도는 제거를 하지 않기 때문에 O(1)으로 보인다.
- 그리고 배열에서 요소를 가져왓으면 `i`값을 1증가시켜 다음 호출 땐 그 다음 값을 가져온다.
- `value`에 가져온 요소의 값 만큼 빼고 다른 배열에 해당 요소를 넣는다.
- 위의 경우는 `value` 가 `helf` 보다 큰 경우이고 그 반대인 경우엔 두 번째 배열 `j` 번째의 요쇼를 가져오고
  `j`값이 증가한다. 그리고 `value` 에 가져온 요소의 값 만큼 더한다.

---

### 결과

![programmers_make_the_sum_of_two_queues_equal_result3](/image/CodingTest/programmers_make_the_sum_of_two_queues_equal/programmers_make_the_sum_of_two_queues_equal_result3.png)

나의 풀이보다 빠른 실행속도를 확인할 수 있다.

`Array.push()` 메서드는 배열의 마지막 요소를 추가하는 것이기 때문에 시간 복잡도가 O(1)이다. 하지만 `Array.shift()` 메서드는
앞의 요소를 제거하고 모든 요소를 한 칸씩 앞으로 이동하기 때문에 시간 복잡도가 O(n)이다. 그렇기 때문에 `Array.shift()` 메서드를
사용하면 시간이 오래 걸린다. 하지만 위의 풀이에서는 `Array.shift()` 메서드를 사용하여 요소를 제거하지 않고 단순히 다음에 처리해야 할
요소의 인덱스를 따로 관리하고 있기 때문에 시간 측면에서 더 효율적인 모습을 보인다. 큐를 사용하지 않은 멋진 풀이였다.

---

## 6. Conclusion

> 5시간 동안 헤맸던 문제였다. 아예 큐라는 자료구조의 특징과 시간복잡도에 대한 개념이 부족해서 오래 걸렸던 문제라고 생각한다.
> 처음에는 배열의 메서드인 `Array.shift()`와 `Array.push()`를 사용하여 풀었지만 계속 3개의 테스트에서 `시간 초과` 오류가 있었다.
> 계속 코드를 수정하면서 어떻게 하면 시간을 단축 시킬 수 있을까? 라는 생각을 했지만 코드를 계속 수정을 해도 테스트 결과는 달라지지 않았다.
> 그러던 중 스터디원의 코드에서 큐 자료구조를 사용하는 것을 보고 힌트를 얻어 큐 자료구조에 대해 검색을 했더니 데이터를 추가, 제거하는 과정에서
> 시간 복잡도가 다르다는 것을 알게되었다. 그 후 배열을 큐 자료구조로 바꾸어 풀었더니 성공적으로 테스트를 통과하게 되었다.  
> 이번 코딩 테스트 문제 이후 `자료구조와 알고리즘`에 대한 공부 필요성을 느끼게 되었고 매주 토요일은 해당 공부에 집중하는 시간을 가져야
> 겠다고 다짐했다.

---

[👆](#두-큐-합-같게-만들기)

📅 2022-08-20
