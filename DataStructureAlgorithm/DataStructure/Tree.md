# 트리(Tree)

## 0. 요약

연결 리스트로 이진 탐색 트리 구현하기

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.left = null;
    this.right = null;
  }
}

class Tree {
  constructor() {
    this.root = null;
    this.size = 0;
  }

  // 노드 추가하기
  add(data) {
    const root = this.root;
    const node = new Node(data);
    if (!root) {
      this.size++;
      this.root = node;
      return;
    }
    const searchTree = (parents) => {
      if (data < parents.data) {
        if (parents.left) return searchTree(parents.left);
        else return (parents.left = node);
      } else if (data > parents.data) {
        if (parents.right) return searchTree(parents.right);
        else return (parents.right = node);
      } else return null;
    };
    this.size++;
    searchTree(root);
  }

  // 노드 탐색하기
  search(data) {
    const root = this.root;
    if (!root) return null;
    let depth = 0;
    const searchNode = (node, parents) => {
      if (data < node.data) {
        if (!node.left) return null;
        depth++;
        return searchNode(node.left, node);
      } else if (data > node.data) {
        if (!node.right) return null;
        depth++;
        return searchNode(node.right, node);
      } else return { node, depth, parents };
    };
    return searchNode(root, null);
  }

  //  같은 레벨의 노드 구하기
  getSameLevel(level) {
    const arr = [];
    const searchNode = (node, depth) => {
      if (!node) return;
      if (depth === level) return arr.push(node);
      else {
        searchNode(node.left, depth + 1);
        searchNode(node.right, depth + 1);
      }
    };
    searchNode(this.root, 0);
    return arr;
  }

  // 특정 노드에서 다른 노드로 이동하는 경로 구하기
  getPath(from, to) {
    const from_node = this.search(from);
    if (!from_node) {
      return null;
    }
    let path = [];
    let cur_node = from_node.node;
    let edge = 0;
    let loop = true;
    while (loop) {
      if (cur_node.data < to) cur_node = cur_node.right;
      else cur_node = cur_node.left;
      if (!cur_node) {
        return null;
      } else if (cur_node.data === to) {
        loop = false;
      } else {
        path.push(cur_node);
      }
      edge++;
    }
    return {
      from: from_node,
      to: this.search(to),
      edge,
      path,
    };
  }
}
```

---

## 1. 개요

그래프의 한 종류인 트리 자료 구조에 대해 정리한다.

---

## 2. 트리란?

트리는 방향성이 있는 비순환 그래프의 한 종류이며 **최소 연결 트리** 라고도 불린다.
또한 노드와 노드들을 연결하는 간선들로 구성되어 있으며 파일의 구조,
조직도와 같은 **계층적 관계**를 표현하는데 적합한 자료 구조이다.

![tree](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRQuNcBdCLO4QmhIjCQHTnN-D9JV2xXwENwzg&usqp=CAU)

---

### 2-1. 트리에서 사용되는 용어

![tree 용어](https://gmlwjd9405.github.io/images/data-structure-tree/tree-terms.png)

- 노드
  - 루트 노드(root node): 부모가 없는 노드, 트리는 하나의 루트 노드를 가짐
  - 단말 노드(leaf node): 자식이 없는 노드
  - 내부(internal) 노드: 단말 노드가 아닌 노드
  - 형제(sibling): 같은 부모를 가지는 노드
  - 노드의 크기(size): 자신을 포함한 모든 자손 노드의 개수
  - 노드의 깊이(depth): 루트에서 어떤 노드에 도달하기 위해 거쳐야 하는 간선의 수
  - 노드의 레벨(level): 트리의 특정 깊이를 가지는 노드의 집합
  - 노드의 차수(degree): 노드의 간선 수
- 간선(edge): 노드를 연결하는 선
- 트리의 차수(degree of tree): 트리의 최대 차수
- 트리의 높이(height): 루트 노드에서 가장 깊숙히 있는 노드의 깊이

---

### 2-2. 트리의 특징

- 계층 모델
- 사이클이 없음
- 노드가 N개인 트리는 항상 N-1개의 간선(edge)을 가짐
- 루트에서 어떤 노드로 가는 경로는 유일함
- 한 개의 루트 노드만이 존재
- 모든 자식 노드는 한 개의 부모 노드만을 가짐

---

## 3. 트리의 종류

### 3-1. 이진 트리(Binary Tree)

이진 트리는 트리 자료구조의 여러 가지 유형 중 가장 기본이 되는 트리이다. 이진 트리는 2개 이하의 자식 노드를 가지며
왼쪽의 노드를 `Left Node`라고 하고, 오른쪽의 노드를 `Right Node`라고 한다.

![Binary Tree](https://t1.daumcdn.net/cfile/tistory/99E6B23E5B339DBB33)

이러한 이진 트리는 여러 가지 유형으로 나뉜다.

---

### 3-2. 편향 이진 트리(Skewed Binary Tree)

편향 이진 트리는 노드가 하나의 차수로만 이루어져 있는 경우를 말한다. 선형 자료 구조이므로 테이터 탐색에 있어
효율적이지 못한 단점이 있다.

![Skewed Binary Tree](https://velog.velcdn.com/images%2Fkimdukbae%2Fpost%2F349a3344-f85e-4a66-ab4e-5d16027f249c%2Fimage.png)

---

### 3-3. 포화 이진 트리(Perfect Binary Tree)

포화 이진 트리는 `leaf node`를 제외한 모든 노드의 차수가 2개로 이루어져 있는 경우를 말한다.

![Full Binary Tree](https://t1.daumcdn.net/cfile/tistory/251FE74B5100D04F2F)

---

### 3-4. 완전 이진 트리(Complete Binary Tree)

마지막 레벨을 제외하고 모든 레벨에 노드가 완전히 채워져 있는 이진 트리이다. 마지막 레벨에서는 노드가
꽉 차 있지 않아도 되지만 노드가 왼쪽에서 오른쪽으로 채워져야 한다.

완전 이진 트리는 배열을 사용해 효율적으로 표현이 가능하다.

![Complete Binary Tree](https://blog.kakaocdn.net/dn/b7BofG/btq9Eilu1J5/0HNO2KiWkBxTvERSJGHla0/img.png)

---

### 3-5. 이진 탐색 트리(Binary Search Tree)

탐색을 위한 이진 트리 기반의 자료 구조이다.

- 모든 노드는 중복된 값을 가지지 않음
- Left Node: 부모노드보다 작은 값이 저장
- Right Node: 부모노드와 값이 같거나 큰 값이 저장

![Binary Search Tree](https://blog.penjee.com/wp-content/uploads/2015/11/binary-search-tree-insertion-animation.gif)

이진 탐색 트리는 데이터를 효율적으로 탐색할 수 있는 장점이 있다. 아래의 그림을 통해 트리에서의 데이터 탐색과
배열에서의 데이터 탐색의 단계를 보자.

![Binary Search Tree vs Array](https://blog.penjee.com/wp-content/uploads/2015/11/binary-search-tree-sorted-array-animation.gif)

---

## 4. 트리의 순회

트리의 각 노드를 탐색하는 과정인 트리의 순회는 노드를 탐색하는 순서에 따라 `전위 순회`, `중위 순회`, `후위 순회`
3가지로 분류할 수 있다.

---

### 4-1. 전위 순회(Preorder)

`루트 노드 -> 왼쪽 서브트리 -> 오른쪽 서브트리`의 순서로 순회하는 방식으로 `깊이 우선 순회`라고도 불린다.

![Preorder](https://upload.wikimedia.org/wikipedia/commons/a/ac/Preorder-traversal.gif)

---

### 4-2. 중위 순회(Inorder)

`왼쪽 서브트리 -> 노드 -> 오른쪽 서브트리`의 순서로 순회하는 방식으로 `대칭 순회`라고도 불린다.

![Inorder](https://upload.wikimedia.org/wikipedia/commons/4/48/Inorder-traversal.gif)

---

### 4-3. 후위 순회(Postorder)

`왼쪽 서브트리 -> 오른쪽 서브트리 -> 노드`의 순서로 순회하는 방식이다.

![Postorder](https://upload.wikimedia.org/wikipedia/commons/2/28/Postorder-traversal.gif)

---

## 5. 연결 리스트로 이진 탐색 트리 구현하기

### 5-1. 인진 탐색 트리의 기본 구조

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.left = null;
    this.right = null;
  }
}

class Tree {
  constructor() {
    this.root = null;
    this.size = 0;
  }
}
```

이진 탐색 트리에서의 노드는 두 개의 자식 노드를 가지므로 `left`와 `rigth`의 두 가지의 값을 가져야 한다.
트리의 기본 구조에는 `root node`를 가리키는 `this.root`가 있고 노드의 개수를 뜻하는 `this.size`를
기본값 0으로 하여 선언한다. 참고로 트리의 총 간선의 수는 `this.size - 1`이 된다.

---

### 5-2. 노드 추가하기

```javascript
class Tree {
  // ...
  add(data) {
    const root = this.root;
    const node = new Node(data);
    if (!root) {
      this.size++;
      this.root = node;
      return;
    }
    const searchTree = (parents) => {
      if (data < parents.data) {
        if (parents.left) return searchTree(parents.left);
        else return (parents.left = node);
      } else if (data > parents.data) {
        if (parents.right) return searchTree(parents.right);
        else return (parents.right = node);
      } else return null;
    };
    this.size++;
    searchTree(root);
  }
}
```

만약 `root node`가 없다면 `root node`에 새롭게 만들어진 노드를 할당한다.

하지만 `root node`가 있다면 새롭게 만들어진 노드를 저장할 위치를 찾아야 한다. `data`와 부모 노드의 값을
비교하여 `left node` 또는 `right node`에 새롭게 만들어진 노드를 할당해야 한다. 하지만 만약
이미 부모 노드의 `left node` 또는 `right node`가 있다면 다시 함수를 호출해야 한다. 함수를 호출 할 때 마다
노드의 깊이는 늘어난다.

---

### 5-3. 노드 탐색하기

```javascript
class Tree {
  // ...
  search(data) {
    const root = this.root;
    if (!root) return null;
    let depth = 0;
    const searchNode = (node, parents) => {
      if (data < node.data) {
        if (!node.left) return null;
        depth++;
        return searchNode(node.left, node);
      } else if (data > node.data) {
        if (!node.right) return null;
        depth++;
        return searchNode(node.right, node);
      } else return { node, depth, parents };
    };
    return searchNode(root, null);
  }
}
```

만약 트리가 비어있으면(`root node` 조차 없을 때) `null`을 반환한다.

그 외의 경우엔 `searchNode` 함수가 재귀적으로 실행된다. `searchNode` 함수가 한 번 실행 될 때마다 `depth`는 1씩
증가된다. 이는 해당 `노드의 깊이`가 된다. `searchNode` 함수는 `root node`부터 시작하여
인수로 전달 받은 `data`가 특정 노드의 `data`와 같은 값이 될 때까지 실행이 된다. 만약 같은 값을 가지는
`node`를 찾기 전 해당 `node`의 자식 노드가 없다면 `null`를 반환한다.

또한 노드의 삭제를 위해 `parents`의 값으로 부모 노드를 함께 반환한다.

> 아직 노드 삭제를 위한 메서드는 구현하지 않았다. 먼가 복잡해..ㅠㅠ

---

### 5-4. 같은 레벨의 노드 구하기

```javascript
class Tree {
  // ...
  getSameLevel(level) {
    const arr = [];
    const searchNode = (node, depth) => {
      if (!node) return;
      if (depth === level) return arr.push(node);
      else {
        searchNode(node.left, depth + 1);
        searchNode(node.right, depth + 1);
      }
    };
    searchNode(this.root, 0);
    return arr;
  }
}
```

`searchNode`를 재귀적으로 호출한다. 두 번째 파라미터인 `depth`가 인수로 전달받은 `level`과 같아질 때
의 노드를 `arr`에 해당 노드를 넣고 함수를 종료한다.

---

### 5-5. 특정 노드에서 다른 노드로 이동하는 경로 구하기

```javascript
class Tree {
  // ...
  getPath(from, to) {
    const from_node = this.search(from);
    if (!from_node) {
      return null;
    }
    let path = [];
    let cur_node = from_node.node;
    let edge = 0;
    let loop = true;
    while (loop) {
      if (cur_node.data < to) cur_node = cur_node.right;
      else cur_node = cur_node.left;
      if (!cur_node) {
        return null;
      } else if (cur_node.data === to) {
        loop = false;
      } else {
        path.push(cur_node);
      }
      edge++;
    }
    return {
      from: from_node,
      to: this.search(to),
      edge,
      path,
    };
  }
}
```

출발하는 노드인 `from_node`가 없다면 경로 자체도 없기 때문에 `null`를 반환한다.

그렇지 않으면 반복문을 실행하게 되는데 `cur_node`의 `data`값과 `to`의 값을 비교하여 `left`, `right` 중
탐색할 노드를 정한다. 탐색을 진행하면서 더 이상 노드가 없다면 `null`를 반환한다. 이는 `to`에 해당하는
노드가 경로에 없다는 뜻이다.

하지만 만약 `cur_node`의 `data`값이 `to`값과 같아진다면 루프를 종료한다. 그 전까지는 `cur_node`를
계속 `path` 배열에 넣고 `egde`를 1증가 시킨다.

반환하는 값

- from: 출발하는 노드
- to: 도착하는 노드
- edge: 도착할 때 까지 거친 간선의 수
- path: `from`과 `to`사이에 존재하는 노드를 요소로 가지는 배열

---

## 6. Conclusion

> 확실히 블로그를 보고 그대로 뺏기는 것 보다 해당 자료 구조의 특징을 생각하며 자료 구조를 구현하는 것이
> 재미있다. 물론 이번 트리 자료 구조를 연결 리스트로 구현하다가 노드의 삭제에서 막힌 부분이 생겨 이후에
> 공부를 좀 더 하고 다시 구현을 하겠지만 나머지 메서드를 구현은 정말로 재미있었다. 앞서 정리한 자료 구조
> 보다 코드의 길이가 길고 생각할 부분이 많이 있어 힘들었지만 잘 작동하는 모습을 보니 뿌듯한 마음이 든다.
> 한 1년 뒤, 2년 뒤 위의 코드를 다시 보면 고치고 싶은 내용들이 많이 있을거고 코드 상에서 문제가 있을 수 있다.
> 틀린 부분을 발견하면 바로바로 수정하자!

---

## 출처

[[자료구조] 트리(Tree)란](https://gmlwjd9405.github.io/2018/08/12/data-structure-tree.html)  
[[자료구조] 트리 (Tree)](https://velog.io/@kimdukbae/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%ED%8A%B8%EB%A6%AC-Tree)

---

📅 2022-09-12
