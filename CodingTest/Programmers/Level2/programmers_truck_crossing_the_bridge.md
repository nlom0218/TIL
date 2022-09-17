# 다리를 지나는 트럭

## 1. 개요

- 프로그래머스
- Lv.2
- 스택/큐
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42583)

---

## 2. 문제 설명

트럭 여러 대가 강을 가로지르는 일차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 다리에는 트럭이 최대 bridge_length대 올라갈 수 있으며, 다리는 weight 이하까지의 무게를 견딜 수 있습니다. 단, 다리에 완전히 오르지 않은 트럭의 무게는 무시합니다.

예를 들어, 트럭 2대가 올라갈 수 있고 무게를 10kg까지 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.

| 경과 시간 | 다리를 지난 트럭 | 다리를 건너는 트럭 | 대기 트럭 |
| --------- | ---------------- | ------------------ | --------- |
| 0         | []               | []                 | [7,4,5,6] |
| 1~2       | []               | [7]                | [4,5,6]   |
| 3         | [7]              | [4]                | [5,6]     |
| 4         | [7]              | [4,5]              | [6]       |
| 5         | [7,4]            | [5]                | [6]       |
| 6~7       | [7,4,5]          | [6]                | []        |
| 8         | [7,4,5,6]        | []                 | []        |

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리에 올라갈 수 있는 트럭 수 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭 별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

---

### 2-1. 문제 설명 - 제한 조건

- bridge_length는 1 이상 10,000 이하입니다.
- weight는 1 이상 10,000 이하입니다.
- truck_weights의 길이는 1 이상 10,000 이하입니다.
- 모든 트럭의 무게는 1 이상 weight 이하입니다.

---

### 2-2. 문제 설명 - 입출력 예

| bridge_length | weight | truck_weights                   | return |
| ------------- | ------ | ------------------------------- | ------ |
| 2             | 10     | [7,4,5,6]                       | 8      |
| 100           | 100    | [10]                            | 101    |
| 100           | 100    | [10,10,10,10,10,10,10,10,10,10] | 110    |

---

## 3. 문제 풀이

내가 처음으로 푼 날 것의 풀이이다.

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class Queue {
  constructor(array) {
    this.front = null;
    this.rear = null;
    this.weight = 0;
    if (array) {
      array.forEach((item) => {
        this.enqueue(item);
      });
    }
  }
  enqueue(data) {
    const node = new Node(data);
    if (!this.front) this.front = node;
    else this.rear.next = node;
    this.rear = node;
    this.weight += data;
  }
  dequeue() {
    const frontQueue = this.front;
    this.front = this.front.next;
    this.weight -= frontQueue.data;
    return frontQueue.data;
  }
  second_data() {
    if (!this.front) return null;
    return this.front.data;
  }
}

function solution(bridge_length, weight, truck_weights) {
  // 1) 다리를 지나야하는 트럭의 큐와 다리를 지나고 있는 트럭의 큐 만들기
  const truck_queue = new Queue(truck_weights);
  const bridge_queue = new Queue(new Array(bridge_length).fill(0));

  // 2) 다리를 지난 트럭 수의 변수와 전체 걸린 시간의 변수 만들기
  let completed_truck = 0;
  let total_time = 0;

  while (true) {
    // 반복문이 실행될 때 마다 time를 1씩 증가시키기
    total_time++;

    // 3) 다리 위의 맨 앞 트럭을 꺼내오기
    const first_truck = bridge_queue.dequeue();
    // 4) 꺼낸 트럭의 값이 0이 아니라면 completed_truck를 1 증가시키기
    if (first_truck !== 0) completed_truck++;

    // 5) 만약 다리를 지난 트럭의 수가 전체 트럭의 수와 같다면 현재의 시간을 리턴하기
    if (completed_truck === truck_weights.length) return total_time;

    // 6) 다음으로 다리를 건너게 될 트럭을 꺼내오기
    const next_truck = truck_queue.second_data();

    // 7) 현재 다리 위의 트럭의 무게와 다음 차례 트럭의 무게를 합 한 값과 다리가 견딜 수 있는 무게 비교하기
    if (next_truck && bridge_queue.weight + next_truck <= weight)
      bridge_queue.enqueue(truck_queue.dequeue());
    else bridge_queue.enqueue(0);
  }
}
```

기본적으로 `큐(Queue)` 자료구조를 사용하였다.

큐에 대한 자세한 내용과 자바스크립트로 큐 자료구조를 구현하는 것은 [큐(Queue)](DataStructureAlgorithm/DataStructure/Queue.md)에서 확인 가능한다.
기본적인 자료구조에서 다리를 건너고 있는 트럭의 무게를 구하기 위해 `this.weight = 0;`를 추가하였고 가장 앞의 노드를 가져오는 `second_data()`를 추가하였다.

---

### 1) 다리를 지나야하는 트럭의 큐와 다리를 지나고 있는 트럭의 큐 만들기

```javascript
const truck_queue = new Queue(truck_weights);
const bridge_queue = new Queue(new Array(bridge_length).fill(0));
```

`truck_weights`의 배열을 매개변수로 전달하여 `truck_queue`를 만든다.

값이 0이고 길이가 `bridge_length`인 배열을 전달하여 `bridge_queue`를 만든다. 길이를 `bridge_length`로 정한 까닭은
1초에 1만큼의 길이를 갈 수 있기 때문에 하나의 트럭이 다리를 통과하기 위해서는 `bridge_length`만큼의 시간이 필요하다. 또한 배열의 요소를 `0`으로
채웠다. 만약 트럭이 지나고 있는 위치라면 `0`아닌 트럭의 무게 될 것이다.

즉, 다리가 `bridge_length`만큼의 칸으로 이루어진 하나의 띠라고 생각하고 1초가 지날 때 마다 한 칸 씩 앞으로 이동한다고 생각하였다.

---

### 2) 다리를 지난 트럭 수의 변수와 전체 걸린 시간의 변수 만들기

```javascript
let completed_truck = 0;
let total_time = 0;
```

아래 과정에의 반복문에서 조건에 따라 바꾸게 될 변수를 미리 정한다.

`completed_truck`은 다리를 통과한 트럭의 수를 세기 위한 변수이고 `total_time`은 반복문이 한 번 실행될 때 마다 1씩 증가한다.
즉, 모든 트럭이 다리를 통과할 때 까지 1씩 증가하게 된다.

---

### 3) 다리 위의 맨 앞 트럭을 꺼내오기

```javascript
const first_truck = bridge_queue.dequeue();
```

`dequeue()`를 사용하여 다리의 맨 앞에 있는 트럭을 꺼내욘다. 다리의 길이는 1이 감소된 `bridge_length - 1`이 된다.
아래에서 다시 `enqueue()`를 통해 다리를 길이를 `bridge_length`로 만들어준다.

---

### 4) 꺼낸 트럭의 값이 0이 아니라면 completed_truck를 1 증가시키기

```javascript
if (first_truck !== 0) completed_truck++;
```

만약 꺼낸 트럭의 값이 0이 아니라는 것은 트럭의 무게 이므로 트럭이 다리를 건넜다는 것이다. 그렇기 때문에 `completed_truck`의
값을 1 증가시켜야 한다.

---

### 5) 만약 다리를 지난 트럭의 수가 전체 트럭의 수와 같다면 현재의 시간을 리턴하기

```javascript
if (completed_truck === truck_weights.length) return total_time;
```

다리를 지난 트럭의 수를 나타내는 변수 `completed_truck`의 값이 트럭의 무게를 요소로 가진 `truck_weights` 배열의 길이와
같아지는 시점이라면 해당 시점이 모든 트럭이 다리를 모두 통과한 시간이다. 그러므로 `total_time`를 리턴한다.

---

### 6) 다음으로 다리를 건너게 될 트럭을 꺼내오기

```javascript
const next_truck = truck_queue.second_data();
```

`truck_queue` 에서 맨 앞의 값을 가져온다. 위 코드에서는 `second_data()` 함수를 만들어 가져왔지만
굳이 이러지 않고 아래와 같이 가져올 수도 있다.

```javascript
const next_truck = truck_queue.front.data;
```

---

### 7) 현재 다리 위의 트럭의 무게와 다음 차례 트럭의 무게를 합 한 값과 다리가 견딜 수 있는 무게 비교하기

```javascript
if (next_truck && bridge_queue.weight + next_truck <= weight)
  bridge_queue.enqueue(truck_queue.dequeue());
else bridge_queue.enqueue(0);
```

다음 차례의 트럭의 무게까지 다르가 견딜 수 있다면 해당 트럭을 `truck_queue`에서 꺼내 `bridge_queue`에 넣는다.
만약 그렇기 않는다면 `bridge_queue`에는 0를 넣는다.

이런 반복문을 통해 `truck_queue`에서 `bridge_queue`로 차례차례 트럭이 이동하게 된다.

---

### 결과

![programmers_truck_crossing_the_bridge_result1](/image/CodingTest/programmers_truck_crossing_the_bridge/programmers_truck_crossing_the_bridge_result1.png)

---

## 4. Refactoring

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class Queue {
  constructor(array) {
    this.front = null;
    this.rear = null;
    this.weights = 0;
    if (array) {
      array.forEach((item) => this.enqueue(item));
    }
  }
  enqueue(data) {
    const node = new Node(data);
    if (!this.front) this.front = node;
    else this.rear.next = node;
    this.rear = node;
    this.weights += data;
  }
  dequeue() {
    const node = this.front;
    if (!node) {
      this.front = null;
      return null;
    }
    this.front = node.next;
    this.weights -= node.data;
    return node;
  }
}

function solution(bridge_length, weight, truck_weights) {
  const bridge_queue = new Queue(new Array(bridge_length).fill(0));
  let time = 0;
  let i = 0;
  while (i < truck_weights.length) {
    time++;
    bridge_queue.dequeue();
    const n_weight = bridge_queue.weights + truck_weights[i];
    if (n_weight <= weight) {
      bridge_queue.enqueue(truck_weights[i]);
      i++;
    } else {
      bridge_queue.enqueue(0);
    }
  }
  return time + bridge_length;
}
```

굳이 큐 자료구조를 사용하지 않아도 되어 리팩토링에서는 `truck_weights`를 가지고 큐를 만들지 않았다. 그 외의 과정은
순서만 약간 다를 뿐 똑같다고 봐도 무방하다.

단 하나더 만약 `truck_weights`의 길이가 `i`보다 작아졌다는 것은 더 이상 출발 대기 중인 트럭이 없다는 뜻이고 바로 직전에
마지막 트럭이 다리 위로 올라갔다는 것이다. 그렇기 때문에 `time`에 다리의 길이인 `bridge_length`를 더한 값을 리턴해야 한다.

---

### 결과

![programmers_truck_crossing_the_bridge_result2](/image/CodingTest/programmers_truck_crossing_the_bridge/programmers_truck_crossing_the_bridge_result2.png)

---

## 5. 다른 사람 풀이

문제 설명이 잘 되어 있는 풀이를 가져왔다. 아래의 풀이는 큐 자료구조를 사용하지 않고 배열을 사용하고 문제를 풀고 있어 `Array.shift()` 메서드를 사요하고 있다.
배열의 길이가 만 이하라면 굳이 큐 자료구조를 사용하지 않고 `Array.shift()`를 사용해도 괜찮은 듯 하다. 오히려 시간이 더 단축 된 것이 신기할 따름이다.

아무래도 나의 풀이와 리팩토링에서는 큐 자료구조를 생성할 때 반복문을 사용하여 이미 시간 복잡도 `O(n)`를 가지고 있어 비슷한 시간이지 않을까 하는 생각이 든다.
그래도 큐에 대해 공부할 수 있는 시간과 굳이 큐를 사용하지 않고 배열을 통해 문제를 해결할 수 있는 시간을 가져 좋은 문제 풀이였다고 생각한다.

마지막으로 다리에 트럭이 추가될 수 없는 경우 시간을 점프한다는 것과 트럭이 나갈 시간을 기존 까지 흐름 시간을 더해 관리한 것이 멋지게 느껴졌다.

```javascript
function solution(bridge_length, weight, truck_weights) {
  // '다리'를 모방한 큐에 간단한 배열로 정리 : [트럭무게, 얘가 나갈 시간].
  let time = 0,
    qu = [[0, 0]],
    weightOnBridge = 0;

  // 대기 트럭, 다리를 건너는 트럭이 모두 0일 때 까지 다음 루프 반복
  while (qu.length > 0 || truck_weights.length > 0) {
    // 1. 현재 시간이, 큐 맨 앞의 차의 '나갈 시간'과 같다면 내보내주고,
    //    다리 위 트럭 무게 합에서 빼준다.
    if (qu[0][1] === time) weightOnBridge -= qu.shift()[0];

    if (weightOnBridge + truck_weights[0] <= weight) {
      // 2. 다리 위 트럭 무게 합 + 대기중인 트럭의 첫 무게가 감당 무게 이하면
      //    다리 위 트럭 무게 업데이트, 큐 뒤에 [트럭무게, 이 트럭이 나갈 시간] 추가.
      weightOnBridge += truck_weights[0];
      qu.push([truck_weights.shift(), time + bridge_length]);
    } else {
      // 3. 다음 트럭이 못올라오는 상황이면 얼른 큐의
      //    첫번째 트럭이 빠지도록 그 시간으로 점프한다.
      //    참고: if 밖에서 1 더하기 때문에 -1 해줌
      if (qu[0]) time = qu[0][1] - 1;
    }
    // 시간 업데이트 해준다.
    time++;
  }
  return time;
}
```

---

### 결과

![programmers_truck_crossing_the_bridge_result3](/image/CodingTest/programmers_truck_crossing_the_bridge/programmers_truck_crossing_the_bridge_result3.png)

---

## 6. Conclusion

> 문제를 풀 때 실수를 많이 해서 시간이 오래 걸렸다. 어떤 방법으로 풀어야 할지에 대한 감을 이미 잡혔지만 변수명을 헷갈리는 문제와 더불어
> 큐의 자료를 가져와 더하는 과정에서 잘못된 데이터를 사용하여 값이 이상해는 경우가 있었다. 또한 `while`문의 조건을 명확히 하지 않아
> 루프를 빠져나오지 못하는 경우도 생겼다. 아직 꼼꼼하지 못한 것 같아 더 노력을 해야 할 듯 하다. 천천히 하지만 확실히 한 번에 해결 할 수 있도록!

---

📅 2022-09-17
