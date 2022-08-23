# Lifecycle

## 1. 개요

`Life Cycle`은 리액트 컴포넌트의 생명주기를 뜻한다. 즉, 리액트 컴포넌트가 브라우저상에 나타나고, 업데이트되고, 사라지게 되는데 이러한 과정이 `Life Cycle`이고 이때 호출 되는 메서드가 `Life Cycle Method(생명주기 메서드)`이다.

생명주기 메서드는 클래스형 컴포넌트에서만 사용 할 수 있기 때문에 `Life Cycle`개념에 대하여 너무 많은 시간을 쏟지 않아도 된다. 단지 어떤 개념이구나 정도로 정리하고 넘어가자.

---

## 2. Life Cycle & Life Cycle Method

![React LIfe Cycle](https://i.imgur.com/cNfpEph.png)

위는 리액트 컴포넌트가 생성, 업데이트, 제거 될 때의 과정을 나타낸 그림이다. 그림을 보면 각각의 과정에서 호출되는 메서드가 있다. 이것이 `Life Cycle Method`이다.

하나하나 살펴보자.

---

## 3. 마운트

컴포넌트가 생성(마운트)될 때 발생하는 생명주기 메서드는 아래와 같다.

- constructor: 컴포넌트의 생성자 메서드, 컴포넌트가 만들어지면 가장 먼저 실행 됨
- getDerivedStateFromProps: `props`로 받아온 것을 `state`에 넣어주고 싶을 때 사용
- render: 컴포넌트를 렌더링
- **componentDidMount**: 컴포넌트의 첫번째 렌더링이 마치고 나면 호출되는 메서드

---

## 4. 업데이트

컴포넌트가 업데이트 되는 시점에 발생하는 생명주기 메서드는 아래와 같다.

- getDerivedStateFromProps: 마운트과정에서 발생한 생명주기 메서드와 같음, 컴포넌트의 `props`나 `state`가 바뀌었을 때도 호출됨
- shouldComponentUpdate: 최적화 할 때 사용하는 메서드로 컴포넌트가 리렌더링 할지 말지를 결정하는 메서드
- render
- getSnapshotBeforeUpdate: 컴포넌트에 변화가 일어나기 직전의 DOM 상태를 가져올 수 있음. 그 다음 발생하게 되는 `componentDidUpdate` 함수에서 이전 값을 사용 가능하게 함
- **componentDidUpdate**: 리렌더링을 마치고, 화면에 우리가 원하는 변화가 모두 반영되고 난 뒤 호출되는 메서드

---

## 5. 언마운트

컴포넌트가 제거(언마운트)될 때 발생하는 생명주기 메서드는 아래와 같다.

- **componentWillUnmount**: 컴포넌트가 화면에서 사라지기 직전에 호출

---

## 6. 함수형 컴포넌트에서의 useEffect() 훅

지금까지 살펴본 생명주기 메서드는 클래스형 컴포넌트에서 다루는 것들이다. 이러한 생명주기 메서드들은 함수형 컴포넌트에서는 `useEffect()` 훅으로 구현이 가능하다.

아래의 3가지의 생명주기 메서드를 `useEffect()` 훅으로 구현하는 것을 마지막으로 이번 챕터를 마무리한다.

- componentDidMount

  ```jsx
  useEffect(() => {
    console.log("나 마운트 되었어!!!");
  }, []);
  ```

- componentDidUpdate

  ```jsx
  useEffect(() => {
    console.log("나 마운트 되었어!!!");
    console.log("그리고 user도 업데이트 되면 또 실행되!!!");
    // user는 리렌더링에 영향을 주어야 하는 값이어야 한다.
    // ex) state, props
  }, [user]);
  ```

- componentWillUnmount

  ```jsx
  useEffect(() => {
    console.log("나 마운트 되었어!!!");
    return () => {
      console.log("나 이제 언마운트 될거야ㅠㅠ");
    };
  }, []);
  ```

---

## 7. Conclusion

> 리액트를 배우면서 생명주기 메서드에 대해 자세히 배우기 보다는 클래스형 컴포넌트에서의 생명주기 메서드가 함수형 컴포넌트에서는 어떻게 사용이 되는지에 대해서 `useEffect()` 훅을 통해 자세히 배웠다. 어떤 강의에서도 `useEffect()` 훅에 대해 자세히 설명을 해주고 예시도 많이 들어주어 중요하다는 것을 알게 되었고 확실히 프로젝트에서 유용하게 사용된다.\
> 클래스형 컴포넌트의 생명주기 메서드도 한 때 너무 중요해서 꼭 알아야 하는 개념이었다고 생각한다. 지금 React Hooks 처럼 말이다. 그리고 언젠가 React Hooks도 다른 획기적인 무언가로 대체되는 날이 오겠지? React도...

---

## 참고

[25. LifeCycle Method](https://react.vlpt.us/basic/25-lifecycle.html)

---

📅 2022-07-12
