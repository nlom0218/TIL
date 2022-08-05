# State

## 1. 개요

리액트에서 props가 중요하듯이 state의 개념도 중요하다. 과거에는 클래스형 컴포넌트에서만 state를 사용해 상태를 관리할 수 있었는데 리액트 16.8v 이후에는 리액트에 Hook개념이 도입되어 함수형 컴포넌트에서도 state를 사용하여 상태를 관리할 수 있게 되었다.

함수형 컴포넌트의 `useState Hook`를 통해 state를 사용한다. `useState`는 `Hooks`파트에서 더 자세히 다루도록 하고 여기서는 리액트의 `state`개념에 대해 다루도록 한다.

---

## 2. State란?

- 일반적으로 컴포넌트의 내부에서 변경 가능한 데이터를 관리해야할 때에 사용한다.
- 프로퍼티(props)의 특징은 컴포넌트 내부에서 값을 바꿀 수 없다는 것이다. 하지만 값을 바꿔야 하는 경우도 분명 존재하며, 이럴때 state라는 것을 사용한다.
- 값을 저장하거나 변경할 수 있는 객체로 보통 이벤트와 함께 사용된다.
- 컴포넌트에서 동적인 값을 상태(state)라고 부르며, 동적인 데이터를 다룰 때 사용된다 볼 수 있다.

---

## 3. state의 특징

- 리액트의 state는 하나의 자바스크립트의 객체이다.
- 렌더링이나 데이터 흐름에 사용되는 값만 state에 포함시켜야 한다. state가 변경될 경우 컴포넌트가 재렌더링되기 때문에 렌더링과 데이터 흐름에 관련 없는 값은 포함시키지 않는다.
- state는 직접적인 변경이 불가능한다. `useState Hook`에서 다루게 되는 `setState()`함수를 통해 state를 변경해야 한다. 즉 `setState()`함수를 통해 상태 변경을 추적하고 변경에 따라 구성 요소를 다시 렌더링을 할 수 있다.

---

## 4. state를 직접 변경을 해서는 안되는 이유

1. state를 직접 수정하게 되면 리액트가 render 함수를 호출하지 않아 상태 변경이 일어나도 렌더링이 일어나지 않을 수 있다.
2. setState는 비동기적으로 동작한다. 때문에 state가 직접 수정되어 여러번 상태를 업데이트 하는 경우 이전 업데이트 내용이 다음 업데이트 내용에 덮어 쓰여질 수 있다.
3. 클래스형 컴포넌트의 PureComponent에서 동작하지 않는다. PureComponent는 this.state과 setState를 비교해 업데이트가 필요한 경우에만 render 함수를 호출한다. 이때, state를 직접 수정하게 되면 기존의 this.state과 setState가 동일하므로 리액트는 render 함수를 호출하지 않는다.

> PureComponent: 기본 Component와의 단순한 성능적인 차이로 PureComponent는 `shouldComponentUpdate`를 통해 props와 state의 얕은 비교를 하여 불필요한 re-render를 막아준다.
> 함수형 컴포넌트일 경우에는 PureComponent의 역할을 React.memo가 하게 된다.

---

## 5. Conclusion

> 리액트에서 정말 많이 사용하는 state이다. 클래스형 컴포넌트에서는 사용하지 않아 정확한 방법은 모르지만 함수형 컴포넌트에서의 사용법은 얼추 알고 있다. 하지만 정확히 state가 어떤 개념인지에 대해 설명하라고 했을 땐 못하였다. 오늘 이후엔 짧게 나마 누구에게 설명을 할 수 있을 거 같은 느낌이 든다.  
> 그리고 PureComponent는 찾아보니까 클래스형 컴포넌트에서 사용했던 개념이었던 거 같다. 해당 개념에 대한 깊은 공부보다는 간단하게 어떤 개념인지만 알고 넘어가고 자세한 개념 공부는 React.memo로 하자.😃

---

## 참고

도서 - 소플의 처음 만난 리액트  
[[React] 4. React 컴포넌트(3) - State 알아보기(React Hooks 사용)](https://goddaehee.tistory.com/301)  
[state를 직접 변경하지 않고 setState를 사용하는 이유에 대해서 설명하세요.](https://mari-mo.tistory.com/214)  
[React - PureComponent 제대로 사용하기](https://godsenal.com/posts/React-PureComponent-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0/)

---

[👆](#state)
