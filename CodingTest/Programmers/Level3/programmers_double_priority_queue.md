# 이중우선순위큐

## 1. 개요

- 프로그래머스
- Lv.3
- 힙(Heap)
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42628)

---

## 2. 문제 설명

이중 우선순위 큐는 다음 연산을 할 수 있는 자료구조를 말합니다.

| 명령어 | 수신 탑(높이)                  |
| ------ | ------------------------------ |
| 숫자   | 큐에 주어진 숫자를 삽입합니다. |
| D 1    | 큐에서 최댓값을 삭제합니다.    |
| D -1   | 큐에서 최솟값을 삭제합니다.    |

이중 우선순위 큐가 할 연산 operations가 매개변수로 주어질 때, 모든 연산을 처리한 후 큐가 비어있으면 [0,0] 비어있지 않으면 [최댓값, 최솟값]을 return 하도록 solution 함수를 구현해주세요.

---

### 2-1. 문제 설명 - 제한사항

- operations는 길이가 1 이상 1,000,000 이하인 문자열 배열입니다.
- operations의 원소는 큐가 수행할 연산을 나타냅니다.
  - 원소는 “명령어 데이터” 형식으로 주어집니다.- 최댓값/최솟값을 삭제하는 연산에서 최댓값/최솟값이 둘 이상인 경우, 하나만 삭제합니다.
- 빈 큐에 데이터를 삭제하라는 연산이 주어질 경우, 해당 연산은 무시합니다.

---

### 2-2. 문제 설명 - 입출력 예

| operations                                                                  | return     |
| --------------------------------------------------------------------------- | ---------- |
| ["I 16", "I -5643", "D -1", "D 1", "D 1", "I 123", "D -1"]                  | [0,0]      |
| ["I -45", "I 653", "D 1", "I -642", "I 45", "I 97", "D 1", "D -1", "I 333"] | [333, -45] |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1

16과 -5643을 삽입합니다.

- 최솟값을 삭제합니다. -5643이 삭제되고 16이 남아있습니다.
- 최댓값을 삭제합니다. 16이 삭제되고 이중 우선순위 큐는 비어있습니다.
- 우선순위 큐가 비어있으므로 최댓값 삭제 연산이 무시됩니다.
- 123을 삽입합니다.
- 최솟값을 삭제합니다. 123이 삭제되고 이중 우선순위 큐는 비어있습니다.

따라서 [0, 0]을 반환합니다.

입출력 예 #2

- 45와 653을 삽입후 최댓값(653)을 삭제합니다. -45가 남아있습니다.
- 642, 45, 97을 삽입 후 최댓값(97), 최솟값(-642)을 삭제합니다. -45와 45가 남아있습니다.
- 333을 삽입합니다.

이중 우선순위 큐에 -45, 45, 333이 남아있으므로, [333, -45]를 반환합니다.

---

## 3. 문제 풀이

```javascript
// 1) 최소 힙, 최대 힙 자료 구조 구현하기
class MinHeap {
  constructor() {
    this.heap = [null];
  }

  pull(arr) {
    this.heap = [null];
    arr.forEach((item) => {
      if (item) this.push(item);
    });
  }

  push(value) {
    this.heap.push(value);
    let cur_index = this.heap.length - 1;
    let parents_index = Math.floor(cur_index / 2);

    while (cur_index !== 1 && this.heap[parents_index] > value) {
      [this.heap[parents_index], this.heap[cur_index]] = [
        this.heap[cur_index],
        this.heap[parents_index],
      ];
      cur_index = parents_index;
      parents_index = Math.floor(cur_index / 2);
    }
  }

  pop() {
    const value = this.heap[1];

    if (this.heap.length <= 2) this.heap = [null];
    else this.heap[1] = this.heap.pop();

    let cur_index = 1;
    let left_index = cur_index * 2;
    let right_index = cur_index * 2 + 1;

    if (!this.heap[left_index]) return value;
    if (!this.heap[right_index]) {
      if (this.heap[left_index] < this.heap[cur_index]) {
        [this.heap[left_index], this.heap[cur_index]] = [
          this.heap[cur_index],
          this.heap[left_index],
        ];
      }
      return value;
    }

    while (
      this.heap[left_index] < this.heap[cur_index] ||
      this.heap[right_index] < this.heap[cur_index]
    ) {
      let min_index =
        this.heap[left_index] > this.heap[right_index]
          ? right_index
          : left_index;

      [this.heap[min_index], this.heap[cur_index]] = [
        this.heap[cur_index],
        this.heap[min_index],
      ];

      cur_index = min_index;
      left_index = cur_index * 2;
      right_index = cur_index * 2 + 1;
    }
    return value;
  }
}

class MaxHeap {
  constructor() {
    this.heap = [null];
  }

  pull(arr) {
    this.heap = [null];
    arr.forEach((item) => {
      if (item) this.push(item);
    });
  }

  push(value) {
    this.heap.push(value);
    let cur_index = this.heap.length - 1;
    let parents_index = Math.floor(cur_index / 2);

    while (cur_index !== 1 && this.heap[parents_index] < value) {
      [this.heap[parents_index], this.heap[cur_index]] = [
        this.heap[cur_index],
        this.heap[parents_index],
      ];
      cur_index = parents_index;
      parents_index = Math.floor(cur_index / 2);
    }
  }

  pop() {
    const value = this.heap[1];

    if (this.heap.length <= 2) this.heap = [null];
    else this.heap[1] = this.heap.pop();

    let cur_index = 1;
    let left_index = cur_index * 2;
    let right_index = cur_index * 2 + 1;

    if (!this.heap[left_index]) return value;
    if (!this.heap[right_index]) {
      if (this.heap[left_index] > this.heap[cur_index]) {
        [this.heap[left_index], this.heap[cur_index]] = [
          this.heap[cur_index],
          this.heap[left_index],
        ];
      }
      return value;
    }

    while (
      this.heap[left_index] > this.heap[cur_index] ||
      this.heap[right_index] > this.heap[cur_index]
    ) {
      let max_index =
        this.heap[left_index] < this.heap[right_index]
          ? right_index
          : left_index;

      [this.heap[max_index], this.heap[cur_index]] = [
        this.heap[cur_index],
        this.heap[max_index],
      ];

      cur_index = max_index;
      left_index = cur_index * 2;
      right_index = cur_index * 2 + 1;
    }
    return value;
  }
}

function solution(operations) {
  // 2) 최소 힙, 최대 힙 만들기
  const min_heap = new MinHeap();
  const max_heap = new MaxHeap();
  operations.forEach((item) => {
    // 3) 각각의 요소를 배열로 만들어 첫 번째 요소를 order, 두 번째 요소를 value에 할당하기
    const [order, value] = item.split(" ");

    // 4) 첫 번째 요소가 I라면 두 개의 힙에 value를 추가하기
    if (order === "I") {
      min_heap.push(Number(value));
      max_heap.push(Number(value));
    } else {
      // 5) 첫 번째 요소가 D이고, value가 1이라면 최대 힙에서 값을 제거하고 새로운 최소 힙 만들기
      if (value === "1") {
        max_heap.pop();
        min_heap.pull(max_heap.heap);
      } else {
        // 6) 힙의 종류만 다를 뿐 5) 과정과 같다.
        min_heap.pop();
        max_heap.pull(min_heap.heap);
      }
    }
  });

  // 7) 최종적으로 남은 힙을 heap 변수에 할당하기
  const heap = min_heap.heap.slice(1);

  // 8) 만약 heap 배열의 길이가 0이라면 [0, 0]를 반환하기
  if (heap.length === 0) return [0, 0];

  // 9) heap 배열의 최대값과 최소값을 차례대로 배열에 담아 반환하기.
  return [Math.max(...heap), Math.min(...heap)];
}
```

---

### 1) 최소 힙, 최대 힙 자료 구조 구현하기

구현한 코드는 너무 길어 생략을 한다. 자바스크립트을 이용한 힙 자료 구조 구현은 아래의 링크에서 확인할 수 있다.

[힙(Heap)](DataStructureAlgorithm/DataStructure/Heap.md)

다만 최소 힙과 최대 힙은 서로 독립적인 것이 아니라 한쪽의 힙에서 값이 삭제되면 다른 한쪽에서도 값이 삭제되어야 한다.
이를 위해 `Heap.pull()` 메서드를 구현해보았다.

```javascript
  pull(arr) {
    this.heap = [null];
    arr.forEach((item) => {
      if (item) this.push(item);
    });
  }
```

해당 메서드는 다른 한 쪽의 힙에서 값이 삭제될 때 실행이 되며 배열을 매개변수로 받는다.

먼저 `this.heap`를 `[null]`로 선언하여 초기화를 한다. 그 후 매개변수로 받은 배열을 순회하면서 `this.heap`에
요소를 추가한다.

---

### 2) 최소 힙, 최대 힙 만들기

```javascript
const min_heap = new MinHeap();
const max_heap = new MaxHeap();
```

최소 힙, 최대 힙 두 개 모두를 다루기 때문에 두 개의 힙을 생성한다.

---

### 3) 각각의 요소를 배열로 만들어 첫 번째 요소를 order, 두 번째 요소를 value에 할당하기

```javascript
const [order, value] = item.split(" ");
```

`operations` 배열의 각 요소를 공백을 기준으로 나누어 배열을 만든 후 차례대로 `order`, `value`에 할당한다.

---

### 4) 첫 번째 요소가 I라면 두 개의 힙에 value를 추가하기

```javascript
if (order === "I") {
  min_heap.push(Number(value));
  max_heap.push(Number(value));
}
```

만약 명령어가 "I"라면 `value`를 최소 힙, 최대 힙에 추가한다.

---

### 5) 첫 번째 요소가 D이고, value가 1이라면 최대 힙에서 값을 제거하고 새로운 최소 힙 만들기

```javascript
if (value === "1") {
  max_heap.pop();
  min_heap.pull(max_heap.heap);
}
```

만약 명령어가 "D"이고 `value`가 "1"이라면 최대값을 제거하는 것이므로 최대 힙에서 요소를 제거한다.
그렇게 되면 `max_heap.heap` 배열에는 최대값 하나가 제거된 상태이다. 이는 최소 힙과 같은 숫자들을 가지고 있지 않는
상태이다. 그러므로 `min_heap`에 `pull(max_heap.heap)` 메서드를 실행한다.

---

### 6) 힙의 종류만 다를 뿐 5) 과정과 같다.

```javascript
min_heap.pop();
max_heap.pull(min_heap.heap);
```

`5. 첫 번째 요소가 D이고, value가 1이라면 최대 힙에서 값을 제거하고 새로운 최소 힙 만들기`과정과 조거만 다를 뿐 같은 과정이다.

---

### 7) 최종적으로 남은 힙을 heap 변수에 할당하기

```javascript
const heap = min_heap.heap.slice(1);
```

반복문이 종료되면 `min_heap.heap` 배열과 `max_heap.heap` 배열에는 같은 요소가 들어있다. 물론 순서를 다르다.
하지만 어차피 최대값과 최소값만 구하면 되니까 둘 중 하나의 배열만 참고해도 된다.

하지만 `min_heap.heap` 배열의 0번째 요소는 항상 `null`이므로 이를 제거하기 위한 `Array.slice()` 메서드를
실행한다.

---

### 8) 만약 heap 배열의 길이가 0이라면 [0, 0]를 반환하기

```javascript
if (heap.length === 0) return [0, 0];
```

큐가 비어있으면 `[0, 0]`를 반환한다.

---

### 9) heap 배열의 최대값과 최소값을 차례대로 배열에 담아 반환하기.

```javascript
return [Math.max(...heap), Math.min(...heap)];
```

큐에 요소가 있으면 요소들 중의 최대값과 최소값을 차례대로 배열에 담아 반환한다.

---

### 결과

![programmers_double_priority_queue_result](/image/CodingTest/programmers_double_priority_queue/programmers_double_priority_queue_result1.png)

---

## 4. 다른 사람 풀이

질문 페이지에서 `howdyfrom2019`님께서 올린 풀이를 가져왔다. 힙을 사용하고 있지만 나처럼 두 개의 힙을 모두 사용하지 않고
최소 힙만 사용하면서 가장 최하단 노드의 값을 비교하여 최대값을 제거하는 방법을 사용하고 있다. 최소 힙 자료구조를 그대로 가져오지
않고 내가 작성한 자료구조에 추가하였다.

```javascript
class MinHeap {
  constructor() {
    this.heap = [null];
  }

  push(value) {
    this.heap.push(value);
    let cur_index = this.heap.length - 1;
    let parents_index = Math.floor(cur_index / 2);

    while (cur_index !== 1 && this.heap[parents_index] > value) {
      this._swap(cur_index, parents_index);
      cur_index = parents_index;
      parents_index = Math.floor(cur_index / 2);
    }
  }

  pop(isTopPop) {
    if (this.heap.length === 1) return;
    if (this.heap.length === 2) return this.heap.pop();

    // 1) 최소값 삭제가 아닌 최대값 삭제일 때
    if (!isTopPop) {
      const parents_index = Math.floor((this.heap.length - 1) / 2);
      const last_leaf = this.heap.slice(parents_index);
      const max = Math.max(...last_leaf);
      this._swap(parents_index + last_leaf.indexOf(max), this.heap.length - 1);
      return this.heap.pop();
    }

    const value = this.heap[1];
    this.heap[1] = this.heap.pop();

    let cur_index = 1;
    let left_index = cur_index * 2;
    let right_index = cur_index * 2 + 1;

    if (!this.heap[left_index]) return value;
    if (!this.heap[right_index]) {
      if (this.heap[left_index] < this.heap[cur_index]) {
        this._swap(left_index, cur_index);
      }
      return value;
    }

    while (
      this.heap[left_index] < this.heap[cur_index] ||
      this.heap[right_index] < this.heap[cur_index]
    ) {
      let min_index =
        this.heap[left_index] > this.heap[right_index]
          ? right_index
          : left_index;
      this._swap(min_index, cur_index);
      cur_index = min_index;
      left_index = cur_index * 2;
      right_index = cur_index * 2 + 1;
    }
    return value;
  }

  // 2) 서로의 위치를 바꾸기
  _swap(a, b) {
    [this.heap[a], this.heap[b]] = [this.heap[b], this.heap[a]];
  }
}

function solution(operations) {
  const min_heap = new MinHeap();
  operations.forEach((item) => {
    const [order, value] = item.split(" ");
    if (order === "I") {
      min_heap.push(Number(value));
    } else {
      min_heap.pop(Number(value) < 0);
    }
  });

  const heap = min_heap.heap.slice(1);
  if (heap.length === 0) return [0, 0];

  return [Math.max(...heap), Math.min(...heap)];
}
```

---

### 1) 최소값 삭제가 아닌 최대값 삭제일 때

```javascript
if (!isTopPop) {
  const parents_index = Math.floor((this.heap.length - 1) / 2);
  const last_leaf = this.heap.slice(parents_index);
  const max = Math.max(...last_leaf);
  this._swap(parents_index + last_leaf.indexOf(max), this.heap.length - 1);
  return this.heap.pop();
}
```

힙 자료 구조를 구현할 때 해당 부분을 눈여겨 보면 좋을 듯하다.

위의 코드는 최소 힙에서 최소값을 제거하는 것이 아닌 최대값을 제거하는 로직이다. 일단 `isTopPop`이라는 변수가 주어지는 데
해당 값이 `true`일 땐 최소값이 제거가 되고 `false`일 땐 최대값이 제거가 된다.

먼저 마지막 요소의 부모 인덱스를 구해야한다. 최하단 노드를 구하기 위해서이다. 최하단 depth의 노드들 중에 최대값이 있다는 것이
보장이 된다. 최소 힙의 특성 상 값이 커질 수록 하단에 존재하기 때문이다.

마지막 요소의 부모 인덱스를 구했으면 부모 인덱스 부터 마지막 인덱스 까지 `Array.slice()` 메서드를 사용하여 자른다.
그렇다면 `last_leaf` 배열에는 최대값이 있고 `Math.max()` 메서드를 사용하여 그 값을 찾는다.

이후 `this._swap()`를 사용하여 최대값이 있는 인덱스의 값과 마지막 인덱스의 값의 위치를 서로 바꾼 후 `Array.pop()`
메서드를 사용하여 마지막 요소를 제거한다.

---

### 2) 서로의 위치를 바꾸기

```javascript
_swap(a, b) {
[this.heap[a], this.heap[b]] = [this.heap[b], this.heap[a]];
}
```

힙 자료구조를 구현하다 보면 서로의 위치를 바꾸는 로직이 많이 나타난다. 이를 위해 따로 함수를 작성해 둔 것이 인상이 깊었다.

---

### 결과

![programmers_double_priority_queue_result1](/image/CodingTest/programmers_double_priority_queue/programmers_double_priority_queue_result2.png)

---

## 5. Conclusion

> 다른 사람 풀이 보다 질문에 멋진 풀이가 있어 가져왔다. 뿐만 아니라 질문에는 다양한 언어를 사용하는 사용자가 올린
> 글들이 있었는데 몇 개를 읽어보면 왜 해당 문제가 Level3에 있는지 모르겠다는 사람들이 많았다. 어느정도 공감이 되지만
> 타 언어에는 힙 자료구조가 내장함수로 구현되어 있어 편하게 힙을 사용할 수 있지만 자바스크립트 같은 경우에는 처음부터
> 끝까지 구현을 해야하기 때문에 결코 쉬운 문제는 아니었다고 생각한다.  
> 그래서 아쉬운 점은 왜 자바스크립트에는 힙이 내장되어 있지 않은 것이다.ㅠㅠ 그렇다고 힙을 사용하기엔 불편한 것은 아니다
> 오히려 자료구조에 대한 공부가 잘 되었다고 생각한다.

---

📅 2022-09-12
