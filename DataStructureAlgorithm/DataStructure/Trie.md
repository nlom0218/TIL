# 트라이(Trie)

## 0. 요약

트라이 구현하기

```javascript
class Node {
  constructor(value = "", end = false) {
    this.value = value;
    this.end = end;
    this.child = new Map();
    this.includesWords = [];
  }
}

class Trie {
  constructor() {
    this.root = new Node();
  }

  // 문자열 삽입하기
  insert(string) {
    let cur_node = this.root;

    for (const char of string) {
      if (!cur_node.child.has(char)) {
        cur_node.child.set(char, new Node(cur_node.value + char));
      }
      cur_node = cur_node.child.get(char);
      cur_node.includesWords.push(string);
    }
    cur_node.end = true;
  }

  // 문자열 검색하기
  search(string) {
    let cur_node = this.root;

    for (const char of string) {
      if (cur_node.child.has(char)) {
        cur_node = cur_node.child.get(char);
      } else return null;
    }
    return cur_node.includesWords[0] === string ? cur_node : null;
  }

  // 특정 단어만 검색되어지는 최소 입력 횟수 구하기
  min_len(string) {
    let cur_node = this.root;
    let len = 0;
    for (const char of string) {
      cur_node = cur_node.child.get(char);
      len++;
      if (cur_node.includesWords.length === 1) return len;
    }
    return len;
  }
}
```

---

## 1. 개요

트리 자료구조의 하나인 트라이 자료구조에 대해 살펴본다.

---

## 2. 트라이란?

문자열을 저장하고 효율적으로 탐색하기 위한 트리 형태의 자료구조이다.

![트라이](https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Trie_example.svg/375px-Trie_example.svg.png)

위 트리의 루트에서부터 자식들을 따라가면서 생성된 문자열들이 트라이 자료구조에 저장되어 있다고 생각할 수 있다. 저장된 단어는
끝을 표시하는 변수를 추가해서 저장된 단어의 끝을 구분할 수 있다.

---

### 2-1. 트라이의 특징

- 검색어 자동완성, 사전 찾기 등에 응용
- 문자열을 탐색할 때 단순하게 비교하는 것보다 효율적으로 찾을 수 있음
- 시간복잡도는 L이 문자열의 길이일 때 탐색, 삽입은 O(L)만큼 소요
- 단점으론 각 정점이 자식에 대한 링크를 전부 가지고 있기에 저장 공간을 더 많이 사용함

---

## 3. 트라이 구현하기

### 3-1. 트라이의 기본 구조

```javascript
class Node {
  constructor(value = "", end = false) {
    this.value = value;
    this.end = end;
    this.child = new Map();
    this.includesWords = [];
  }
}

class Trie {
  constructor() {
    this.root = new Node();
  }
}
```

먼저 `Node` 부터 살펴보자.

- value: 현재 경로까지의 누적값
- end: 해당 노드에서 끝나는 문자열이 있는 여부
- child: 자식
- includesWords: 해당 노드의 `value`를 포함한 문자열의 배열, `includesWords`은 자동완성 및 특정 단어만 검색되어지는
  최소 입력 횟수를 구할 때 용이하게 사용할 수 있다.

그리고 트라이의 구조는 `root`로 이루어져 있고 `root`는 빈 문자열로 시작한다.

---

### 3-2. 문자열 삽입하기

```javascript
class Trie {
  // ...
  insert(string) {
    let cur_node = this.root;

    for (const char of string) {
      if (!cur_node.child.has(char)) {
        cur_node.child.set(char, new Node(cur_node.value + char));
      }
      cur_node = cur_node.child.get(char);
      cur_node.includesWords.push(string);
    }
    cur_node.end = true;
  }
}
```

문자열을 삽입하는 과정이다. 루트노드를 시작으로 탐색을 진행한다.

- `cur_node`의 `child`에 `char`키를 가진 값이 없다면 새롭게 만들어 준다.

  ```javascript
  if (!cur_node.child.has(char)) {
    cur_node.child.set(char, new Node(cur_node.value + char));
  }
  ```

- `cur_node`의 자식 노드로 이동한 뒤 `cur_node`의 `includesWords`에 `string`를 넣는다.

  ```javascript
  cur_node = cur_node.child.get(char);
  cur_node.includesWords.push(string);
  ```

- 반복문이 종료되면 문자열 삽입이 끝났으므로 그 때의 `cur_node`의 `end`를 `ture`로 바꾼다.

  ```javascript
  cur_node.end = true;
  ```

---

### 3-3. 문자열 검색하기

```javascript
class Trie {
  // ...
  search(string) {
    let cur_node = this.root;

    for (const char of string) {
      if (cur_node.child.has(char)) {
        cur_node = cur_node.child.get(char);
      } else return null;
    }
    return cur_node.includesWords[0] === string ? cur_node : null;
  }
}
```

매개변수로 들어온 문자열을 루트 노드에서 부터 아래로 내려가면서 탐색을 시작한다.
만약 `cur_node`의 `child`에 다음 문자열을 키로 가지는 값이 없다면 저장되지 않는 문자열이기 때문에
`null`를 반환한다.

하지만 만약 끝까지 탐색을 완료했으면 탐색을 완료한 시점의 `cur_node`의 `includesWords`의 0번째 요소가
내가 찾고자 하는 단어와 같으면 `cur_node`를 반환하고 그렇지 않으면 삽입한 단어가 아니기 때문에 `null`를
반환한다.

물론 이 과정은 상황에 따라 다르게 구현될 수 있다. 만약 `hello`를 삽입을 했고 `hell`를 찾는다 하면
특정 노드까지 내려갈 수 있다. 하지만 내가 `hell`를 삽입을 한 것이 아니라 `hello`로 가는 경로 중에 있기
때문에 엄밀히 생각하면 내가 삽입(등록)한 단어가 아니라는 것이다.

만약 경로 상에 있는 모든 단어까지 포함한다 하면 마지막에 그냥 `cur_node`를 반환하기만 하면 된다.

---

### 3-4. 특정 단어만 검색되어지는 최소 입력 횟수 구하기

```javascript
class Trie {
  // ...
  min_len(string) {
    let cur_node = this.root;
    let len = 0;
    for (const char of string) {
      cur_node = cur_node.child.get(char);
      len++;
      if (cur_node.includesWords.length === 1) return len;
    }
    return len;
  }
}
```

위의 코드는 [프로그래머스 문제 - [3차]자동완성](https://school.programmers.co.kr/learn/courses/30/lessons/17685)의
풀이를 위해 작성한 코드이다.

특정 단어만 자동완성이 될 수 있는 최소 입력 횟수를 구하는 코드이다. 해당 코드에서 `includesWords`가 아주
유용하게 사용되었다.

`includesWords`의 길이가 1이라면 그 때가 바로 최소 입력 회수가 되는 것이고 만약 반복문이 끝날 때 까지
`includesWords`의 길이가 1이 아니라면 `string`의 길이를 반환하면 된다. 이러한 이유는 `go`와 `gone`이
삽입되어 있을 때 `go`는 끝까지 입력을 해야한다. 계속해서 `gone`도 함께 검색 될 테니까 말이다.

---

## 4. Conclusion

> 너무 멋진 자료구조이다. 자바스크립트에 내장된 함수가 없는게 너무 아쉬울 따름이다. 그래서 직접 구현하면서
> 어떻게 하면 트라이 자료구조를 만들 수 있을까 생각을 많이 하였고 특히 `min_len` 함수를 만들 때
> 고민을 많이 했었다. 자주자주 보면서 생각의 흐름을 잘 정리하고 언제든지 필요할 때 사용할 수 있도록 하자.

---

## 참고

[자료구조 - 트라이(Trie)](https://baek.dev/til/data-structure/trie)
[[Algorithm] 트라이(Trie) 개념과 기본 문제](https://twpower.github.io/187-trie-concept-and-basic-problem)
[자바스크립트를 이용한 Trie 구현](https://velog.io/@teihong93/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-Trie-%EA%B5%AC%ED%98%84)
