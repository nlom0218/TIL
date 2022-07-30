# useEffect()

## 👉 바로가기

- [1. 개요](#1-개요)

---

## 1. 개요

리액트에서 `useState()`과 함께 가장 많이 사용하는 Hook은 `useEffect()`이다. `useEffect()`는 리액트 컴포넌트가 랜더링 될 때마다 특정 작업을 실행할 수 있도록 도움을 준다. 이러한 기능은 클래스형 컴포넌트의 생명주기 메서드(componentDidMount, componentDidUpdate, compoenntWillUnMount)와 같은 기능이다.

---

## 2. useEffect() 사용하기

기본형태 `useEffect(function, deps)`

- function: 수행하고자 하는 작업
- deps: 배열 형태이며, 배열 안에는 검사하고자 하는 특정 값이 들어가거나 또는 빈 배열이다.

아래는 간단한 `useEffect()`의 사용예시이다.

```js
import React, { useEffect } from "react";

const App = () => {
  useEffect(() => {
    consolo.log("Hello useEffect()!");
  }, []);
  return <div></div>;
};

export default App;
```

---

---

## 참고

[[React] 리액트 Hooks : useEffect() 함수 사용법](https://cocoon1787.tistory.com/796)
