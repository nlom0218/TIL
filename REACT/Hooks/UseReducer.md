# useReducer()

## 1. 개요

리액트에서는 보통 상태를 업데이트 하기 위해 `useState()`훅을 사용한다. 하지만 `useReducer()`훅을 사용하여 상태를 관리할 수 있다.

하지만 `useReducer()`훅이 가지고 있는 특별한 특징은 한 컴포넌트 내에서 `state`를 업데이트하는 로직 부분을 그 컴포넌트로부터 분리시키는 것이 가능하다. 상태 업데이트 로직을 컴포넌트 바깥에 작성 할 수도 있고, 심지어 다른 파일에 작성 후 불러와서 사용 할 수도 있다.

이러한 `useReducer()`훅에 대해 `useState()`훅과 비교하는 것을 시작하여 살펴보자.

---

## 2. useState() vs useReducer()

언제 `useState()`훅을 사용하는지 언제 `useReducer()`훅을 사용하는지는 정해진 기준은 없지만 보통 아래의 상황에 따라 나뉘어 `state`를 관리한다.

- `useState()`훅을 사용할 경우

  1. 관리해야 할 `state`가 1개일 경우
  2. 그 `state`가 단순한 숫자, 문자열 또는 `boolean`값일 경우

  ***

- `useReducer`훅을 사용할 경우
  1. 관리해야 할 `state`가 1개 이상, 복수일 경우
  2. 혹은 현재는 단일 `state`값만 관리하지만, 추후 유동적일 가능성이 있는 경우
  3. 스케일이 큰 프로젝트의 경우
  4. `state`의 구조가 복잡해질 것으로 보이는 경우

---

## 참고

[20. useReducer 를 사용하여 상태 업데이트 로직 분리하기](https://react.vlpt.us/basic/20-useReducer.html)  
[React Hooks :: useReducer에 대해 알아보기](https://velog.io/@iamhayoung/React-Hooks-useReducer%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)
