# 그래프(Graph)

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

### 5-3. 정점의 존재여부 확인하기

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

### 5-4. 간선 연결하기

```javascript
class Graph {
  // ...
  connect(from, to) {
    if (this.contains(from) && this.contains(to)) {
      this.vertices.set(from, [...this.vertices.get(from), to]);
      this.vertices.set(to, [...this.vertices.get(to), from]);
    }
  }
}
```

간선을 연결하기 위해서는 연결할 정점이 모두 존재해야 한다.

두 개의 정점의 존재가 모두 확인되면 각각의 인접 리스트에 서로의 정점을 추가한다.

---

## 참고

[[자료구조] 그래프(Graph)란](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)  
[[자료구조] 그래프(Graph)의 개념 설명](https://leejinseop.tistory.com/43)  
[[JavaScript] 자료구조 (3-2): 그래프(Graph)와 인접 리스트](https://velog.io/@porupit0122/JavaScript-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-3-2-%EA%B7%B8%EB%9E%98%ED%94%84Graph%EC%99%80-%EC%9D%B8%EC%A0%91-%EB%A6%AC%EC%8A%A4%ED%8A%B8)
