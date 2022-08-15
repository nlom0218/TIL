# useMemo()

## 1. 개요

`useMemo()`는 컴포넌트의 최적화를 시키기 위해 사용되는 hook 중 하나이다. `useMemo()`에서 Memo는 Memoization을 뜻하는데 이는 기존에 수행한 연산의 결과값을 어딘가에 저장해 두고 동일한 입력이 들어오면 재활용하는 프로그래밍 기법을 말한다.

***

## 2. useMemo()를 사용하지 않는 컴포넌트의 랜더링

함수형 컴포넌트도 말 그대로 하나의 함수이다. 즉, 랜더링이 된다는 것은 함수가 호출이 되고 함수 내부에서 선언된 변수는 초기화가 된다.

아래와 같은 컴포넌트가 있다고 가정하자.

```jsx
import React from "react";

const Test = () => {
  const calculate = () => {
    // 엄청나게 복잡한 로직
    console.log("나 지금 함수 실행하고 있어!");
    return 10;
  };

  const result = calculate();

  return <div>{result}</div>;
};

export default Test;
```

위의 코드를 보면 App 컴포넌트가 랜더링이 된면 result에 `calculate()`함수가 실행되어 리턴값이 할당된다.

컴포넌트를 리랜더링시켜보자. 컴포넌트가 리랜더링은 `state`, `props`의 변화가 있을 때 한다. 여기서는 간단히 `state`를 변화시켜 리랜더링을 해보자.

아래와 같이 코드를 추가 작성한다.

```jsx
import React, { useState } from "react";

const Test = () => {
  const [render, setRender] = useState(false);

  const calculate = () => {
    // 엄청나게 복잡한 로직
    console.log("나 지금 함수 실행하고 있어!");
    return 10;
  };

  const result = calculate();

  const onClickBtn = () => setRender((prev) => !prev);

  return (
    <div>
      {result}
      <button onClick={onClickBtn}>리랜더링</button>
    </div>
  );
};

export default Test;
```

이제 리랜더링 버튼을 누를 때 마다 `state`값이 변하게 되어 컴포넌트가 리랜더링 된다. 아래의 콘솔을 보자.

![useMemo\_rerender1](../../image/React/UseMemo/useMemo\_rereder1.png)

컴포넌트가 리랜더링 될 때마다 `calculate()`함수가 다시 호출되는 것을 볼 수 있다. 눈에 보이는 `result`값은 같지만 리랜더링 될 때마다 변수가 초기화 되기 때문에 `calculate()`함수가 반복적으로 호출이 된다. 만약 `calculate()`함수가 무거운 일을 하게 된다면 비효율적인 과정을 계속해서 거치게 되는 것이다. 이를 해결할 수 있는 것이 바로 `useMemo()`이다.

***

## 3. useMemo()를 사용한 컴포넌트의 랜더링

이번에는 `useMemo()`를 사용하여 컴포넌트를 랜더링 해보자.

먼저 `useMemo()`의 기본 사용법은 아래와 같다.\
사용법 `const value = useMemo(function, deps)`

* function: Memoization을 할 값을 계산해서 리턴해 줄 콜백함수
* deps: 의존성 배열로 `useEffect()`의 deps와 같다.

`2. useMemo()를 사용하지 않는 컴포넌트의 랜더링`에서 작성한 코드 중 `const result = calculate();`를 아래와 같이 수정하자.

```jsx
const result = useMemo(() => {
  return calculate();
}, []);
```

`calculate()`함수의 return값을 Memoization을 하여 result에 할당하고 있다. 그리고 의존성 배열은 비어있기 때문에 컴포넌트가 처음 생성될 때만 `useMemo()`의 콜백함수가 실행된다.

위와 같이 수정 한 후 아무리 리랜더링 버튼을 눌러도 `calculate()`함수가 호출되지 않는다.

만약 의존성 배열에 어떠한 값이 있다면 그 값이 변할 때 마다 콜백함수를 다시 호출한다. 해당 경우를 코드와 함께 살펴보자. 아래와 같이 코드를 추가 및 수정한다.

```jsx
import React, { useState, useMemo, useEffect } from "react";

const Test = () => {
  const [render, setRender] = useState(false);
  const [num, setNum] = useState(1);

  const calculate = () => {
    // 엄청나게 복잡한 로직
    console.log("나 지금 함수 실행하고 있어!");
    return num * 20;
  };

  const result = useMemo(() => {
    return calculate();
  }, [num]);

  const onClickPlusBtn = () => setNum((prev) => prev + 1);

  const onClickBtn = () => setRender((prev) => !prev);

  useEffect(() => {
    console.log("컴포넌트가 랜더링 되었습니다.");
  }, [render, num]);

  return (
    <div>
      {result}
      <button onClick={onClickPlusBtn}>Plus</button>
      <button onClick={onClickBtn}>리랜더링</button>
    </div>
  );
};

export default Test;
```

이번에는 `useMemo()`의 의존성 배열에 `num`값을 추가 하였고 `num`값은 `state`으로 Plus 버튼을 누르 때 마다 증가한다. 즉, `useMemo()`의 콜백함수는 Plus 버튼을 눌러 `num`값이 증가하게 되면 다시 호출이 되어 새로운 `result`값을 가지게 된다. 하지만 여전히 리랜더링 버튼을 누를 때에는 콜백함수가 다시 호출 되지 않는다.

아래는 각각의 버튼을 눌렀을 때 나타나는 콘솔의 모습이다.

![useMemo\_rerender2](../../image/React/UseMemo/useMemo\_rerender2.png)

***

## 4. useMemo()를 항상 사용하여 랜더링 최적하면 좋은걸끼?

정답은 그렇지 않다. 값을 재활용하기 위해 따로 메모리를 소비하여 저장을 하기 때문에 불필요한 값까지 모두 `useMemo()`를 사용하여 Memoization을 하게 된다면 오히려 성능은 더 안 좋아질 수 있다. 그렇기 때문에 꼭 필요할 때에만 적절하게 `useMemo()`를 사용해야 한다.

***

## 5. Conclusion

> 리액트로 여러 프로젝트를 진행했지만 `useMemo()`를 사용한 적은 한 번도 없었다. 그 이유는 첫 번째 `useMemo()`에 대해 몰랐고, 두 번째 랜더링 최적화에 대한 고민을 해 본 적이 없기 때문이다. 그래서 지금까지 진행했던 그리고 진행하고 있는 리액트 프로잭트를 리팩토링 할 때 랜더링 최적화가 필요하다면 적극적으로 `useMemo()`를 사용할 계획이다. 그렇다고 무분별하게 사용은 하지 않을거다.😃

***

## 참고

[17. useMemo 를 사용하여 연산한 값 재사용하기](https://react.vlpt.us/basic/17-useMemo.html)\
[\[React\] useMemo 사용법 및 예제](https://itprogramming119.tistory.com/entry/React-useMemo-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%B0%8F-%EC%98%88%EC%A0%9C)

도움이 많이 되었던 유튜브 🎬

* React Hooks에 취한다 - useMemo 제대로 사용하기 | 리액트 훅스 시리즈\
  [![React Hooks에 취한다 - useMemo 제대로 사용하기 | 리액트 훅스 시리즈](https://img.youtube.com/vi/e-CnI8Q5RY4/0.jpg)](https://www.youtube.com/watch?v=e-CnI8Q5RY4)

***

[👆](UseMemo.md#usememo)

📅 2022-08-02
