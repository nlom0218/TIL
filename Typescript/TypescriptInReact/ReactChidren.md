# React children with TypeScript

## 0. 빠른 요약

`React.ReactNode` 또는 `React.PropsWithChildren<T>`를 사용하자.

```typescript
interface IProps {
  children: React.ReactChild;
}
```

또는

```typescript
const Box = ({ children }: React.PropsWithChildren<IProps>) => {
  return (
    <div>
      <div>Box 부분입니다.</div>
      {children}
    </div>
  );
};
```

---

## 1. 개요

React의 props중 `children`은 React의 강력한 합성 모델을 구현하기 위해 사용된다. 여기서 합성이라는 것은
컴포넌트에서 다른 컴포넌트를 담는 다는 것을 의미한다. 간단한 예시만 보고 넘어간다.

```javascript
const Box = ({ children }) => {
  return (
    <div>
      <div>Box</div>
      {chldren}
    </div>
  );
};

const Item = () => {
  return (
    <Box>
      <div>Item</div>
    </Box>
  );
};
```

`Item` 컴포넌트는 `Box` 컴포넌트의 모양을 그대로 담으면서 `Item` 컴포넌트만의 JSX를 반환하고 있다. `React children`에 대한
추가적인 자세한 설명은 `React` 페이지에서 나중에 다루도록 하고 여기서는 `React children`의 타입을 정하는 방법에 대해 정리한다.

`chlidren` props은 아래와 같은 내용을 담을지에 따라 타입을 다르게 설정해줘야 한다.

- React Element
- primitive(ex, 문자열)
- 배열

---

## 2. JSX.Element

가장 제한적인 유형이다. 단일 `Element`을 담을 때만 사용 가능하다.

아래의 예시를 살펴보자.

```typescript
// Box.tsx
interface IProps {
  children: JSX.Element;
}

const Box = ({ children }: IProps) => {
  return (
    <div>
      <div>Box 부분입니다.</div>
      {children}
    </div>
  );
};

export default Box;

// App.tsx
import Box from "./Box";

function App() {
  return (
    <Box>
      <div>App 부분입니다.</div>
    </Box>
  );
}

export default App;
```

`App` 컴포넌트는 `Box` 컴포넌트에 감싸져 있다. 즉, `App` 컴포넌트가 `Box` 컴포넌트에 담아져 있다. 그리고 담긴 `App` 컴포넌트의 내용은
아래와 같다.

```html
<div>App 부분입니다.</div>
```

이렇게 단일 `Element`를 `children`으로 담을 땐 아래와 같이 `children`를 `JSX.Element` 타입으로 지정한다.

```typescript
interface IProps {
  children: JSX.Element;
}
```

---

### 2-1. 오류 1) 여러 개의 Element

- 오류 코드

  ```typescript
  import Box from "./Box";

  function App() {
    return (
      <Box>
        <div>App 부분입니다.</div>
        <div>App 부분입니다.</div>
      </Box>
    );
  }
  export default App;
  ```

하지만 단일 `Element`가 아니라 여러 개의 `Element`가 있으면 어떻게 될까? 그렇게 되면 아래의 사진과 같은 오류를 볼 수 있다.

![JSX.Element error 1](/image/Typescript/ReactChildren/react_children_jsx_element1.png)

하나의 자식이 아닌 여러 개의 자식이 `children`으로 제공되었다는 내용이다. 이는 여러 개의 `Element`를 하나의 태그로 묶어주면 해결이 가능한다.(아래 코드 참고)

```typescript
import Box from "./Box";

function App() {
  return (
    <Box>
      <div>
        <div>App 부분입니다.</div>
        <div>App 부분입니다.</div>
      </div>
    </Box>
  );
}

export default App;
```

---

### 2-2. 오류 2) string 타입의 Text

- 오류 코드

  ```typescript
  import Box from "./Box";

  function App() {
    return <Box>App 부분입니다.</Box>;
  }

  export default App;
  ```

이번엔 `Element` 타입의 `children`이 아니라 `string` 타입의 `children`이라면 어떻게 될까. 아래와 같은 오류를 볼 수 있다.

![JSX.Element error 2](/image/Typescript/ReactChildren/react_children_jsx_element2.png)

---

## 3. React.ReactChild

`React.ReactChild` 타입을 사용하게 되면 문자열 같은 `primitive`도 `children`으로 허용이된다.

```typescript
// Box.tsx
import React from "react";

interface IProps {
  children: React.ReactChild;
}

const Box = ({ children }: IProps) => {
  return (
    <div>
      <div>Box 부분입니다.</div>
      {children}
    </div>
  );
};

export default Box;

// App.tsx
import Box from "./Box";

function App() {
  return <Box>App 부분입니다.</Box>;
}

export default App;

// App2.tsx
import Box from "./Box";

function App() {
  return (
    <Box>
      <div>App 부분입니다.</div>
    </Box>
  );
}

export default App;
```

---

### 3-1. 오류 1) 여러 개의 Element

- 오류 코드

  ```typescript
  import Box from "./Box";

  function App() {
    return (
      <Box>
        <div>App 부분입니다.</div>
        <div>App 부분입니다.</div>
      </Box>
    );
  }

  export default App;
  ```

`JSX.Element` 타입을 사용했을 때와 마찬가지로 여러개의 `Element`를 사용할 수 없다.

![React.ReactChild error 1](/image/Typescript/ReactChildren/react_children_react_reactchild1.png)

---

### 3-2. 오류 2) Element와 Text 함께 사용

- 오류 코드

  ```typescript
  import Box from "./Box";

  function App() {
    return (
      <Box>
        <div>App 부분입니다.</div>
        App 부분입니다.
      </Box>
    );
  }

  export default App;
  ```

`React.ReactChild`은 단일 자식만 허용을 한다.

![React.ReactChlid error 2](/image/Typescript/ReactChildren/react_children_react_reactchild2.png)

---

### 3-3. 오류 3) 배열 사용

- 오류 코드

  ```typescript
  import Box from "./Box";

  function App() {
    return (
      <Box>
        {[1, 2, 3, 4].map((item, index) => (
          <div key={index}>{item}</div>
        ))}
      </Box>
    );
  }

  export default App;
  ```

`children` 부분에 배열을 사용을 한다면 어떻게 될까? 이것 또한 가능하지 않다.

`React.ReactChild` 타입은 `Element[]`의 형태의 타입을 허용하지 않는다는 오류를 볼 수 있다.

![React.ReactChild error 3](/image/Typescript/ReactChildren/react_children_react_reactchild3.png)

이런 오류를 해결하기 위해서는 `children`의 타입을 `React.ReactChild[]`로 바꾸어 주면 된다. 해당 타입은
여러개의 `Element`를 허용한다. 단 `Text`와 `Element`는 함께 사용할 수 없다.

---

### 3-4. ReactChild 타입은 더 이상 사용하지 않는다.

`children`를 `React.ReactChild` 타입으로 설정한 곳을 보면 `ReactChild`의 가운데에 선이 그어져 있는 것을 볼 수 있다.
이는 더 이상 사용하지 않는 타입이라는 뜻이다. 그러므로 사용하지 말고 이러한 타입이 있었다는 것만 알고 넘어가자.

![React.ReactChild deprecated](/image/Typescript/ReactChildren/react_children_react_reactchild4.png)

---

## 4. React.ReactNode

`React.ReactNode`는 모든 것을 담을 수 있는 최강의 타입이다. 단일 `Element`, 다수의 `Element`, 배열, 문자와 같은 것을
모두 사용할 수 있고 섞어서 사용 또한 가능하다.

```typescript
// Box.tsx
import React from "react";

interface IProps {
  children: React.ReactNode;
}

const Box = ({ children }: IProps) => {
  return (
    <div>
      <div>Box 부분입니다.</div>
      {children}
    </div>
  );
};

export default Box;

// App.tsx
import Box from "./Box";

function App() {
  return (
    <Box>
      App 부분입니다.
      {[1, 2, 3, 4].map((item, index) => (
        <div key={index}>{item}</div>
      ))}
      <div>App 부분입니다.</div>
    </Box>
  );
}

export default App;
```

---

## 5. React.PropsWithChildren`<T>`

해당 타입은 아래와 같이 사용이 가능하다.

1. `children` 뿐 아니라 다른 `props`가 존재할 경우

```typescript
import React from "react";

interface IProps {
  anyProps: any;
}

const Box = ({ children }: React.PropsWithChildren<IProps>) => {
  return (
    <div>
      <div>Box 부분입니다.</div>
      {children}
    </div>
  );
};

export default Box;
```

2. `children` 만 존재할 경우

```typescript
import React from "react";

const Box = ({ children }: React.PropsWithChildren) => {
  return (
    <div>
      <div>Box 부분입니다.</div>
      {children}
    </div>
  );
};

export default Box;
```

`React.PropsWithChildren<T>` 타입도 `React.ReactNode` 타입과 마찬가지로 모든 것을 담을 수 있다.

---

## 5. Conclusion

> 타입스크립트로 리액트 프로젝트를 진행하다 보면 항상 어떤 타입을 사용할지 구글로 검색을 하면서 찾는다. 매번 같은 내용이지만 아직
> 익숙하지 않아 계속 같은 내용을 찾는다. 히지만 이렇게 나만의 공간에 정리를 하고 계속 참고해서 보면 찾기도 훨씬 쉽고 정리고 스스로 하기
> 때문에 개념이 잘 들어온다. 또한 필요한 내용의 추가 삭제도 가능하니 앞으로 타입스크립트를 사용하면서 자주 사용되고 중요한 내용을 정리해야겠다.
> 이번 공부를 통해 React Children의 타입은 `React.ReactNode` 또는 `React.PropsWithChildren<T>`를 사용하면 좋겠다는
> 생각을 하게되었다.

---

## 참고

[React Children 과 친해지기](https://fe-developers.kakaoent.com/2021/211022-react-children-tip/)  
[React children with typescript. 리액트 children 컴포넌트 타이핑](https://itchallenger.tistory.com/394)  
[타입스크립트 : React.FC는 그만! children 타이핑 올바르게 하기](https://itchallenger.tistory.com/641)
