# Conditional Rendering

## 1. 개요

조건부 렌더링은 값의 결과가 `true`인지 `false`인지 또는 특정한 값인지 따라 다른 결과물을 렌더링 하는 것을 의미한다.

---

## 2. if문을 통한 조건부 렌더링

아래와 같은 `App.js`컴포넌트, `Hello.js`컴포넌트가 있다.

```js
// App.js
import { useState } from "react";
import Hello from "./Hello";

function App() {
  const [isLogin, setIsLogin] = useState(false);

  const onClickBtn = () => setIsLogin((prev) => !prev);

  return (
    <div>
      <Hello isUser={isLogin} />
      <button onClick={onClickBtn}>상태 바꾸기</button>
      <span>isLogin값: {isLogin ? "true" : "false"}</span>
    </div>
  );
}

export default App;

// Hello.js
import React from "react";

const Hello = ({ isLogin }) => {
  if (isLogin) {
    return <div>반가워요🖐</div>;
  } else {
    return <div>로그인이 필요해요😿</div>;
  }
};

export default Hello;
```

`isLogin`의 값에 따라 `Hello.js`컴포넌트에서 `if문` 통해 다른 JSX를 반환하고 있다.

아래는 `isLogin`의 값에 따라 달라지는 렌더링의 결과 사진인다.

![ConditionalRendering Result1](/image/React/ConditionalRendering/conditional_rendering_result1.png)

![ConditionalRendering Result2](/image/React/ConditionalRendering/conditional_rendering_result2.png)

---

## 3. 삼항 연산자를 통한 조건부 렌더링

### 3-1. 삼항 연산자란?

먼저 삼항 연산자에 대해 살펴보자.

조건부 삼항 연산자는 JavaScript에서 세 개의 피연산자를 취할 수 있는 유일한 연산자이다. 맨 앞에 조건문 들어가고. 그 뒤로 물음표(?)와 조건이 참이라면 실행할 식이 물음표 뒤로 들어간다. 그리고 바로 뒤로 콜론(:)이 들어가며 조건이 거짓이라면 실행할 식이 마지막에 들어간다. 보통 if 명령문의 단축 형태로 쓰인다.

> 기본구문: condition ? exprIfTrue : exprIfFalse

- condition: 조건문으로 들어갈 표현식
- exprIfTrue: 참일 때 실행할 식
- exprIfFalse: 거짓일 때 실행할 식

만약 **조건문이 참일 경우만** 나타내기 위해서는 어떻게 해야 할까? 해당 경우는 아래와 같이 작성한다.

> condition && exprIfTrue

**연속된 조건문**은 아래와 같이 작성한다.

> condition1 ? value1 : condition2 ? value2 : value3;

---

### 3-2. 조건부 렌더링

이제 위의 `Hello.js`컴포넌트를 삼항연산자를 사용하여 코드를 수정해보자.

```js
// Hello.js
import React from "react";

const Hello = ({ isLogin }) => {
  return <div>{isLogin ? "반가워요🖐" : "로그인이 필요해요😿"}</div>;
};

export default Hello;
```

렌더링 결과는 위와 같다.

---

## 4. Conclusion

> 컴포넌트를 렌더링을 할 때 조건에 따라 값을 달리 경우가 생각보다 많다. 나는 대부분을 간단한 값을 다룰 때에는 삼항 연산자를 사용을 하지만 긴 JSX를 묶어야 하는 경우가 있을 땐 if문을 사용하는 편이다. 예를 들어 아래와 같은 경우다.
>
> ```js
> if (loading) {
>    return <Loading />;
> }
> if (!loading) {
>   return (
>   <div>Hello!!</div>
>   <div>Hello!!</div>
>   <div>Hello!!</div>
>   <div>Hello!!</div>
>   // 긴 코드가 이어짐
>   )
> }
> ```
>
> `loading`은 DB의 데이터를 불러오고 있는 중인지, 불러왔는지를 알려주는 값이고 이에 따라 `<Loading />`컴포넌트를 렌더링 할지 아니면 아래의 JSX를 렌더링할지를 결정하는 편이다. 물론 위도 삼항 연산자로 충분히 바꾸어 사용할 수 있다. 하지만 개인적으로는 코드가 길어지면 위와 같은 코드가 가독성이 좋아보인다:)

---

## 참고

[6. 조건부 렌더링](https://react.vlpt.us/basic/06-conditional-rendering.html)  
[삼항 조건 연산자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)
