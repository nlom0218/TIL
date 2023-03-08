# 연결 리스트(Linked List)

## 0. 요약

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class LinkedList {
  constructor() {
    this.head = null;
    this.rear = null;
    this.size = 0;
  }

  // 연결 리스트 맨 앞에 원소 추가하기
  insertFirst(data) {
    const node = new Node(data);
    if (this.size === 0) this.rear = node;
    node.next = this.head;
    this.head = node;
    this.size++;
  }

  // 연결 리스트 맨 뒤에 원소 추가하기
  push(data) {
    const node = new Node(data);
    if (this.size === 0) {
      this.head = node;
    } else {
      this.rear.next = node;
    }
    this.rear = node;
    this.size++;
  }

  // 연결 리스트 중간에 원소 추가하기
  insertAt(data, index) {
    if (index < 0 || index > this.size) {
      return;
    }
    if (index === 0) {
      return this.insertFirst(data);
    }
    if (index === this.size) {
      return this.push(data);
    }
    const node = new Node(data);
    let cur = this.head;
    let next = this.head.next;
    let count = 1;

    while (count < index) {
      count++;
      cur = next;
      next = next.next;
    }

    cur.next = node;
    node.next = next;

    this.size++;
  }

  // 연결 리스트의 원소 가져오기
  getAt(index) {
    if (index < 0 || index >= this.size) {
      return;
    }
    if (index === this.size - 1) {
      return this.rear;
    }
    let cur = this.head;
    let count = 0;
    while (count < index) {
      cur = cur.next;
      count++;
    }
    return cur;
  }

  // 연결 리스트의 원소 삭제하기
  removeAt(index) {
    if (index < 0 || index >= this.size) {
      return;
    }
    let cur = this.head;
    let prev;
    let count = 0;

    if (index === 0) {
      this.head = cur.next;
    } else {
      while (count < index) {
        count++;
        prev = cur;
        cur = cur.next;
      }
      prev.next = cur.next;
    }
    this.size--;
    return cur;
  }

  // 연결 리스트 초기화하기
  clearList() {
    this.head = null;
    this.rear = null;
    this.size = 0;
  }

  // 연결 리스트의 모든 원소 출력하기
  printListData() {
    let cur = this.head;

    while (cur) {
      console.log(cur);
      cur = cur.next;
    }
  }
}
```

---

## 1. 개요

배열과 함께 다른 자료 구조들을 구현하는 데 자주 사용되는 연결 리스트에 대해 살펴본다.

---

## 2. 연결 리스트란?

각 노드가 데이터와 포인터를 가지고 한 줄로 연결되어 있는 방식으로 데이터를 저장하는 자료 구조이다.
연결 리스트는 자료의 추가와 삭제가 O(1)의 시간을 가능하다는 장점을 갖지만 특정 위치의
데이터를 검색해 내는데에는 O(n)의 시간이 걸리는 단점을 갖고 있다.

연결 리스트의 종류는 아래와 같다.

1. 단일(단방향) 연결 리스트
   - 각 노드에 자료 공간과 한 개의 포인터 공간이 있다.
   - 각 노드의 포인터는 다음 노드를 가리킨다.  
     ![단일 연결 리스트의 구조](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/Single_linked_list.png/800px-Single_linked_list.png)
2. 이중(양방향) 연결 리스트
   - 포인터 공간이 두 개가 있고 각각의 포인터는 앞의 노드와 뒤의 노드를 가리킨다.  
     ![이중 연결 리스트의 구조](https://upload.wikimedia.org/wikipedia/commons/thumb/c/ca/Doubly_linked_list.png/800px-Doubly_linked_list.png)
3. 원형 연결 리스트
   - 일반적인 연결 리스트에 마지막 노드와 처음 노드를 연결시켜 원형으로 만든 구조이다.
     ![원형 연결 리스트의 구조](https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/Circurlar_linked_list.png/800px-Circurlar_linked_list.png)

---

## 3. 자바스크립트로 연결 리스트 구현하기

연결 리스트 중 단일 연결 리스트를 자바스크립트를 사용하여 구현해본다.

### 3-1. 노드와 연결 리스트의 기본 구조

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}
```

노드의 기본구조는 자신이 가지는 값인 `data`와 자신으 뒤 노드의 값을 가리키는 `next`로 이루어진다.
만약 이중 연결 리스트라면 자신의 앞 노드를 가리키는 `this.prev`가 추가된다.

```javascript
class LinkedList {
  constructor() {
    this.head = null;
    this.rear = null;
    this.size = 0;
  }
}
```

연결 리스트의 기본 구조로 맨 앞 노드인 `head`, 맨 뒤 노드인 `rear` 그리고 연결 리스트의
원소의 개수인 `size`로 구성된다.

### 3-2. 연결 리스트에 원소 추가하기

1. 연결 리스트 맨 앞에 원소를 추가하는 함수

   ```javascript
   class LinkedList {
     // ...
     unshift(data) {
       const node = new Node(data);
       if (!this.head) this.rear = node;

       node.next = this.head;
       this.head = node;
       this.size += 1;

       return this;
     }
   }
   ```

   - 새로운 `Node`를 생성한다.
   - 만약 연결 리스트의 `size`가 0이라면 맨 뒤 노드인 `rear` 값을 새롭게 생성한 `Node`로 지정한다.
   - 생성한 `Node`의 뒤 노드 값을 현재 연결 리스트의 `head`로 지정한다.
   - 연결 리스트의 `head`에 새로운 `Node`가 들어간다.
   - 연결 리스트의 `size`를 하나 증가시킨다.

2. 연결 리스트 맨 뒤에 원소를 추가하는 함수

   ```javascript
   class LinkedList {
     // ...
     push(data) {
       const node = new Node(data);
       if (this.size === 0) this.head = node;
       else this.rear.next = node;

       this.rear = node;
       this.size++;

       return this;
     }
   }
   ```

   - 새로운 `Node`를 생성한다.
   - 연결 리스트의 `size`가 0이라면 `head`의 값을 생성한 `Node`로 지정한다.
   - 연결 리스트의 `size`가 0이 아니라면 현재 연결 리스트의 `rear`의 뒤 노드의 값을 새롭게 생성한 `Node`로 지정한다.
   - 연결 리스트의 `rear`에 새로운 `Node`가 들어간다.
   - 연결 리스트의 `size`를 하나 증가시킨다.
   - 연결 리스트를 반환한다.

3. 연결 리스트 중간에 원소를 추가하는 함수

   ```javascript
   class LinkedList {
     // ...
     insert(index, data) {
       if (index < 0 || index > this.size) return false;

       if (index === 0) return !!this.unshift(data);

       if (index === this.size) return !!this.push(data);

       const node = new Node(data);

       let prev = this.get(index - 1);
       node.next = prev.next;
       prev.next = node;

       this.size += 1;

       return true;
     }
   }
   ```

   - `index`가 0보다 작거나 연결 리스트의 크기보다 크다면 아무것도 리턴하지 않는다.
   - `index`가 0일 경우 연결 리스트 맨 앞에 원소를 추가하는 경우이다.
   - `index`가 `size`와 같을 경우 연결 리스트 맨 뒤에 원소를 추가하는 경우이다.
   - `cur`를 연결 리스트의 맨 앞에 위치한 원소로 지정한다.
   - `next`를 `cur` 앞이 원소로 지정한다.
   - `count`가 `index`보다 커질 때 까지 반복문을 돌린다.
   - 반복문 안에서 `cur`은 `next`가 되고 `next`는 `next`의 앞 원소가 된다.
   - 새롭게 생성한 노드가 `cur`과 `next` 사이에 들어간다.

### 3-3. 연결 리스트의 원소 가져오기

```javascript
class LinkedList {
  // ...
  get(index) {
    if (index < 0 || index >= this.size) return null;

    if (index === this.size - 1) return this.rear;

    let cur = this.head;
    let count = 0;
    while (count < index) {
      cur = cur.next;
      count += 1;
    }
    return cur;
  }
}
```

- `index`가 0보다 작거나 연결 리스트의 크기보다 크다면 아무것도 리턴하지 않는다.
- `index`가 연결 리스트의 마지막(size-1) 번호라면 연결 리스트의 `rear`를 리턴한다.
- `cur`를 연결 리스트의 맨 앞에 위치한 원소로 지정한다.
- `count`가 `index`보다 작다면 `while`문을 실행한다.
- `while`문에서는 `cur`를 앞 원소로 바꾸고 `count`를 1증가시킨다.
- `cur`를 반환한다.

### 3-4. 연결 리스트의 원소 업데이트하기

```javascript
class LinkedList {
  // ...
  set(index, data) {
    if (!this.get(index)) return false;

    let foundNode = this.get(index);
    foundNode.data = data;

    return true;
  }
}
```

### 3-5. 연결 리스트의 원소 삭제하기

1.  연결 리스트의 마지막 원소 삭제하기

    ```javascript
    class LinkedList {
      // ...
      pop() {
        if (!this.head) return undefined;

        let current = this.head;
        let newRear = current;

        while (current.next) {
          newRear = current;
          current = current.next;
        }

        newRear.next = null;
        this.rear = newRear;
        this.size -= 1;

        if (this.size === 0) {
          this.head = null;
          this.rear = null;
        }

        return current;
      }
    }
    ```

    - `head`가 없다면 `undefined`를 반환한다.
    - 현재 바라보고 있는 노드를 `current` 변수에 할당한다.
    - 다음 `rear`될 노드를 `newRear` 변수에 할당한다.
    - `current` 노드의 다음 노드가 있을 때 반복문을 실행한다.
      - 반복문 내에서 `newRear`은 `current`가 된다.
      - 반복문 내에서 `current`는 다른 노드가 된다.
    - 반복문에서 나오게 될 때, `current`는 마지막 노드를 바라보게 된다.
    - 반복문에서 나오게 될 때, `newRear`는 마지막 노드의 이전 노드를 바라보게 된다.
    - `newRear`의 다음 노드를 `null`로 할당한 뒤 `this.resr`가 되도록 한다.
    - `size`를 1줄인다.
    - 만약 `size`가 0이 되었다면 `this.head`, `this.rear`은 모두 `null`이다.
    - 연결 리스트에서 끊어진 `current` 노드를 반환한다.

2.  연결 리스트의 첫번째 원소 삭제하기

    ```javascript
    class LinkedList {
      // ...
      shift() {
        if (!this.head) return undefined;

        const currentHead = this.head;
        this.head = currentHead.next;
        this.size -= 1;

        if (this.size === 0) this.rear = null;

        return currentHead;
      }
    }
    ```

    - `head`가 없으면 `undefined`를 반환한다.
    - `head`를 변수(currentHead)에 저장을 한다.
    - `head`를 변수(currentHead)의 다음 노드로 바꾼다.
    - `size`를 1줄인다.
    - 만약 `size`가 0이 되었다면 더 이상 연결리스트에는 노드가 없는 것이기 때문에 `rear`을 `null`로 바꾼다.
    - 제거하려 했던 첫 번째 노드, 즉 currentHead 를 반환한다.

3.  연결 리스트의 특정 위치의 원소 삭제하기

    ```javascript
    class LinkedList {
      // ...
      remove(index) {
        if (index < 0 || index >= this.size) return undefined;

        if (index === 0) return this.shift();
        if (index === this.size - 1) return this.pop();

        let prev = this.get(index - 1);
        let removed = prev.next;
        prev.next = prev.next.next;

        this.size -= 1;

        return removed;
      }
    }
    ```

    - `index`가 0보다 작거나 연결 리스트의 크기보다 크다면 아무것도 리턴하지 않는다.
    - `cur`를 연결 리스트의 맨 앞에 위치한 원소로 지정한다.
    - `prev`를 선언한다.
    - `count`의 기본값을 1로 한다.
    - `index`가 0인 경우 연결 리스트의 `head`를 `cur`의 앞에 위치한 원소로 지정한다.
    - 그 외의 경우 `count`가 `index`보다 작다면 반복문을 실행한다.
    - 반복문에서는 `count`값을 1증가시키고 `prev`를 `cur`로, `cur`를 `cur`의 앞에 위차한 원소를 지정한다.
    - 반복문이 종료되는 시점에는 `cur`값이 삭제되는 원소이다.
    - 그러므로 `prev`의 `next`를 `cur`의 앞에 위차한 원소로 바꾼다.
    - 연결 리스트의 `size`를 1감소시키고 삭제된 원소를 리턴한다.

### 3-6. 연결 리스트 초기화하기

```javascript
class LinkedList {
  // ...
  clearList() {
    this.head = null;
    this.rear = null;
    this.size = 0;
  }
}
```

- `head`와 `rear`를 `null`로 바꾸고 `size`를 0으로 바꾼다.
- 메모리자체에는 `Node`가 남아있다.

### 3-7. 연결 리스트의 모든 원소 출력하기

```javascript
class LinkedList {
  // ...
  printListData() {
    let cur = this.head;

    while (cur) {
      console.log(cur);
      cur = cur.next;
    }
  }
}
```

- `cur`이 존재하지 않을때까지 `cur`를 콘솔에 출력한다.
- 반복문에서 `cur`은 계속해서 앞의 원소로 교체된다.

### 3-8. 연결 리스트 역순으로 바꾸기

```javascript
class LinkedList {
  // ...
  reverse() {
    let head = this.head;
    let rear = this.rear;

    this.head = rear;
    this.rear = head;

    let prev = this.rear;
    let cur = this.rear.next;
    let count = 0;

    while (count < this.size - 1) {
      const temp = cur.next;
      cur.next = prev;
      cur = temp;
      prev = cur;

      count += 1;
    }

    this.rear.next = null;

    return this;
  }
}
```

---

## 4. Conclusion

> 연결 리스트(Linked List)를 자바스크립트로 구현하는 연습을 하였다. 구현을 하면서 배열의 메서드도 공부했던
> 과정처럼 만들어지는 것 같았다. `while 문`을 언제까지 실행하는 것을 정하는 것이 조금 까다로웠지만 구현을 하고 나니
> 많은 공부가 되었다. 블로그를 참고하였는데 참고하면서 최대한 의미를 파악하고 나의 방식으로 다시 코드를 구현하도록 노력했다.
> 연결 리스트가 앞으로 얼마나 많이 사용할지는 모르겠지만 오늘 배운 내용이 자료 구조를 공부할 때 많은 도움이 되었으면 한다.
> 연결 리스트는 다른 자료 구조를 구현하는데 많이 사용하기 때문에 기본이라고 생각한다. 머리가 살짝 지끈거리다.🤣

---

## 출처

[위키백과 - 연결 리스트](https://ko.wikipedia.org/wiki/%EC%97%B0%EA%B2%B0_%EB%A6%AC%EC%8A%A4%ED%8A%B8)  
[Javascript를 이용한 Linked List 구현](https://velog.io/@kimkevin90/Javascript%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-Linked-List-%EA%B5%AC%ED%98%84)

---

📅 2022-09-01
