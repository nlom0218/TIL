# 프린터

## 1. 개요

- 프로그래머스
- Lv.2
- 스택/큐
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42587)

---

## 2. 문제 설명

일반적인 프린터는 인쇄 요청이 들어온 순서대로 인쇄합니다. 그렇기 때문에 중요한 문서가 나중에 인쇄될 수 있습니다. 이런 문제를 보완하기 위해 중요도가 높은 문서를 먼저 인쇄하는 프린터를 개발했습니다. 이 새롭게 개발한 프린터는 아래와 같은 방식으로 인쇄 작업을 수행합니다.

> 1. 인쇄 대기목록의 가장 앞에 있는 문서(J)를 대기목록에서 꺼냅니다.
> 2. 나머지 인쇄 대기목록에서 J보다 중요도가 높은 문서가 한 개라도 존재하면 J를 대기목록의 가장 마지막에 넣습니다.
> 3. 그렇지 않으면 J를 인쇄합니다.

예를 들어, 4개의 문서(A, B, C, D)가 순서대로 인쇄 대기목록에 있고 중요도가 2 1 3 2 라면 C D A B 순으로 인쇄하게 됩니다.

내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 알고 싶습니다. 위의 예에서 C는 1번째로, A는 3번째로 인쇄됩니다.

현재 대기목록에 있는 문서의 중요도가 순서대로 담긴 배열 priorities와 내가 인쇄를 요청한 문서가 현재 대기목록의 어떤 위치에 있는지를 알려주는 location이 매개변수로 주어질 때, 내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 return 하도록 solution 함수를 작성해주세요.

---

### 2-1. 문제 설명 - 제한사항

- 현재 대기목록에는 1개 이상 100개 이하의 문서가 있습니다.
- 인쇄 작업의 중요도는 1~9로 표현하며 숫자가 클수록 중요하다는 뜻입니다.
- location은 0 이상 (현재 대기목록에 있는 작업 수 - 1) 이하의 값을 가지며 대기목록의 가장 앞에 있으면 0, 두 번째에 있으면 1로 표현합니다.

---

### 2-2. 문제 설명 - 입출력 예

| priorities         | location | return |
| ------------------ | -------- | ------ |
| [2, 1, 3, 2]       | 2        | 1      |
| [1, 1, 9, 1, 1, 1] | 0        | 5      |

---

### 2-3. 문제 설명 - 입출력 예 설명

예제 #1

문제에 나온 예와 같습니다.

예제 #2

6개의 문서(A, B, C, D, E, F)가 인쇄 대기목록에 있고 중요도가 1 1 9 1 1 1 이므로 C D E F A B 순으로 인쇄합니다.

---

## 3. 문제 풀이

```javascript
function solution(priorities, location) {
  // 1) priorities 배열의 요소를 요소의 index와 기존 요소를 값으로 가지는 객체로 바꾸기
  priorities = priorities.map((item, index) => {
    return { index, item };
  });

  // 2) 중요도 순서대로 프린트 될 문서를 차례대로 저장하게 될 배열 선언하기
  let print = [];
  let index = 0;

  // 3) print 배열에 내가 인쇄를 요청한 문서가 있을 때 까지 반복문 실행하기
  while (print.findIndex((item) => item.index === location) === -1) {
    // 4) 현재 처리할 문서 가져오기
    const list = priorities[index];

    // 5) 남아있는 문서 중 가장 큰 중요도 구하기
    const max = Math.max(...priorities.slice(index).map((item) => item.item));

    // 6) 현재 문서의 중요도가 가장 큰 중요도보다 작으면 priorities 배열의 마지막 요소로 다시 추가하기
    if (list.item < max) {
      priorities.push(list);
    } else {
      // 7) 현재 문서의 중요도가 가장 큰 중요도라면 print 배열에 추가하기
      print.push(list);
    }

    // 8) 다음 문서를 처리하기 위해 index값을 1증가시키기
    index++;
  }

  // 9) print 배열의 길이를 반환하기
  return print.length;
}
```

---

### 1) priorities 배열의 요소를 요소의 index와 기존 요소를 값으로 가지는 객체로 바꾸기

```javascript
priorities = priorities.map((item, index) => {
  return { index, item };
});
```

인쇄 순서의 정보를 담고 있는 `index`와 문서의 중요도를 값으로 가지고 있는 객체를 `priorities` 배열의 요소로 만든다.

`index`를 값으로 저장하는 이유는 중요도가 다른 문서보다 낮으면 배열의 맨 뒤로 이동하게 되는데 이때
인덱스가 달라지기 때문이다. 즉, 처음의 인쇄 순서를 기억하고 싶어서 저장을 한다.

---

### 2) 중요도 순서대로 프린트 될 문서를 차례대로 저장하게 될 배열 선언하기

```javascript
let print = [];
let index = 0;
```

- `print`: 중요도가 큰 문서부터 정렬이 되어 있는 배열, 반복문을 통해 중요도가 큰 문서부터 저장된다.
- `index`: 반복문에서 현재 처리해야 할 문서를 가져오기 위한 값

---

### 3) print 배열에 내가 인쇄를 요청한 문서가 있을 때 까지 반복문 실행하기

```javascript
while (print.findIndex((item) => item.index === location) === -1) {}
```

`location`은 내가 인쇄를 요청한 문서의 위치이다. 즉 `1)`에서 새롭게 구성된 `priorities` 배열의
요소 중 `index`의 값이다.

`print` 배열의 요소 중...

- `index`가 `location`과 같은 요소가 있다면 내가 인쇄를 요청한 문서가 존재하는 것이므로
  더 이상의 반복문은 무의미하다.
- `index`가 `location`과 같은 요소가 없다면 `Array.findIndex()` 메서드는
  -1를 반환하게 되므로 계속 반복문을 실행한다.

---

### 4) 현재 처리할 문서 가져오기

```javascript
const list = priorities[index];
```

`index`의 초기값은 0이고 매 반복문마다 1씩 증가한다. 즉, `priorities` 배열의 요소를 처음부터
차례대로 가져온다.

---

### 5) 남아있는 문서 중 가장 큰 중요도 구하기

```javascript
const max = Math.max(...priorities.slice(index).map((item) => item.item));
```

- Array.slice(index): `index`에 해당하는 요소부터 배열의 마지막 요소를 포함한 새로운 배열을 만든다.
- `Array.slice(index)` 메서드를 사용하는 이유
  - 아래의 조건문을 통해 `priorities[index]`는 제거가 되지 않는다.
  - 조건에 따라 `print`배열에 저장이 되거나 `priorities` 배열의 마지막 요소로 추가된다.
  - `priorities` 배열의 첫 번재 요소부터 마지막 요소까지의 최댓값을 구한다면 항상 같은 값을 얻을 것이다.
  - `priorities[index]` 부터 마지막 요소까지 최댓값을 구하면 이미 처리된 문서의 중요도는 제외된다.
  - 가장 높은 중요도를 가진 문서는 `print` 배열로 이동했기 때문에 가장 큰 중요도를 구하는 과정에 포함시키면 안된다.
- Math.max(): 인자로 주어진 값들 중 가장 큰 값을 반환한다.

> 매 반복문마다 해당 과정이 실행되기 때문에 실행 속도가 늦어지는 것이 아닐까 싶다.

---

### 6) 현재 문서의 중요도가 가장 큰 중요도보다 작으면 priorities 배열의 마지막 요소로 다시 추가하기

```javascript
if (list.item < max) {
  priorities.push(list);
}
```

만약 중요도가 `max`보다 작다면 나중에 다시 인쇄를 해야하기 때문에 `priorities` 베열의 마지막 요소로 추가한다.

---

### // 7) 현재 문서의 중요도가 가장 큰 중요도라면 print 배열에 추가하기

```javascript
else {
    print.push(list);
}
```

만약 중요도가 `max`와 같다면 가장 먼저 처리해야 할 인쇄 목록이기 때문에 `print` 배열에 넣는다.

---

### 8) 다음 문서를 처리하기 위해 index값을 1증가시키기

```javascript
index++;
```

현재 문서를 처리했기 때문에 다음 문서를 처리하기 위해 `index`값을 1증가시킨다.

---

### 9) print 배열의 길이를 반환하기

```javascript
return print.length;
```

문제에서 원하는 정답은 내가 요청한 문서의 인쇄 순서이다. `print` 배열에 내가 요청한 문서가 있으면 더 이상
요소의 추가가 이루어지지 않는다. 즉, `print` 배열에는 내가 요청한 문서가 가장 마지막의 요소로 저장되어 있다.

그리고 `print` 배열의 요소는 순서대로 인쇄를 할 문서들이다. 즉, `print` 배열의 길이가 내가 요청한 문서의
인쇄 순서가 된다.

---

### 결과

![programmers_printer_result1](/image/CodingTest/programmers_printer/programmers_printer_result1.png)

---

## 4. 큐를 사용한 문제 풀이

```javascript
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class Queue {
  constructor() {
    this.head = null;
    this.tail = null;
    this.size = 0;
  }

  enqueue(newValue) {
    const newNode = new Node(newValue);
    if (this.head === null) {
      this.head = this.tail = newNode;
    } else {
      this.tail.next = newNode;
      this.tail = newNode;
    }
    this.size++;
  }

  dequeue() {
    const value = this.head.value;
    this.head = this.head.next;
    this.size--;
    return value;
  }

  peek() {
    return this.head.value;
  }
}

function solution(priorities, location) {
  // 1) 큐를 생성하고 priorities 배열의 요소를 차례대로 큐의 원소로 저장하기
  const queue = new Queue();
  for (let i = 0; i < priorities.length; i++) {
    queue.enqueue([priorities[i], i]);
  }

  // 2) priorities 배열을 올림차순을 정렬하기
  priorities.sort((a, b) => b - a);

  // 3) 인쇄를 한 문서의 수를 세는 count 변수 선언하기
  let count = 0;

  // 4) 큐에 원소가 없을 때까지 반복문 실행하기
  while (queue.size > 0) {
    // 5) 큐의 맨 앞 원소 가져오기
    const cur = queue.peek();

    // 6) 큐의 맨 앞 원소의 중요도가 가장 큰 중요도가 아니라면 해당 원소를 큐의 마지막 원소로 저장하기
    if (cur[0] < priorities[count]) {
      queue.enqueue(queue.dequeue());
    } else {
      // 7) 큐의 맨 앞 원소가 중요도가 가장 크다면 큐에서 해당 원소를 제거하기
      const value = queue.dequeue();
      // 8) 인쇄가 되었으므로 count 값을 1증가시키기
      count += 1;

      // 9) 만약 내가 처리해야 할 요소가 인쇄가 되었으면 count를 리턴하기
      if (location === value[1]) {
        return count;
      }
    }
  }

  // 10) 여기까지 왔으면 내가 요청한 문서는 가장 마지막에 인쇄가 된다.
  return count;
}
```

### 1) 큐를 생성하고 priorities 배열의 요소를 차례대로 큐의 원소로 저장하기

```javascript
const queue = new Queue();
for (let i = 0; i < priorities.length; i++) {
  queue.enqueue([priorities[i], i]);
}
```

새로운 큐를 생성하고 `priorities` 배열의 요소를 차례대로 큐의 원소로 저장한다.
이때 저장되는 원소의 데이터는 `[중요도, 처음 인덱스]`의 형태를 가진다.

---

### 2) priorities 배열을 올림차순을 정렬하기

```javascript
priorities.sort((a, b) => b - a);
```

`priorities` 배열을 올림차순으로 정렬한다. 이렇게 되면 중요도가 큰 순서대로 정렬이 된다.

---

### 3) 인쇄를 한 문서의 수를 세는 count 변수 선언하기

```javascript
let count = 0;
```

`count` 변수는 두 가지의 역할을 한다.

- 인쇄되는 문서의 수를 나타낸다.(또한 반복문에서 해당 문서가 인쇄되는 순서를 뜻한다.)
- `priorities` 배열의 값을 가져온다.

---

### 4) 큐에 원소가 없을 때까지 반복문 실행하기

```javascript
while (queue.size > 0) {}
```

반복문을 통해 큐에 저장된 데이터를 제거한다. 그러다가 큐에 원소가 없어지면 반복문을 종료한다.
하지만 특정 조건이 만족이 되면 반복문이 종료가 된다. 조건이 만족되지 않으면 끝까지 반복문을 실행하는 것이고...

---

### 5) 큐의 맨 앞 원소 가져오기

```javascript
const cur = queue.peek();
```

인쇄 요청이 먼저 들어온 문서부터 처리를 해야하기 때문에 `peek()` 메서드를 통해 큐의 맨 앞의 원소를 가져온다.

---

### 6) 큐의 맨 앞 원소의 중요도가 가장 큰 중요도가 아니라면 해당 원소를 큐의 마지막 원소로 저장하기

```javascript
if (cur[0] < priorities[count]) {
  queue.enqueue(queue.dequeue());
}
```

만약 현재 처리하는 문서의 중요도가 `priorities[count]`의 값보다 작으면 큐의 맨 앞의 원소를 제거하고
다시 큐의 마지막 원소로 저장한다. 이를 인쇄 순서가 미루어졌다는 뜻이다.

---

### 7) 큐의 맨 앞 원소가 중요도가 가장 크다면 큐에서 해당 원소를 제거하기

```javascript
const value = queue.dequeue();
```

만약 현재 처리하는 문서의 중요도가 `priorities[count]`의 값과 같다면 해당 문서는 바로 인쇄를 해야하는
문서이다. 그렇기 때문에 큐의 맨 앞 원소를 제거하고 제거한 값을 `value` 변수에 할당한다.

---

### 8) 인쇄가 되었으므로 count 값을 1증가시키기

```javascript
count += 1;
```

`7)` 과정에 인쇄가 되었으므로 해당 문서가 인쇄되는 순서를 의미하는 `count`의 값을 하나 올린다.

또한 `count` 값을 하나 증가시켰으므로 `priorities[count]`의 값도 다음 값으로 바뀌게 된다. 이는 그 다음으로
중요한 문서를 찾는다고 생각을 할 수 있다. 왜냐! 이전 중요도에 해당하는 문서는 인쇄가 되었기 때문이다.

---

### 9) 만약 내가 처리해야 할 요소가 인쇄가 되었으면 count를 리턴하기

```javascript
if (location === value[1]) {
  return count;
}
```

만약 현재 인쇄하는 문서가 내가 요청한 문서와 같다면 해당 문서가 인쇄하는 순서를 값으로 가지고 있는 `count`를
반환하며 반복문을 종료한다.

---

### 10) 여기까지 왔으면 내가 요청한 문서는 가장 마지막에 인쇄가 된다.

```javascript
return count;
```

내가 요처한 반복문이 중요도가 가장 낮고 그 중에서도 가장 마지막에 인쇄가 되는 문서라면 반복문이 모두 실행되고
나서 `count`를 반환하면 된다.

---

### 결과

![programmers_printer_result2](/image/CodingTest/programmers_printer/programmers_printer_result2.png)
