# useContext()

## 1. 개요

`useContext()`훅에 대해 이해하기 위해서는 React에서의 `Context`가 무엇인지 알아야 한다.

`Context`에 대해 간단히 살펴보고 `useContext()`의 사용과 특징에 대해 알아보자.

---

## 2. Context

> 일반적인 React 애플리케이션에서 데이터는 위에서 아래로 (즉, 부모로부터 자식에게) props를 통해 전될되지만, 애플리케이션 안의 여러 컴포넌트들에 전해줘야 하는 props의 경우 이 과정이 번거로울 수 있습니다. context를 이용하면, 트리 단계마다 명시적으로 props를 넘겨주지 않아도 많은 컴포넌트가 이러한 값을 공유하도록 할 수 있습니다.

위는 리액트 공식문서에 나타난 `Context`에 대한 설명이다.

즉, 같은 `props`를 자식 컴포넌트에게 많이 전달하여 많은 컴포넌트가 공유하고 있단면 `Context`를 사용하여 전달하는 과정을 생략하고 부모 컴포넌트에서 한 번 전역으로 전달하여 데이터를 공유할 수 있다.

---

### 2-1. Context API - React.createContext

```js
const MyContext = React.createContext(defaultValue);
```

Context 객체를 만드는 API이다. Context 객체를 구독하고 있는 컴포넌트를 렌더링할 때 React는 트리 상위에서 가장 가까이 있는 짝이 맞는 `Provider`로부터 현재값을 읽는다.

---

### 2-2. Context API - Context.Provider

```js
<MyContext.Provider value={/* 어떤 값 */}>
```

`Context` 오브젝트에 포함된 React 컴포넌트인 `Provider`는 context를 구독하는 컴포넌트들에게 `context`의 변화를 알리는 역할을 한다.

`Provider` 컴포넌트는 `value props`을 받아서 이 값을 하위에 있는 컴포넌트에게 전달한다. 값을 전달받을 수 있는 컴포넌트의 수에 제한은 없다. 만약 `Provider` 컴포넌트 하위에 또 다른 `Provider` 컴포넌트가 있는 경우 하위 `Provider`의 값이 우선시 된다.

`Provider` 컴포넌트의 `value props`가 바뀔 때 마다 해당 `Provider`컴포넌트를 구독하고 있는 컴포넌트는 리렌더링 된다.

---

### 2-3. Context API - Context.Consumer

```js
<MyContext.Consumer>
    {value => /* context 값을 이용한 렌더링 */}
</MyContext.Consumer>
```

`context` 변화를 구독하는 React 컴포넌트이다. 이 컴포넌트를 사용하면 함수 컴포넌트안에서 context를 구독할 수 있다.

`Context.Consumer` 컴포넌트의 자식은 함수여야 한다. 이 함수는 `context`의 현재값을 받고 React노드를 반환한다. 이 함수가 받는 `value` 매개변수 값은 해당 `context`의 `Provider` 컴포넌트 중 상위 트리에서 가장 가까운 `Provider` 컴포넌트의 `value props`과 동일하다.

---

## 3. Context를 사용한 제주도 기념품 컴포넌트

제주도 여행 기념품 목록을 만드는 컴포넌트를 `Context`를 활용하여 새롭게 작성해보자.

```js
//App.js
import { useState, createContext } from "react";
import Children from "./Children";


// 1) createContext를 사용하여 Context 객체 만들기
export const AppContext = createContext({
  gift: ["한라봉", "초콜릿"],
  addGift: () => {},
});

function App() {
  const [gift, setGift] = useState(["한라봉", "초콜릿"]);

  const addGift = (gift) => {
    setGift((prev) => [...prev, gift]);
  };
  return (
    // 2) AppContext.Provider 생성하여 Context의 변화 알리기
    <AppContext.Provider value={{ gift, addGift }}>
      <div>안녕, 제주!</div>
      <Children />
    </AppContext.Provider>
  );
}

export default App;

// Children.js
import React from "react";
import { useForm } from "react-hook-form";
import { AppContext } from "./App";

const Children = () => {
  const { register, handleSubmit, setValue, getValues } = useForm();

  return (
    // 3) AppContext.Consumer를 통해 Context를 구독하기
    <AppContext.Consumer>
      {({ gift, addGift }) => (
        <div>
          <form
            onSubmit={handleSubmit(() => {
              console.log(`${gift}가 기념품 목록에 추가됩니다😀`);
              addGift(getValues("gift"));
              setValue("gift", "");
            })}
          >
            <input {...register("gift")} placeholder="사고 싶은 기념품" />
            <input type="submit" value="기념품 고르기" />
          </form>
          <div>나의 기념품 목록: {gift.join(", ")}</div>
        </div>
      )}
    </AppContext.Consumer>
  );
};

export default Children;
```

![Context result](/image/React/UseContext/context_result.png)

---

### 1) createContext를 사용하여 Context 객체 만들기

```js
export const AppContext = createContext({
  gift: ["한라봉", "초콜릿"],
  addGift: () => {},
});
```

`createcontext API`를 사용하여 Context 객체를 만들다. `defaultValue`로는 하위 컴포넌트가 받고 있는 매개변수(`Provider API`의 `value`) 모양과 동일하게 만들었다.

---

### 2) AppContext.Provider 생성하여 Context의 변화 알리기

```js
<AppContext.Provider value={{ gift, addGift }}>
  <div>안녕, 제주!</div>
  <Children />
</AppContext.Provider>
```

`gift`는 App컴포넌트에서 만든 `state`값이고 `addGift`함수는 `gift`의 값을 변화시키기 위해 사용되는 함수이다. 이를 `Proivder API`의 `value`를 통해 자식 컴포넌트가 해당 값들을 구독할 수 있도록 한다.

> 처음에 오류가 났던 부분이다. `value props`의 값이 Object이면 `AppContext.Consumer`컴포넌트에서 Object로 받고 Array이면 `AppContext.Consumer`컴포넌트에서 Array로 받자

---

### 3) AppContext.Consumer를 통해 Context를 구독하기

```js
<AppContext.Consumer>
    {({ gift, addGift }) => ()}
</AppContext.Consumer>
```

`consumer API`를 통해 `AppContext.Consumer`컴포넌트를 만들었다. 해당 컴포넌트의 자식은 함수로 인자로 `AppContext.Consumer`컴포넌트에서 전달한 `value props`이 들어있다. 이를 가지고 화면을 그리면 된다.

> 확실히 `props`가 필요없는 컴포넌트들에게 까지 `props`를 전달하는 것 보다 `Context`를 사용하여 딱 필요한 자식 컴포넌트에게만 전달하는 것이 깔끔해 보인다. 이제 아래에서 `Context`를 더 편하게 사용할 수 있게 해주는 `useContext()`훅에 대해 살펴보자.

---

## 4. useContext()훅 사용하기

`useContext()`의 기본 사용법은 아래와 같다.

```js
const value = useContext(MyContext);
```

`context` 객체(`React.createContext`에서 반환된 값)을 받아 그 `context`의 현재 값을 반환한다. `context`의 현재 값은 트리 안에서 이 Hook를 호출하는 컴포넌트 가장 가까이에 있는 `<MyContext.Provider>`의 `value props`에 의해 결정된다.

`useContext()`훅을 호출한 컴포넌트는 `context`값이 변경되면 항상 리렌더링 된다.

이제 `useContext()`훅을 사용하여 위의 `Children.js` 컴포넌트를 아래와 같이 수정하자.

```js
// Children.js
import React, { useContext } from "react";
import { useForm } from "react-hook-form";
import { AppContext } from "./App";

const Children = () => {
  const { gift, addGift } = useContext(AppContext);
  const { register, handleSubmit, setValue, getValues } = useForm();

  return (
    <div>
      <form
        onSubmit={handleSubmit(() => {
          console.log(`${getValues("gift")}가 기념품 목록에 추가됩니다😀`);
          addGift(getValues("gift"));
          setValue("gift", "");
        })}
      >
        <input {...register("gift")} placeholder="사고 싶은 기념품" />
        <input type="submit" value="기념품 고르기" />
      </form>
      <div>나의 기념품 목록: {gift.join(", ")}</div>
    </div>
  );
};

export default Children;
```

` <AppContext.Consumer>`컴포넌트가 없어진 것을 확인할 수 있다. 대신에 `useContext()`훅을 통해 `gift`와 `addGift`를 불러와 `AppContext`의 값을 사용하고 있다.

`useContext()`훅을 사용함으로써 이전 보다 간결한 코드 작성이 가능해졌다.

---

## 5. Conclusion

> `useContext()`훅도 `useReducer()`훅과 마찬가지로 한 번도 사용해본 적이 없는 React Hook이었다. 그래서 처음에는 생소하였지만 생각보다 그리? 어렵고 복잡한 개념이 아니었기 때문에 금방 이해가 가능했다.  
> 궁금한 점은 `useContext()`훅과 `useReducer()`훅이 `porps`를 필요하지 않는 자식 컴포넌트를 건너 뛰고 필요한 자식 컴포넌트에게만 `props`를 전달을 가능하게 하는 훅으로 보인다. 그렇다면 이 두가지의 훅을 모두 사용하는 것이 좋은지 아니면 상황에 따라 한 가지를 선택하여 사용하는 것이 좋은지가 궁금하다.

마무리로 `useContext()`훅과 `useReducer()`훅을 모두 사용하여 위의 컴포넌트를 작성하였다. 확실히 컴포넌트의 구조가 복잡해진다면 이 훅들은 유용해질 것 같다😃 그리고 같은 내용이지만 코드에 사용된 훅이 다르다는 것도 재밌는 부분이다.(사실 이건 그냥 useState만 사용하는 것이 좋은거 같다😂)

```js
// App.js
import { createContext, useReducer } from "react";
import Children from "./Children";

const inItGift = ["한라봉", "초콜릿"];

const reducer = (gift, action) => {
  switch (action.type) {
    case "submitGift":
      return [...gift, action.gift];
    default:
      throw new Error("type Error");
  }
};

export const AppContext = createContext({
  inItGift,
  dispatch: () => {},
});

function App() {
  const [gift, dispatch] = useReducer(reducer, inItGift);
  return (
    <AppContext.Provider value={{ gift, dispatch }}>
      <div>안녕, 제주!</div>
      <Children />
    </AppContext.Provider>
  );
}

export default App;

// Children.js
import React, { useContext } from "react";
import { useForm } from "react-hook-form";
import { AppContext } from "./App";

const Children = () => {
  const { gift, dispatch } = useContext(AppContext);
  const { register, handleSubmit, setValue } = useForm();

  const onSubmitFrom = (data) => {
    const { gift } = data;
    console.log(`${gift}가 기념품 목록에 추가됩니다😀`);
    dispatch({
      type: "submitGift",
      gift,
    });
    setValue("gift", "");
  };

  return (
    <div>
      <form onSubmit={handleSubmit(onSubmitFrom)}>
        <input {...register("gift")} placeholder="사고 싶은 기념품" />
        <input type="submit" value="기념품 고르기" />
      </form>
      <div>나의 기념품 목록: {gift.join(", ")}</div>
    </div>
  );
};

export default Children;

```

---

## 참고

[[TIL #6] React (Hooks) - useContext 란?](https://velog.io/@jminkyoung/TIL-6-React-Hooks-useContext-%EB%9E%80)
[Context](https://ko.reactjs.org/docs/context.html)

---

[👆](#usecontext)

📅 2022-08-10
