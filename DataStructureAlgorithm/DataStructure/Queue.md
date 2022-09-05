# 큐(Queue)

## 0. 요약

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
    this.size = 0;
    if (array) {
      array.forEach((item) => {
        this.enqueue(item);
      });
    }
  }

  // 큐가 비어있는지 확인하기
  isEmpty() {
    return !Boolean(this.size);
  }

  // 큐에 원소 추가하기
  enqueue(data) {
    const node = new Node(data);
    if (this.isEmpty()) this.front = node;
    else this.rear.next = node;
    this.rear = node;
    this.size++;
  }

  // 큐에 원소 제거하기
  dequeue() {
    const frontQueue = this.front;
    if (this.isEmpty()) return;
    this.front = this.front.next;
    this.size--;
    if (this.size === 0) this.rear = null;
    return frontQueue;
  }

  // 큐에 해당 원소가 존재하는지 확인하여 존재하면 해당 원소를 반환하기
  contains(data) {
    let cur = this.front;
    let node;
    while (!node) {
      if (!cur) return;
      if (cur.data === data) node = cur;
      else cur = cur.next;
    }
    return node;
  }

  // 큐의 모든 원소 출력하기
  display() {
    let cur = this.front;
    let index = 0;
    while (index < this.size) {
      console.log(`${index}. ${cur.data}`);
      cur = cur.next;
      index++;
    }
  }
}
```

---

## 1. 개요

JAVA 또는 파이썬 같은 경우 내장 라이브러리에 큐를 제공하고 있지만 자바스크립트 경우엔 그렇지 않다. 그래서 자바스크립트에서
큐를 사용하기 위해서는 직접 큐를 작성해야한다.

사실 자바스크립트에서 큐를 사용하지 않고 배열을 이용하여 큐의 기능을 사용할 수 있다. `Array.shift()` 메서드와 `Array.pop()` 메서드를
사용하면 된다. 하지만 이렇게 되면 시간복잡도에서 큐와 큰 차이를 보이게 된다. 큐에서의 원소 제거는 시간복잡도 O(1)를 따르게 되는데 배열에서의
`Array.shift()` 메서드를 사용한 원소 제거는 시간복자도 O(n)를 따르게 되기 때문이다.

`Array.shift()` 메서드의 내부 로직은 다음과 같다.

- 배열의 첫 번째 원소에 접근하여 해당 값을 반환하고 배열에서 제거
- 기존 배열에서 첫 번째 원소의 값은 사라졌지만 공간은 차지하고 있음
- `Array.shift()` 메서드는 계속해서 첫 번째 원소에 접근할 예정
- 따라서 나머지 배열의 원소들을 앞으로 한 칸씩 당겨주는 과정 필요

즉 마지막의 과정에 대한 연산이 추가적으로 요구된다. 이러한 추가적인 연산으로 인해 코딩 테스트 문제에서 데이터의 양이 매우 큰 경우, 시간
복잡도를 매우 세세하게 관리해야 하는 경우에는 배열로 이용하여 문제를 풀게 된다면 통과할 수 없는 경우가 있을 수 있다. 이런 경우 큐를 이용하여
더 빠른 시간 복잡도로 문제에 접근한다면 통과할 수 있는 확률이 올라갈 것이다.

참고할 만한 코딩 테스트 문제

- [두 큐 합 같게 만들기](/CodingTest/Programmers/Level2/programmers_make_the_sum_of_two_queues_equal.md)
- [기능개발](/CodingTest/Programmers/Level2/programmers_feature_development.md)
- [프린터](/CodingTest/Programmers/Level2/programmers_printer.md)

---

## 2. 큐란?

FIFO(First in, First out) 원칙으로 만들어진 자료구조로 자바스크립트 엔진에서 비동기 함수 실행시 콜백들이
대기열로 들어오는 `Task queue`가 대표적인 예이다.

![Queue](https://images.velog.io/images/gillog/post/63841ffd-fffc-4825-97ae-7ebac63af39a/bandicam%202020-10-13%2010-49-20-585.png)

큐는 놀이공원에서 먼저 온 사람이 놀이기구를 먼저 타는 것과 마찬가지로 먼저 넣은 데이터가 먼저 나간다. 큐의 가장 첫 번째 원소를 `front`,
끝 원소를 `rear`로 부르며 데이터의 접근방법은 `front`와 `rear`로만 가능하다. 또한 `rear`에서는 삽입 연산만 일어나고,
`front` 에서는 삭제 연산만 일어난다.

큐에서 사용되는 메서드는 아래와 같이 정의할 수 있다.

- dequeue: `front`를 제거하고 반환한다.
- enqueue: 큐에 원소를 추가한다. 이때 해당 원소는 `rear`가 된다.
- contains: 해당 원소가 큐에 존재하는지 확인하여 존재하면 해당 원소를 반환한다.

---

## 3. 연결 리스트로 큐 구현하기

연결 리스트를 통해 큐를 구현한다.

---

### 3-1. 노드와 큐의 기본 구조

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class Queue {
  constructor() {
    this.front = null;
    this.rear = null;
    this.size = 0;
  }
}
```

노드와 큐의 기본 구조는 연결 리스트에서 다루었던 내용과 같다.

---

### 3-2. 큐가 비어있는지 확인하기

```javascript
class Queue {
  // ...
  isEmpty() {
    return !Boolean(this.size);
  }
}
```

자바스크립트에서 숫자 `0`은 불리언으로 변환했을 때 `false`를 가리킨다. 그렇기 때문에 앞에 `!`를 붙여
`ture`로 바꾸어 `isEmpty` 함수가 큐에 원소가 없을 경우 `true`를 반환하게 한다.

---

### 3-3. 큐에 원소 추가하기

```javascript
class Queue {
  // ...
  enqueue(data) {
    const node = new Node(data);
    if (this.isEmpty()) this.front = node;
    else this.rear.next = node;
    this.rear = node;
    this.size++;
  }
}
```

- 큐에 원소가 없을 경우 새롭게 생성된 `Node`를 `fornt`로 지정한다.
- 큐에 원소가 있을 경우 기존 큐의 `rear.next`를 새롭게 생성된 `Node`로 지정한다.
  > `front`와 `rear`도 `Node`이다. 그래서 data 속성과 next 속성을 가진다.
- 마지막으로 새롭게 생성된 `Node`를 `rear`로 바꾸면 된다.(`size`도 하나 올리자.)

---

### 3-4. 큐에 원소 제거하기

```javascript
class Queue {
  // ...
  dequeue() {
    const frontQueue = this.front;
    if (this.isEmpty()) return;
    this.front = this.front.next;
    this.size--;
    if (this.size === 0) this.rear = null;
    return frontQueue;
  }
}
```

- 큐의 `front`를 `frontQueue`에 저장한다.
- 큐가 비어있다면 아무것도 리턴하지 않고 함수를 종료한다.
- 기존 큐의 `front`를 `front.next`로 지정한다.
- `size`를 하나 감소시킨다.
- 만약 `size`가 0이 되면 큐에 원소가 없는 것이므로 `rear`를 `null`로 바꾸어야 한다.
- 마지막으로 큐에서 제거한 원소인 `frontQueue`를 반환한다.

---

### 3-5. 큐에 해당 원소가 존재하는지 확인하여 존재하면 해당 원소를 반환하기

```javascript
class Queue {
  // ...
  contains(data) {
    let cur = this.front;
    let node;
    while (!node) {
      if (!cur) return;
      if (cur.data === data) node = cur;
      else cur = cur.next;
    }
    return node;
  }
}
```

- `cur`를 큐의 `front`로 지정한다.
- 리턴하게 될 변수 `node`를 선언한다.
- `node`에 큐의 원소가 할당될 때 까지 반복문을 실행한다.
- 만약 `cur`이 `null`이라면 해당 원소가 없기 때문에 아무것도 반환하지 않고 함수를 종료한다.
- `cur`이 `null`이 된다는 것은 큐의 모든 원소를 확인했다는 것이다.
- `cur.data`와 `contains` 함수의 인자로 받은 `data`가 같다면 `node`에 `cur`를 할당한다.
- `node`에 `cur`를 할당하게 되면 `node`는 `null`이 아니기 때문에 반복문은 더 이상 실행되지 않는다.
- 하지만 같지 않다면 `cur`를 `cur.next`로 바꾸어 반복문을 계속 실행한다.

---

### 3-6. 큐의 모든 원소 출력하기

```javascript
class Queue {
  // ...
  display() {
    let cur = this.front;
    let index = 0;
    while (index < this.size) {
      console.log(`${index}. ${cur.data}`);
      cur = cur.next;
      index++;
    }
  }
}
```

---

### 3-7. 배열을 넘겨 큐 생성하기

```javascript
class Queue {
  constructor() {
    this.front = null;
    this.rear = null;
    this.size = 0;
    if (array) {
      array.forEach((item) => {
        this.enqueue(item);
      });
    }
  }
}
```

- `constructor` 함수를 위와 같이 수정하여 새로운 큐를 만들 때 `array`가 전달되면
  `enqueue` 함수가 실행되게 한다.
- `Array.forEach()` 메서드를 사용하여 배열의 모든 요소를 큐의 원소로 넣는다.(앞에서 부터 순서대로)

---

## 4. 큐의 시간 복잡도

- enqueue(추가): 큐에 데이터를 추가하는 것은 큐의 맨 뒤 원소(rear)에 하나를 추가하면 된다. 큐에 무수히 많은
  데이터가 있더라도 하나의 데이터 삽입은 한 번이기 때문에 시간 복잡도는 O(1)이 된다.
- dequeue(삭제): FIFO에 따라 데이터가 삭제될 때 큐의 맨 앞 원소(front)가 삭제되는 것이므로 큐의 크기에 상관없이
  시간 복잡도는 O(1)를 가지게 된다.
- contains(접근과 검색): 큐의 원소 접근은 큐의 맨 앞 원소(front)에서만 가능하다. 즉, n번째 원소의 접근은
  첫 번째 원소(front)부터 순회하므로 시간 복잡도는 O(n)를 가지게 된다.

| Big-O (시간복잡도) | 삽입 | 삭제 | 접근 | n번째 접근 | 검색 |
| ------------------ | ---- | ---- | ---- | ---------- | ---- |
| 큐 (Queue)         | O(1) | O(1) | O(1) | O(n)       | O(n) |

---

## 5. Conclusion

> 큐를 자바스크립트로 구현 해보면서 어떤 자료구조인지 공부해보았다. 확실히 배열과는 다른 특징이 있다. 원소의 추가, 삭제는
> 빠르지만 원소에 접근, 검색은 느리기 때문에 상황에 맞는 필요한 자료구조를 잘 선택을 해야겠다. 이전에 코딩 테스트 공부를 하면서
> 풀었던 문제를 조금 수정하였다. 그 이유는 그 땐 단순히 블로그에 있는 큐 생성하는 클래스를 복사붙여 넣기를 했기 때문이다.
> 내가 스스로 만든(물론 블로그의 힘을 빌린긴 했지만..) 큐 클래스를 사용하고 문제도 잘 통과되는 것을 보니 재밌고 뿌듯하다.

---

## 참고

[[자료구조] JS로 구현하는 큐 (Queue)](https://velog.io/@longroadhome/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-JS%EB%A1%9C-%EA%B5%AC%ED%98%84%ED%95%98%EB%8A%94-.%ED%81%90-Queue)  
[[알고리즘, 자료구조] 자바스크립트로 큐(Queue)구현하기 (+개념이해)](https://algoroot.tistory.com/55)

---

📅 2022-09-02
