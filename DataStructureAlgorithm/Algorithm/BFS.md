# 너비 우선 탐색(BFS)

## 1. 개요

너비 우선 탐색(BFS, Breadth-First Search)는 그래프 탐색에 사용하는 알고리즘이다. 그래프 탐색은
하나의 정점으로부터 시작하여 차례대로 모든 정점들을 한 번씩 방문하는 것을 말한다.

---

## 2. 너비 우선 탐색이란?

너비 우선 탐색은 루트 노드(혹은 다른 임의의 노드)에서 시작해서 인접한 노드를 먼저 탐색하는 방법을 말한다.

시작 정점으로부터 가까운 정점을 먼저 방문하고 멀리 떨어져 있는 정점을 나중에 방문하는 순회 방법이다. 즉, 깊게(deep) 탐색하기
전에 넓게(wide) 탐삭해는 것이다.

![BFS](https://velog.velcdn.com/images%2Fsukong%2Fpost%2F103fbeed-3f70-4074-9a7d-76915a7764f2%2FBFS.png)

주로 **두 노드 사이의 최단 경로** 혹은 **임의의 경로를 찾고 싶을 때** 사용한다.

---

## 3. 너비 우선 탐색의 특징

1. 직관적이지 않은 면이 있다.
   - 시작 노드에서 시작해서 거리에 따라 단계별로 탐색한다고 볼 수 있다.
2. 재귀적으로 동작하지 않는다.
3. 어떤 노드를 방문했었는지 여부를 반드시 검사해야 한다.
4. 방문한 노드들을 차례대로 저장한 후 꺼낼 수 있는 자료 구조인 큐(Queue)를 사용한다. 선입선출 원칙으로 탐색한다.

---

## 4. BFS 구현하기

```javascript
const graph = {
  A: ["B", "C"],
  B: ["A", "D"],
  C: ["A", "G", "H", "I"],
  D: ["B", "E", "F"],
  E: ["D"],
  F: ["D"],
  G: ["C"],
  H: ["C"],
  I: ["C", "J"],
  J: ["I"],
};

const BFS = (graph, startNode) => {
  const visited = [];
  let needVisit = [];

  needVisit.push(startNode);

  while (needVisit.length !== 0) {
    const node = needVisit.shift();
    if (!visited.includes(node)) {
      visited.push(node);
      needVisit = [...needVisit, ...graph[node]];
    }
  }
};
```

---

## 참고

[[알고리즘] 너비 우선 탐색(BFS)이란](https://gmlwjd9405.github.io/2018/08/15/algorithm-bfs.html)
