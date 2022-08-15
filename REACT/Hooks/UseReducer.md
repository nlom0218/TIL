# useReducer()

## 1. 개요

리액트에서는 보통 상태를 업데이트 하기 위해 `useState()`훅을 사용한다. 하지만 `useReducer()`훅을 사용하여 상태를 관리할 수도 있다.

`useReducer()`훅이 가지고 있는 특별한 특징은 한 컴포넌트 내에서 `state`를 업데이트하는 로직 부분을 그 컴포넌트로부터 분리시키는 것이 가능하다는 것이다. 상태 업데이트 로직을 컴포넌트 바깥에 작성 할 수도 있고, 심지어 다른 파일에 작성 후 불러와서 사용 할 수도 있다.

이러한 `useReducer()`훅에 대해 `useState()`훅과 비교하는 것을 시작하여 살펴보자.

***

## 2. useState() vs useReducer()

언제 `useState()`훅을 사용하는지 언제 `useReducer()`훅을 사용하는지는 정해진 기준은 없지만 보통 아래의 상황에 따라 나뉘어 `state`를 관리한다.

*   `useState()`훅을 사용할 경우

    1. 관리해야 할 `state`가 1개일 경우
    2. 그 `state`가 단순한 숫자, 문자열 또는 `boolean`값일 경우

    ***
* `useReducer()`훅을 사용할 경우
  1. 관리해야 할 `state`가 1개 이상, 복수일 경우
  2. 혹은 현재는 단일 `state`값만 관리하지만, 추후 유동적일 가능성이 있는 경우
  3. 스케일이 큰 프로젝트의 경우
  4. `state`의 구조가 복잡해질 것으로 보이는 경우

***

## 3. useReducer()훅의 기본 구조

아래는 useReducer()훅의 기본 구조이다.

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

* state: 컴포넌트에서 사용할 상태
* dispatch: `reducer` 함수를 실행시킨다. 컴포넌트 내에서 state의 업데이트를 일으키기 위해 사용한다.
* reducer: `(state, action) =-> newState`의 형태를 가진다. 컴포넌트 외부에서 `state`를 업데이트하는 로직을 담당하는 함수이다.
* initalArg: 초기 `state`
* init: 초기 함수

***

## 4. initialArg를 통한 초기 state값 설정

`initialArg`를 통해 초기 `state`값을 설정할 수 있다. 그러면 `useCallback()`챕터에서 다루었던 제주도 여행 컴포넌트 바탕으로 새롭게 리액트 컴포넌트를 만들어보자.

```jsx
import { useState, useEffect } from "react";
import { useForm } from "react-hook-form";

const reducer = () => {};

const initialArg = {
  isSunny: true,
  gift: [],
};

function App() {
  const [state, dispatch] = useReducer(reducer, initialArg);
  return <div></div>;
}

export default App;
```

위와 같이 우선 `initialArg`를 만들고 `App`컴포넌트에서는 `useReducer()`훅을 불러와 기본 코드를 작성하자. `reudecer()`함수는 아래에서 다룬다.

먼저 궁금한 점이 생겼다. `useReducer()`훅을 통해 불러온 `state`에는 어떤 값이 저장되어 있을까? 콘솔을 찍어 그 내용을 확인하자.

![useReducer state console](../../image/React/UseReducer/useReducer\_state\_console.png)

`initialArg`에서 선언한 객체가 그대로 찍히는 것을 볼 수 있다.

***

## 5. state값을 가져와 화면에 그리기

이번엔 `initialArg`객체의 `gift`배열에 "한라봉"과 "초콜릿"을 추가한 후 `state`값을 가져와 화면에 출력하자. 아래의 코드 처럼 코드를 추가 작성해보자.

```jsx
import { useState, useReducer } from "react";
import { useForm } from "react-hook-form";

const reducer = () => {};

const initialArg = {
  isSunny: true,

  // gift의 초기 값 변경
  gift: ["한라봉", "초콜릿"],
};

function App() {
  const [state, dispatch] = useReducer(reducer, initialArg);

  // 구조분해 할당으로 state안의 isSunny와 gift를 가져왔다
  const { isSunny, gift } = state;
  const { register, handleSubmit, setValue } = useForm();

  return (
    <div>
      <div>
        <span>기념품을 가지고 집으로 돌아갈 수 있을까요?</span>
        <button>날씨 바꾸기</button>
        <div>
          {isSunny
            ? "날씨가 매우 좋네요! 비행기가 이륙할 수 있어요 🛫"
            : "태풍이 왔네요ㅠㅠ 내일까지 기다려봐요 ⛈"}
        </div>
      </div>
      <form>
        <input {...register("gift")} placeholder="사고 싶은 기념품" />
        <input type="submit" value="기념품 고르기" />
      </form>
      <div>나의 기념품 목록: {gift.join(", ")}</div>
    </div>
  );
}

export default App;
```

`state`를 구조분해 할당을 하여 `isSunny`와 `gift`을 가져왔다. 그리고 해당 값을 바탕으로 화면을 그리고 있다.

> `useCallback()`챕터에서 다루었던 제주도 여행과 관련된 컴포넌트다. 변수의 이름은 변하지 않았으니 그대로 복붙하고 사용하지 않는 함수가 지우면 된다.

***

## 6. state를 업데이트하기 위한 첫 번째 과정인 dispatch함수 작성

`dispatch`함수는 `state`의 업데이트를 일으키기 위해 사용되며 `dispatch`함수의 인자로써 업데이트를 위한 정보를 가진 `aciton`를 이용한다. 또한 `dispatch`함수는 바로 아래에서 다루게 되는 `reducer`함수를 실행시킨다.

이벤트를 연결하여 `dispatch`함수를 작성해보자.

```jsx
import { useState, useReducer } from "react";
import { useForm } from "react-hook-form";

const reducer = (state, action) => {
  console.log(state, action);
  return state;
};

const initialArg = {
  isSunny: true,
  gift: ["한라봉", "초콜릿"],
};

function App() {
  const [state, dispatch] = useReducer(reducer, initialArg);
  const { isSunny, gift } = state;
  const { register, handleSubmit, setValue } = useForm();

  // 이벤트 발생 시 실행 될 함수
  const onClickChangeBtn = () => dispatch({ type: "changeWeather" });
  const onSubmitFrom = (data) => {
    const { gift } = data;
    dispatch({
      type: "submitGift",
      gift,
    });
    setValue("gift", "");
  };

  return (
    <div>
      <div>
        <span>기념품을 가지고 집으로 돌아갈 수 있을까요?</span>
        <button onClick={onClickChangeBtn}>날씨 바꾸기</button>
        <div>
          {isSunny
            ? "날씨가 매우 좋네요! 비행기가 이륙할 수 있어요 🛫"
            : "태풍이 왔네요ㅠㅠ 내일까지 기다려봐요 ⛈"}
        </div>
      </div>
      <form onSubmit={handleSubmit(onSubmitFrom)}>
        <input {...register("gift")} placeholder="사고 싶은 기념품" />
        <input type="submit" value="기념품 고르기" />
      </form>
      <div>나의 기념품 목록: {gift.join(", ")}</div>
    </div>
  );
}

export default App;
```

`button`태그와 `form`태그에 각각 `onClick`과 `onSubmit`이벤트를 연결하였고 `onClickChangeBtn()`함수와 `onSubmitFrom()`함수를 통해 이벤트가 실행된다. 하나하나 살펴보자.

* onClickChangeBtn(): `dispatch`함수가 실행되고 인자로 객체`{type: "changeWeather"}`가 전달되었다.
* onSubmitFrom(): 여기서는 `dispatch`함수의 인자로 객체`{ type: "submitGift", gift }`가 전달 되었다.

***

## 7. state를 업데이트하기 위한 두 번째 과정인 reducer함수 작성

`dispatch`함수의 실행으로 `reducer`함수가 호출된다. `reducer`함수는 두 개의 인자를 받는데 첫 번째는 현재(바뀌기 전)의 `state`이고 두 번째는 `action`이다. 즉 아래와 같은 기본 형태를 가지고 있다.

```jsx
const reducer = (state, action) => {};
```

`reudcer`함수를 아래와 같이 수정하여 전달 받은 `state`와 `action`를 콘솔로 찍어보자

```jsx
const reducer = (state, action) => {
  console.log(state, aciton);
};
```

![useReducer\_reducer\_error](../../image/React/UseReducer/useReducer\_reducer\_error.png)

`날씨 바꾸기`버튼을 눌렀더니 위와 같은 에러가 떴다. 다행히 우리가 원하는 `state`와 `action`은 콘솔로 잘 찍혔다. 에러를 파해치기 전 해당 값을 살펴보자.

* state: 현재의 `state`
* action: `dispatch`함수에 인자로 전달한 객체

에러는 도대체 왜? 발생한 것일까? 그 이유는 간단하다. `useReducer()`훅의 `reducer`함수는 `state` 반환해야 한다. 당연히 인자로 받은 `state`를 그대로 반환하는 것이 아니라(물론 가능은 하지만 그렇게 되면 `state`값을 관리하기 위해 사용하는 `useReducer()`훅의 의미가 없어진다.) 어떠한 로직을 통해 바뀌는 `state`값을 반환해야 한다.

위의 코드에서는 반환되는 `state`가 없기 때문에 에러가 발생한 것이다. 임시방편으로 이를 해결하기 위해서는 아래와 같이 코드를 작성하면 된다.

```jsx
const reducer = (state, action) => {
  console.log(state, aciton);
  return state;
};
```

`reducer`함수의 인자로 받는 `state`와 `action`에 대해 알아봤으니 이를 가지고 `state`를 업데이트 하는 코드를 작성하자.

`reducer`함수를 아래와 같이 추가 작성한다.

```jsx
const reducer = (state, action) => {
  const { isSunny, gift } = state;
  switch (action.type) {
    case "changeWeather":
      return {
        ...state,
        isSunny: !isSunny,
      };
    case "submitGift":
      return {
        ...state,
        gift: [...gift, action.gift],
      };
    default:
      throw new Error("type Error");
  }
};
```

1. 구조분해 할당으로 `state`값의 내부에 존재하는 `isSuuny`와 `gift`을 가져온다.
2. `action`객체에는 `type`이 존재하며 이는 이벤트에 따라 달라진다. `onSubmitForm`이벤트인 경우 `type`뿐 아니라 선물의 이름을 값으로 가지는 `gift`도 존재한다.
3. `switch`문으로 사용하여 `action.type`과 일치하는 case를 찾아 변경된 `state`를 반환한다.
4. 각각의 `case`에서 리턴되는 객체를 보면 기존 `state`를 spread 연산자를 사용하였다. 이는 `reducer`함수에서 새로운 상태를 만들 때에는 불변성을 지켜주어야 하기 때문이다.
5. 추가로 `default`를 설정하여 `case`에 존재하지 않는 `aciton.type`이 들어왔을 때 에러를 발생시키고 있다.

`reducer`함수에서 주의해야 할 점은 기존의 `state`를 새로운 `state`로 대체(replace)를 해야 한다는 것이다. 즉, 기존의 `state`를 변경(modify)하거나, 추가(add)하거나, 덮어쓰지(overwrite) 않아야 한다.

***

## 8. Conclusion

> 사실 위의 예제는 `useReducer()`휵울 사용하는 것 보다 `useState()`훅을 사용하는 것이 적절하다. 하지만 간단한 예제를 보여주기 위해 사용한 점을 참고해주었으면 한다.\
> `useReducer()`훅은 여러 리액트 프로젝트를 진행하면서 사용하지 않았던 훅이다. 하지만 예전에 `redux`를 배울 때 비슷한 개념에 대해 공부했던 기억이 떠올라 쉽게 이해가 되었던 거 같다. 나중에 `redux`를 다시 공부할 때 오늘 배운 내용이 큰 도움이 되었음 한다.

***

## 참고

[20. useReducer 를 사용하여 상태 업데이트 로직 분리하기](https://react.vlpt.us/basic/20-useReducer.html)\
[React Hooks :: useReducer에 대해 알아보기](https://velog.io/@iamhayoung/React-Hooks-useReducer%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)

***

[👆](UseReducer.md#usereducer)

📅 2022-08-09
