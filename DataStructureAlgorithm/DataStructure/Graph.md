# 그래프(Graph)

## 0. 요약

### 0-1. 인접 리스트로 무방향 그래프 구현하기

```javascript
class Graph {
  constructor() {
    this.vertices = new Map();
  }

  // 정점 추가하기
  add(vertex) {
    if (!this.vertices.get(vertex)) {
      this.vertices.set(vertex, []);
    }
  }

  // 정점 삭제하기
  remove(vertex) {
    if (this.vertices.get(vertex)) {
      const v = this.vertices.get(vertex);
      v.forEach((item) => {
        this.vertices.set(
          item,
          this.vertices.get(item).filter((item) => item !== vertex)
        );
      });
      this.vertices.delete(vertex);
      return v;
    }
  }

  // 정점의 존재여부 확인하기
  contains(vertex) {
    return this.vertices.has(vertex);
  }

  // 간선 연결하기
  connect(from, to) {
    if (this.contains(from) && this.contains(to)) {
      if (this.vertices.get(from).includes(to)) return;
      this.vertices.set(from, [...this.vertices.get(from), to]);
      this.vertices.set(to, [...this.vertices.get(to), from]);
    }
  }

  // 간선 삭제하기
  disconnect(from, to) {
    if (this.contains(from) && this.contains(to)) {
      this.vertices.set(
        from,
        this.vertices.get(from).filter((item) => item !== to)
      );
      this.vertices.set(
        to,
        this.vertices.get(to).filter((item) => item !== from)
      );
    }
  }

  // 간선 존재여부 확인하기
  hasEdge(from, to) {
    if (this.contains(from) && this.contains(to)) {
      const edges = this.vertices.get(from);
      return edges.includes(to);
    }
    return false;
  }
}
```

---

### 0-2. 인접 행렬로 방향 그래프 구현하기

```javascript
class Graph {
  constructor(size) {
    this.matrix = [];
    for (let i = 0; i < size; i++) {
      this.matrix.push(new Array(size).fill(0));
    }
    this.length = this.matrix.length;
    this.last_vertex = this.matrix.length - 1;
  }

  //정점의 존재여부 확인하기
  contains(vertex) {
    return vertex < this.length && vertex >= 0;
  }

  // 간선 연결하기
  connect(from, to) {
    if (
      !from === 0
        ? false
        : !from || to === 0
        ? false
        : !to || !this.contains(from) || !this.contains(to) || from === to
    )
      return;
    this.matrix[from][to] = 1;
  }

  // 간선 삭제하기
  disconnect(from, to) {
    if (
      !from === 0
        ? false
        : !from || to === 0
        ? false
        : !to || !this.contains(from) || !this.contains(to) || from === to
    )
      return;
    this.matrix[from][to] = 0;
  }

  // 간선 존재여부 확인하기
  hasEdge(from, to) {
    return this.matrix[from][to] === 1;
  }

  // 진입 차수 구하기
  in_degree_num(vertex) {
    let count = 0;
    this.matrix.forEach((item) => {
      if (item[vertex] === 1) count++;
    });
    return count;
  }

  // 진출 차수 구하기
  out_degree_num(vertex) {
    return this.matrix[vertex].filter((item) => item === 1).length;
  }

  // 전체 차수 구하기
  degree_num(vertex) {
    return this.in_degree_num(vertex) + this.out_degree_num(vertex);
  }
}
```

---

## 1. 개요

비선형 구조의 자료 구조의 첫 정리는 그래프 자료 구조로 시작한다. 선형 구조의 자료 구조보다 생김새는 복잡하여
무서운 느낌이 들지만 한 번 정리를 하여 특징과 쓰임 그리고 자바스크립트로 구현을 해보도록 하자.

---

## 2. 그래프란?

노드(N, node)와 그 노드를 연결하는 간선(E, edge)을 하나로 모아 놓은 자료 구조이다. 즉, 연결되어 있는 객체
간의 관계를 표현할 수 있는 자료 구조이다.

![Graph](https://miro.medium.com/max/488/0*UgMHEDLriw2efXbx)

---

### 2-1. 그래프에서 사용되는 용어

- 정점(vertex): 위치(node라고도 부름)
- 간선(edge): 위치 간의 관계 즉, 노드를 연결하는 선(link, branch라고도 부름)
- 인접 정점(adjacent vertex): 간선에 의해 직접 연결된 정점
- 차수(degree): 정점에 부속되어 있는 간선의 수
  - 진입 차수(in-degree): 방향 그래프에서 외부에서 오는 간선의 수
  - 진출 차수(out-degree): 방향 그래프에서 외부로 향하는 간선의 수
- 경로(path): 특정 정점에서 다른 정점까지 간선으로 연결된 정점으로 순서대로 나열한 리스트
  - 경로 길이(path length): 경로를 구성하는 데 사용된 간선의 수
  - 단순 경로(simple path): 경로 중에서 반복되는 정점이 없는 경우
- 사이클(cycle): 단순 경로의 시작 정점과 종료 정점이 동일한 경우

---

### 2-2. 그래프의 특징

- 그래프는 네트워크 모델
- 2개 이상의 경로가 가능
- 루트 노드의 개념이 없음
- 부모-자식 관계의 개념이 없음
- 순회는 DFS나 BFS로 이루어짐
- 그래프는 크게 무방향 그래프와 방향 그래프로 나뉨

---

## 3. 그래프의 종류

- 방향의 유무에 따른 분류
  - 무방향 그래프
  - 방향 그래프
- 연결 상태에 따른 분류
  - 연결 그래프
  - 비연결 그래프
  - 완전 그래프

![그래프의 종류](https://laboputer.github.io/assets/img/algorithm/ds/06_graph2.PNG)

---

### 3-1. 무방향 그래프(Undirected Graph)

두 정점을 연결하는 간선에 방향이 없는 그래프

![무방향 그래프](https://api.ahribori.com/image/azAgjKCafNrmtUbR3YFZ7veA.png)

방향 그래프에서 정점 Vi와 Vj를 연결하는 간선을 `<Vi, Vj>`로 표현한다. 이때 `<Vi, Vj>`와 `<Vj, Vi>`는
같은 간선이다.

예) 양방향 통행 도로

---

### 3-2. 방향 그래프(Directed Graph)

간선에 방향이 있는 그래프

![방향 그래프](https://blog.kakaocdn.net/dn/cpufQZ/btqNthgiQp9/6JFP5VlNUuRGzwNohdccr0/img.png)

방향 그래프에서 정점 Vi와 Vj를 연결하는 간선을 `<Vi, Vj>`로 표현한다. 이때 `<Vi, Vj>`와 `<Vj, Vi>`는
서로 다른 간선이다.

예) 일방 통행 도로

---

### 3-3. 연결 그래프(Connected Graph)

떨어져 있는 정점이 없는 그래프, 무방향 그래프에 있는 모든 정점쌍에 대해서 항상 경로가 존재

---

### 3-4. 비연결 그래프(Disconnected Graph)

연결되지 않은 정점이 있는 그래프, 무방향 그래프에서 특정 정점쌍 사이에 경로가 존재하지 않음

![연결 그래프 vs 비연결 그래프](https://blog.kakaocdn.net/dn/coZTm9/btq7DTzeZvf/HqyzGKTiG6kzftEpqCZNR0/img.png)

---

### 3-5. 완전 그래프(Complete Graph)

한 정점에서 다른 모든 정점과 연결되어 최대 간선 수를 가지는 그래프

![완전 그래프](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQRuqfJ_j_1ddGiUumFxnUkSJbM79XR8H604nkoILqH5U96fz62DKUhgLxwuPe_k1egFKY&usqp=CAU)

정점이 n개인 완전 그래프에서의 최대 간선 수

- 무방향 그래프: n(n-1)/2
- 방향 그래프: n(n-1)

---

### 3-6. 가중치 그래프(Weight Graph)

정점을 연결하는 간서에 가중치를 할당한 그래프, 네트워크라도 함

![가중치 그래프](https://miro.medium.com/max/488/0*UgMHEDLriw2efXbx)

예) 도시-도시의 연결, 도로의 길이, 통신망의 사용료

---

### 3-7. 사이클(Cycle)

단순 경로의 시작 정점과 종료 정점이 동일한 경우

---

### 3-8 비순환 그래프(Acyclic Graph)

사이클이 없는 그래프

![사이클 vs 비순환 그래프](https://blog.kakaocdn.net/dn/uDB8l/btq9gXVVkTF/xQU9xlfVxOLfagNiXvKp0K/img.png)

---

## 4. 그래프의 구현방법

그래프는 인접 리스트(Adjacency List)와 인접 행렬(Adjacency Matrix) 두 가지의 방법으로 구현할 수 있다.

두 가지 방법의 특징은 아래와 같다.

- 인접 리스트
  - 모든 정점을 인접 리스트에 저장한다. 배열 또는 연결 리스트를 이용해서 인접 리스트를 표현한다.
  - 무방향 그래프에서 간선은 두 번 저장된다.
  - **그래프 내 적은 숫자의 간선만을 가지는 희소 그래프(Sparse Graph)의 경우에 적합하다.**
  - 어떤 노드에 인접한 노드들을 쉽게 찾을 수 있다.
  - 그래프에 존재하는 모든 간선의 수는 인접 리스트 전체를 조사하면 알 수 있다.
  - 간선의 존재 여부와 정점의 차수를 알기 위해서는 정점 i의 리스트에 있는 노드의 수(정점 차수)만큼 시간이 필요하다.
- 인접 행렬
  - N\*N Boolean Matrix로써 matrix[i][j]가 true라면 i에서 j로 가는 간선이 있다는 뜻이다.
  - 간선의 수와 무관하게 항상 n^2개의 메모리 공간이 필요하다.
  - **그래프에 간선이 많이 존재하는 밀집 그래프(Dense Graph)의 경우에 적합하다.**
  - 두 정점을 연결하는 간선의 존재 여부(M[i][j])를 O(1) 안에 즉시 알 수 있다.
  - 정점의 차수는 인접 배열의 i번 째 행 또는 열을 모두 더하면 알 수 있다.
  - 어떤 노드에 인접한 노드들을 찾기 위해서는 모든 노드를 전부 순회해야 한다.
  - 그래프에 존재하는 모든 간선의 수는 O(N^2)안에 알 수 있다.

---

## 5. 인접 리스트로 무방향 그래프 구현하기

### 5-1. 그래프의 기본 구조

```javascript
class Graph {
  constructor() {
    this.vertices = new Map();
  }
}
```

그래프의 기본 구조는 정점을 나타내는 `vertices`로 구성된다. 그리고 `vertices`는 맵 객체이다.

> 맵 객체의 키는 기본적으로 중복이 불가능하다. 하지만 키가 객체인 경우에는 중복이 발생한다. 그렇기 때문에
> 해당 방법으로 그래프를 만들기 위해서는 정점은 객체가 아닌 값을 사용해야 한다. 즉, string, number, boolean과 같은
> 원시 타입의 자료형을 사용해야 한다. 이 부분이 조금은.. 아쉽다ㅠㅠ

---

### 5-2. 정점 추가하기

```javascript
class Graph {
  // ...
  add(vertex) {
    if (!this.vertices.get(vertex)) {
      this.vertices.set(vertex, []);
    }
  }
}
```

넘겨받은 `vertex`은 맵 객체의 키가 되며 빈 배별을 값으로 할당한다.
만약 이미 존재하는 정점이라면 새롭게 생성을 하지 않는다.

---

### 5-3. 정점 삭제하기

```javascript
class Graph {
  // ...
  remove(vertex) {
    if (!this.vertices.get(vertex)) {
      const v = this.vertices.get(vertex);
      v.forEach((item) => {
        this.vertices.set(
          v,
          this.vertices.get(v).filter((item) => item !== vertex)
        );
      });
      this.vertices.delete(vertex);
      return v;
    }
  }
}
```

인자로 넘겨받은 정점이 존재하면 해당 정점을 삭제함은 물론, 다른 모든 정점들의 리스트를 순회하며 넘겨받은 정점과
이어져 있는 간선을 제거해야 한다.

넘겨받은 정점과 연결되어 있는 정점은 `넘겨받은 정점의 리스트(v)`를 보면 쉽게 찾을 수 있다.

---

### 5-4. 정점의 존재여부 확인하기

```javascript
class Graph {
  // ...
  contains(vertex) {
    return this.vertices.has(vertex);
  }
}
```

`Map.has()` 메서드를 사용하여 키가 존재하는지 확인하다.

---

### 5-5. 간선 연결하기

```javascript
class Graph {
  // ...
  connect(from, to) {
    if (this.contains(from) && this.contains(to)) {
      if (this.vertices.get(from).includes(to)) return;
      this.vertices.set(from, [...this.vertices.get(from), to]);
      this.vertices.set(to, [...this.vertices.get(to), from]);
    }
  }
}
```

간선을 연결하기 위해서는 연결할 정점이 모두 존재해야 한다.

두 개의 정점의 존재가 모두 확인되면 각각의 인접 리스트에 서로의 정점을 추가한다.
만약 두 정점이 이미 연결이 되어 간선이 있다면 함수를 종료하여 중복된 간선을 만들지 않도록 한다.

---

### 5-6. 간선 삭제하기

```javascript
class Graph {
  // ...
  disconnect(from, to) {
    if (this.contains(from) && this.contains(to)) {
      this.vertices.set(
        from,
        this.vertices.get(from).filter((item) => item !== to)
      );
      this.vertices.set(
        to,
        this.vertices.get(to).filter((item) => item !== from)
      );
    }
  }
}
```

두 정점을 연결하는 간선을 삭제하기 위해서는 두 정점이 모두 존재해야 한다.

두 정점이 모두 존재하면 `from` 정점과 `to` 정점의 리스트에서 서로의 정점을 제거한다. 이를 위해 `Array.filter()` 메서드를
사용하였다.

---

### 5-7. 간선 존재여부 확인하기

```javascript
class Graph {
  // ...
  hasEdge(from, to) {
    if (this.contains(from) && this.contains(to)) {
      const edges = this.vertices.get(from);
      return edges.includes(to);
    }
    return false;
  }
}
```

두 정점이 모두 존재하지 않으면 `false`를 리턴한다.

두 정점이 모두 존재하면 `from` 정점의 리스트에 `to` 정점이 포함되어있는지 `Array.includes()` 메서드를
사용하여 확인하여 메서드의 반환 값을 리턴한다.

---

## 6. 인접 행렬로 방향 그래프 구현하기

### 6-1. 그래프의 기본 구조

```javascript
class Graph {
  constructor(size) {
    this.matrix = [];
    for (let i = 0; i < size; i++) {
      this.matrix.push(new Array(size).fill(0));
    }
    this.length = this.matrix.length;
    this.last_vertex = this.matrix.length - 1;
  }
}
```

인자로 받은 `size` 만큼의 이중 배열이 생성된다.

생성되는 정점의 수는 `matrix.length`이다. 하지만 여기서 주의 해야할 점은 배열의 인덱스는 `0`부터
세기 때문에 간선을 연결할 때 `matrix.length`의 값은 인자로 전달해서는 안된다. 즉 받을 수 있는 인자 값의
최댓값은 `matrix.length - 1`이다.

---

### 6-2. 정점의 존재여부 확인하기

```javascript
class Graph {
  // ...
  contains(vertex) {
    return vertex < this.length && vertex >= 0;
  }
}
```

인자로 받은 정점이 `0` 이상 `matrix`의 길이보다 보다 작으면 정점이 존재한다.

---

### 6-3. 간선 연결하기

```javascript
class Graph {
  // ...
  connect(from, to) {
    if (
      !from === 0
        ? false
        : !from || to === 0
        ? false
        : !to || !this.contains(from) || !this.contains(to) || from === to
    )
      return;
    this.matrix[from][to] = 1;
  }
}
```

간선을 연결하기 위해서는 연결할 두 정점이 필요하고 두 정점 모두 존재해야 한다. 또한 연결할 두 정점이
서로 같지 않아야 한다. 단 정점이 `0`인 경우 `!0`은 `true`를 반환하기 때문에
이를 위해 삼항연산자를 작성해줘야 한다.

정점이 연결 된 경우 해당 위치의 요소를 1로 바꾼다.

> 물론 자기 자신을 간선으로 연결할 수 있지만 여기서는 제외하여 생각한다.

---

### 6-4. 간선 삭제하기

```javascript
class Graph {
  // ...
  disconnect(from, to) {
    if (
      !from === 0
        ? false
        : !from || to === 0
        ? false
        : !to || !this.contains(from) || !this.contains(to) || from === to
    )
      return;
    this.matrix[from][to] = 0;
  }
}
```

정점이 연결이 끊긴 경우 해당 위치의 요소를 0로 바꾼다.

---

### 6-5. 간선 존재여부 확인하기

```javascript
class Graph {
  // ...
  hasEdge(from, to) {
    return this.matrix[from][to] === 1;
  }
}
```

---

### 6-6. 차수 구하기

```javascript
class Graph {
  // ...

  // 진입 차수 구하기
  in_degree_num(vertex) {
    let count = 0;
    this.matrix.forEach((item) => {
      if (item[vertex] === 1) count++;
    });
    return count;
  }

  // 진출 차수 구하기
  out_degree_num(vertex) {
    return this.matrix[vertex].filter((item) => item === 1).length;
  }

  // 차수 구하기
  degree_num(vertex) {
    return this.in_degree_num(vertex) + this.out_degree_num(vertex);
  }
}
```

진입 차수는 외부에서 정점으로 향하는 간선의 수 이고 진출 차수는 정점에서 외부로 향하는 간선의 수이다.

진입 차수는 이중 배열에서 세로의 요소, 진출 차수는 이중 배열에서 가로의 요소를
확인하여 구하면 된다.

---

## 7. Conclusion

> 그래프 자료 구조에 대해 정리를 하였다. 개념을 공부할 땐 어떤 자료 구조인지
> 정확히 이해가 되지 않았다. 하지만 직접 코드를 작성하면서 그래프를 만들어 보니
> 확실히 어떤 상황에서 유용하게 사용할 수 있는지 짐작이 되었다. 지금까지 자료구는
> 원시타입, 배열, 객체만 이용했었는데 이렇게 직접 자료 구조를 만드니 정말 재밌다는
> 생각이 든다. 또한 왜 자바스크립트에서는 다양한 자료 구조를 지원하지 않는지
> 궁금하다.

---

## 참고

[[자료구조] 그래프(Graph)란](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)  
[[자료구조] 그래프(Graph)의 개념 설명](https://leejinseop.tistory.com/43)  
[[JavaScript] 자료구조 (3-2): 그래프(Graph)와 인접 리스트](https://velog.io/@porupit0122/JavaScript-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-3-2-%EA%B7%B8%EB%9E%98%ED%94%84Graph%EC%99%80-%EC%9D%B8%EC%A0%91-%EB%A6%AC%EC%8A%A4%ED%8A%B8)  
[Javascript Graph 자료구조 인접행렬(Adjacency Matrix) 구현](https://about-tech.tistory.com/entry/Algorithm-Javascript-Graph-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EC%9D%B8%EC%A0%91%ED%96%89%EB%A0%ACAdjacency-Matrix-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)
