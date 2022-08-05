# useEffect()

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

## 3. 마운트

컴포넌트가 처음 나타났을 때(마운트 되었을 때)를 `useEffect()`로 관리를 해보자.

deps 배열을 비우게 된다면, 컴포넌트가 처음 나타날때에만 `useEffect()` 에 등록한 함수가 호출됩니다.

아래의 코드를 예시로 살펴보자.

```js
import React, { useEffect, useState } from "react";

const Test = () => {
  const [numberArr, setNumberArr] = useState([]);
  const onClickBtn = () => {
    const num = Math.floor(Math.random() * 100);
    const newNumArr = [...numberArr, num];
    setNumberArr(newNumArr);
  };
  return (
    <div>
      <button onClick={onClickBtn}>숫자 생성</button>
      {numberArr.map((item, index) => {
        return <Number key={index} number={item} />;
      })}
    </div>
  );
};

export default Test;

const Number = ({ number }) => {
  useEffect(() => {
    console.log(`컴포넌트 마운트 ${number}`);
  }, []);
  return <div>Number: {number}</div>;
};
```

위의 컴포넌트를 살펴보자. Test컴포넌트에서 **숫자 생성** 버튼을 누르면 새로운 숫자이 생기고 `numberArr`에 새로운 숫자가 담기게 된다. 그리고 `numberArr`에 따라 `Number` 컴포넌트가 생기게 된다.

Number컴포넌트에서는 컴포넌트가 생성(마운트)될 때 `useEffect()`로 인해 콘솔이 찍히게 된다. 아래는 숫자가 생성될 때 보여지는 화면과 콘솔이다.

![useEffect mount](/image/React/UseEffect/useEffectMount1.png)
![useEffect mount2](/image/React/UseEffect/useEffectMount2.png)

---

## 4. 언마운트

이번에는 컴포넌트가 사라질 때(언마운트 되었을 때)를 `useEffect()`로 관리를 해보자.

`useEffect()`에서 함수를 반환할 수 있는데 이 함수를 `cleanup`함수라고 불린다. 이 `cleanup`함수가 컴포넌트가 사라질 때 호출된다. 쉽게 말해 `cleanup`함수가 `useEffect()`에 대한 뒷정리를 해준다고 할 수 있다.

`cleanup`함수를 사용해보자. 그러기 위해서는 위의 Test컴포넌트에 추가적인 코드(Test컴포넌트를 삭제하기 위한 코드, `cleanup`함수 코드)가 필요하다. 아래와 같이 작성해보자.

```js
import React, { useEffect, useState } from "react";

const Test = () => {
  const [numberArr, setNumberArr] = useState([]);
  const onClickBtn = () => {
    const num = Math.floor(Math.random() * 100);
    const newNumArr = [...numberArr, num];
    setNumberArr(newNumArr);
  };
  return (
    <div>
      <button onClick={onClickBtn}>숫자 생성</button>
      {numberArr.map((item, index) => {
        return (
          <Number
            key={index}
            number={item}
            numberArr={numberArr}
            setNumberArr={setNumberArr}
          />
        );
      })}
    </div>
  );
};

export default Test;

const Number = ({ number, numberArr, setNumberArr }) => {
  // Number컴포넌트를 언마운트하기 위한 함수
  // setNumberArr를 통해 numberArr의 값을 바꾼다.
  const onClickDelBtn = () => {
    const newNumArr = numberArr.filter((item) => item !== number);
    setNumberArr(newNumArr);
  };
  useEffect(() => {
    console.log(`컴포넌트 마운트 ${number}`);

    // cleanup 함수
    return () => {
      console.log(`컴포넌트 언마운드 ${number}`);
    };
  }, []);
  return (
    <div>
      <span>Number: {number}</span>
      <button onClick={onClickDelBtn}>숫자 지우기</button>
    </div>
  );
};
```

![useEffect unMount](/image/React/UseEffect/useEffectUnMount1.png)
![useEffect unMount](/image/React/UseEffect/useEffectUnMount2.png)

---

## 5. deps 에 특정 값 넣기

deps에 특정 값을 넣게 된다면 컴포넌트가 처음 마운트 될 때 `useEffect()`가 호출되고 deps에 넣은 특정 값이 바뀔 때에도 `useEffect()`가 호출된다. 아래의 코드를 보면서 deps에 특정값이 있을 때와 없을 때의 `useEffect()`를 비교해보지.

```js
import React, { useEffect, useState } from "react";

const Test = () => {
  const [number, setNumber] = useState(1);

  const onClickChangeBtn = () => {
    const num = Math.floor(Math.random() * 100);
    setNumber(num);
  };

  // 컴포넌트가 마운트 될 때만 실행
  useEffect(() => {
    console.log("컴포넌트 마운트");
  }, []);

  // 컴포넌트가 마운트 또는 업데이트 될 때 실생
  useEffect(() => {
    console.log(`컴포넌트 마운트 또는 컴포넌트 업데이트 ${number}`);
  }, [number]);
  return (
    <div>
      <span>Number: {number}</span>
      <button onClick={onClickChangeBtn}>숫자 바꾸기</button>
    </div>
  );
};

export default Test;
```

위의 코드에서 `useEffect()`가 두 번 보인다. 보이는 순서대로 `1번 useEffect()`, `2번 useEffect()`라고 부르겠다.

`1번 useEffect()`의 deps은 특정 값이 없은 빈 배열이다. 그렇기 때문에 처음 컴포넌트가 생성될 때만 호출이 된다.  
`2번 useEffect()`의 deps에는 `number`값이 들어가있다. `number`의 값은 **숫자 바꾸기 버튼**을 누를 때 마다 바뀌기 때문에 `2번 useEffect()`는 **숫자 바꾸기 버튼**을 눌러 `number`값이 바뀔 때 때 마다 호출이 된다.

아래는 **숫자 바꾸기 버튼**을 눌를 때 마다 호출되는 `useEffect()`의 결과 사진이다.

![useEffect() update](/image/React/UseEffect/useEffectUpdate.png)

---

## 6. deps 파라미터를 생략하기

`useEffect()`의 deps의 자체를 생략하게 된다면 컴포넌트가 마운트 될 때 뿐만 아니라 리랜더링 될 때마다 `useEffect()`가 호출된다.

```js
import React, { useEffect, useState } from "react";

const Test = () => {
  const [number, setNumber] = useState(1);

  const onClickChangeBtn = () => {
    const num = Math.floor(Math.random() * 100);
    setNumber(num);
  };

  useEffect(() => {
    console.log("컴포넌트 마운트 또는 리랜더링");
  });

  return (
    <div>
      <span>Number: {number}</span>
      <button onClick={onClickChangeBtn}>숫자 바꾸기</button>
    </div>
  );
};

export default Test;
```

위의 코드에서 **숫자 바꾸기 버튼**을 누를 때 마다 `number`값이 바꾸기 때문에 컴포넌트가 계속 리랜더링 된다. 그리고 `useEffect()`를 보면 deps가 완전히 생략되어 있기 때문에 컴포넌트가 리랜더링 될 때 마다 `useEffect()`가 호출된다. 다시 말해 흐름을 정리하면 아래와 같다.

1. 숫자 바꾸기 버튼을 누른다.
2. `number`값이 바뀐다.
3. 컴포넌트가 리랜더링 된다.
4. `useEffect()`가 호출된다.

아래는 **숫자 바꾸기 버튼**를 누를 때 마다 `useEffect()`가 호출되어 계속 찍히는 콘솔의 사진이다.

![useEffect not deps](/image/React/UseEffect/useEffectRerender.png)

---

## 7. Conclusion

> `useEffect()`를 자주 사용했었다. 하지만 lifecycle과는 연관을 지어 생각을 하지 않았다. 이번 공부를 통해 lifecycle과 연관을 지어 이해하도록 노력을 했고, cleanup함수에 대해서도 막연하게 알고 있었는데 어떨 때 사용하지는 알 수 있는 공부였다.

---

## 참고

[[React] 리액트 Hooks : useEffect() 함수 사용법](https://cocoon1787.tistory.com/796)  
[16. useEffect를 사용하여 마운트/언마운트/업데이트시 할 작업 설정하기](https://react.vlpt.us/basic/16-useEffect.html)

---

[👆](#useeffect)
