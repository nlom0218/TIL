# 스택(Stack)

## 0. 요약

### 0-1. 연결 리스트로 스택 구현하기

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class Stack {
  constructor() {
    this.top = null;
    this.bottom = null;
    this.size = 0;
  }

  // 스택에 원소 추가하기
  push(data) {
    const node = new Node(data);
    if (this.size === 0) this.bottom = node;
    else node.next = this.top;
    this.top = node;
    this.size++;
  }

  // 스텍에 원소 제거하기
  pop() {
    const topNode = this.top;
    if (!topNode) return null;
    this.top = topNode.next;
    this.size--;
    if (this.size === 0) this.bottom = null;
    return topNode;
  }

  // 스택에 쌓인 가장 마지막 원소 가져오기
  peek() {
    return this.top;
  }
}
```

---

### 0-2. 배열로 스택 구현하기

```javascript
class Stack {
  constructor() {
    this.arr = [];
    this.size = 0;
  }

  // 스택에 원소 추가하기
  push(data) {
    this.arr.push(data);
    this.size++;
  }

  // 스텍에 원소 제거하기
  pop() {
    if (this.size === 0) return null;
    this.size--;
    return this.arr.pop();
  }

  // 스택에 쌓인 가장 마지막 원소 가져오기
  peek() {
    if (this.size === 0) return null;
    return this.arr[this.size - 1];
  }
}
```

---

## 1. 개요

자바스크립트를 처음 배울 때 자바스크립트에서 모든 함수 호출은 스택 자료 구조로 이루어졌다는 것을 배운 기억이 있다.
하지만 스택이란 정확히 어떤 자료 구조인지는 공부하지 않았다. 이번 기회에 스택 자료 구조에 대해 공부하며 어떤
특징이 있는지 자세히 살펴보고자 한다.

참고할 만한 코딩 테스트 문제

- [올바른 괄호](/CodingTest/Programmers/Level2/programmers_correct_parenthesis.md)

---

## 2. 스택이란?

스택이란 단어는 "차곡 차곡 쌓여진 더미"를 의미하며 프로그래밍에서는 데이터를 한 줄로 쌓아서 마치 탑을 쌓듯
추가하는 형태의 자료 구조를 뜻한다. LIFO(Last In First Out, 후입선출)의 구조를 가진다.

![Stack](https://blog.kakaocdn.net/dn/CSWsW/btq2t9Wc0um/qTQgiVXqjxA0l9weuUKTH1/img.png)

스택은 입구가 하나 뿐인 주차장을 생각하면 이해하기 쉽다. 주차를 하기 위해 주차장에 들어선 자동차는 앞으로 쭉
직진을 하다 앞에 자동차가 있으면 멈추어 주차를 한다. 그렇게 자동차들이 주차를 하게 되면 맨 마지막에 들어온
자동차가 주차장에서 벗어나지 않으면 다른 자동차들 또한 벗어날 수 없다. 즉, 마지막에 들어온 자동차가 먼저
주차장에서 벗어나야 한다.

이러한 스택의 아래와 같은 곳에서 사용된다.

- 재귀함수
- DFS(깊이우선탐색)
- 계산기
- 괄호 정합성 판단

스택에서 사용되는 메서드와 프로퍼티는 아래와 같다.

- push: 스택에 새 원소 추가
- pop: 스택의 맨 위에 있는 원소 삭제
- peek: 스택의 맨 위에 있는(top) 원소 확인 및 가져오기
- top: 스택에 마지막으로 추가된 원소
- bottom: 스택에 처음으로 추가된 원소

---

## 3. 연결 리스트로 스택 구현하기

연결 리스트를 통해 스택을 구현한다.

---

### 3-1. 노드와 스택의 기본 구조

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class Stack {
  constructor() {
    this.top = null;
    this.bottom = null;
    this.size = 0;
  }
}
```

---

### 3-2. 스택에 원소 추가하기

```javascript
class Stack {
  // ...
  push(data) {
    const node = new Node(data);
    if (this.size === 0) this.bottom = node;
    else node.next = this.top;
    this.top = node;
    this.size++;
  }
}
```

- 만약 스택에 원소가 없을 경우 새롭게 생성된 `Node`를 `bottom`으로 지정한다.
- 스택에 원소가 있을 경우 현재의 `top`의 `next`를 새롭게 생성된 `Node`로 지정한다.
- 이후 스택의 `top`를 새롭게 생성된 `Node`로 바꾼다.
- 스택에 원소가 하나 추가되었기 때문에 `size`를 1증가 시킨다.

---

### 3-3. 스텍에 원소 제거하기

```javascript
class Stack {
  // ...
  pop() {
    const topNode = this.top;
    if (!topNode) return null;
    this.top = topNode.next;
    this.size--;
    if (this.size === 0) this.bottom = null;
    return topNode;
  }
}
```

- 스택의 `top`를 `topNode`에 저장한다.
- 만약 `topNode`가 없다면 스택에 원소가 없기 때문에 `null`를 반환한다.
- 현재 스택의 `top`를 제거할 원소인 `topNode`의 `next`로 지정한다.
- 스택의 원소를 하나 제거하기 때문에 `size`를 하나 감소시킨다.
- `size`가 감소되어 0이 되었다면 스택에 원소가 없는 것이므로 `bottom`를 `null`로 바꾼다.
- 제거한 원소인 `topNode`를 반환한다.

---

### 3-4. 스택에 쌓인 가장 마지막 원소 가져오기

```javascript
class Stack {
  // ...
  peek() {
    return this.top;
  }
}
```

- 스택의 `top`에 해당하는 원소를 리턴한다.

---

## 4. 배열로 스택 구현하기

배열로 스택을 구현한다. 배열로 스택을 구현하면 배열 메서드를 사용하여 보다 쉽게 스택을 구현할 수 있다.
물론 배열 메서드를 사용하지 않고도 구현을 할 수 있다.

---

### 4-1. 스택의 기본 구조

```javascript
class Stack {
  constructor() {
    this.arr = [];
    this.size = this.arr.length;
  }
}
```

---

### 4-2. 스택에 원소 추가하기

```javascript
class Stack {
  // ...
  push(data) {
    this.arr.push(data);
    this.size++;
  }
}
```

---

### 4-3. 스택에 원소 제거하기

```javascript
class Stack {
  // ...
  pop() {
    if (this.size === 0) return null;
    this.size--;
    return this.arr.pop();
  }
}
```

---

### 4-4. 스택에 쌓인 가장 마지막 원소 가져오기

```javascript
class Stack {
  // ...
  peek() {
    if (this.size === 0) return null;
    return this.arr[this.size - 1];
  }
}
```

---

## 5. 스택의 시간 복잡도

- push(추가): 스택에 원소를 추가하는 것은 스택의 맨 위에 원소를 하나 올리기만 하면 된다. 즉, 스택이
  무수히 커도, 원소를 추가하기 위해 한 번의 노력만 하면 된다.
- pop(삭제): 스택이 끝없이 크더라도 맨 위의 원소를 삭제하면 되므로 시간 복잡도는 O(1)를 가진다.
- search(검색): 스택에서 원소의 접근은 맨 아래의 원소에서만 가능하므로 n번째 있는 원소를 검색하기 위해서는
  시간 복잡도 O(n)를 가진다.

| Big-O (시간복잡도) | 삽입 | 삭제 | 검색 |
| ------------------ | ---- | ---- | ---- |
| 스택 (Stack)       | O(1) | O(1) | O(n) |

---

## 6. Conclusion

> 스택은 큐보다 구현이 쉬웠다. 자바스크립트의 배열로 바로 만들 수 있어서 그런 듯 하다. 스택과 큐 자료구조 문제에서
> 자주 보이기 때문에 잘 숙지하고 넘어가자. 지금까지 정리한 자료구조(스택, 큐)는 큰 어려움이 없었다. 하지만 아직
> 문제 풀이의 경험이 부족하기 때문에 어떤 문제에서 사용해야 하는지는 감이 떨어지지만 꾸준히 문제를 풀고 공부를 한다면
> 쉽고 빠른 시간안으로 해결할 수 있어 보인다.

---

## 참고

[[자료구조] 스택 with JavaScript](https://overcome-the-limits.tistory.com/14)  
[[자료구조 1] 스택(Stack) 활용하기](https://laboputer.github.io/ps/2017/09/09/stack/)

---

📅 2022-09-06
