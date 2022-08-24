# Styled Components with TypeScript

## 1. 개요

리액트에서 스타일을 다룰 때 `styled-components`를 많이 사용한다. 이런 `styled-components`과 Typescript를
함께 사용하기 위해선 어떻게 해야할까? 이번 챕터에서는 해당 내용을 다룬다.

---

## 2. declaration 파일 설치

> $ npm i "styled-components"

위의 명령어를 통해 `syled-components`를 설치하고 바로 스타일을 다루기 위해 사용한다면 아래와 같은
오류를 발견한다.

![styled-components error1](/image/Typescript/StyledComponents/styled_components_typescript1.png)

이를 해결하기 위한 방법은 오류에서 찾을 수 있다. 아래의 명령어를 입력하여 `styled-components`와 관련된
declaration파일을 설치하면 된다.

> $ npm i --save-dev @types/styled-components

---

## 3. 스타일 컴포넌트에 사용되는 props의 타입 정하기

아래와 같은 컴포넌트가 있다고 생각하자. `Layout`은 스타일 컴포넌트로 `isRed` 값을 전달 받아 참이면 글씨의 색을 빨간색으로
그렇지 않으면 파란색으로 나타낸다.

```typescript
import React, { useState } from "react";
import styled from "styled-components";

const Layout = styled.div`
  color: ${(props) => (props.isRed ? "red" : "blue")};
`;

const Box = ({ children }: React.PropsWithChildren) => {
  const [isRed, setIsRed] = useState(true);
  return (
    <Layout isRed={isRed}>
      <div>Box 부분입니다.</div>
      {children}
    </Layout>
  );
};

export default Box;
```

`Layout`도 스타일 컴포넌트로 근본은 컴포넌트이다. 그렇기 때문에 `props`를 부모 컴포넌트에서 받아 사용할 수 있다.
위의 예제에서는 `isRed`라는 `props`를 전달 받은 경우이다. 하지만 전달만 했을 뿐 어떤 타입의 `props`인지 정의하지 않아
오류가 난 모습을 확인할 수 있다.

![styled-components props error](/image/Typescript/StyledComponents/styled_components_typescript2.png)

그렇다면 `Layout`의 `props` 타입을 정의하여 오류를 해결하자. 먼저 `Layout` 스타일 컴포넌트 `prop`의 타입을 정의하면
아래와 같다.

```typescript
interface IStyled {
  isRed: boolean;
}
```

그 다음 정의한 타입을 `Layout` 스타일 컴포넌트로 전달하여 `Layout` 스타일 컴포넌트에서 `props`를 사용할 수 있도록 하자.

```typescript
interface IStyled {
  isRed: boolean;
}

const Layout = styled.div<Layout>`
  color: ${(props) => (props.isRed ? "red" : "blue")};
`;
```

---

## 4. 스타일 컴포넌트의 customMedia에서 props가 사용될 때

`sytled components`의 customMedia은 반응형 디자인을 할 때 사용된다. 당연히 해당 기능을 사용할 때도
스타일 컴포넌트가 `props`로 받은 변수를 사용할 수 있기 때문에 타입스크립트를 사용한다면 몇가지 추가 작업을 해야한다.

참고로 `customMedia`의 내용은 아래와 같다.

```javascript
import { generateMedia } from "styled-media-query";

export const customMedia = generateMedia({
  mobile: "320px",
  tablet: "768px",
  desktop: "1024px",
});
```

먼저 기존 자바스크립트에서는 어떻게 사용되는지 알아보자.

```javascript
const SLunchmenu = styled.div`
  display: grid;
  ${customMedia.greaterThan("tablet")`
    row-gap: ${(props) => (props.summary ? "5px" : "10px")};
    row-gap: ${(props) => (props.summary ? "0.3125rem" : "0.625rem")};
  `}
`;
```

`SLunchmenu` 스타일 컴포넌트 내에 `customMedia.greaterThan()`을 사용하여 반응형 디자인을 할 수 있다.
이때 `customMedia.greaterThan()`에서 사용되는 `props`는 아래의 방법으로 사용할 수 있다.

```javascript
${(props) => (props.summary ? "5px" : "10px")}
```

그렇다면 타입스크립트을 사용한다면 어떻게 될까? `customMedia.greaterThan()`가 아닌 상황에서는 단지 `props` 타입만
정의하면 된다. 먼저 그렇게 해보자.

```typescript
interface IStyled {
  summary?: string;
}

const SLunchmenu = styled.div<IStyled>`
  display: grid;
  ${customMedia.greaterThan("tablet")`
    row-gap: ${(props) => (props.summary ? "5px" : "10px")};
    row-gap: ${(props) => (props.summary ? "0.3125rem" : "0.625rem")};
  `}
`;
```

`IStyled`로 `props`의 타입을 정의하고 `SLunchmenu` 스타일 컴포넌트로 전달하였다. 하지만 이렇게만 작성하면 아래와 같은 오류를 볼 수 있다.

![customMedia props error](/image/Typescript/StyledComponents/styled_components_typescript3.png)

`summary`가 `ThemeProps<any>` 타입에 존재하지 않는다는 에러이다. 우리는 `IStyled`에서 `SLunchmenu`에서
사용하는 `props`의 타입을 정의를 했다. 하지만 에러의 내용을 보면 타입이 `customMedia`에 제대로 전달이 되지 않았다는 것을 알 수 있다.
그러면 어떻게 `customMedia`에 `props`을 전달할 수 있을까?

방법은 아래와 같이 작성하면 된다.

```typescript
  ${({ summary }) => customMedia.greaterThan("tablet")`
    row-gap: ${summary ? "5px" : "10px"};
    row-gap: ${summary ? "0.3125rem" : "0.625rem"};
  `}
```

함수로 정의하면 된다. `summary`를 다시 한 번 더 인수로 전달하면 `customMedia`에서 사용할 수 있도록 한다.

---

## 5. ThemeProvider의 theme 타입

해당 부분은 티처캔 리팩토링을 진행하면서 다루게 될 경우 정리한다.

---

📅 2022-08-25 - 1. 개요 ~ 4. 스타일 컴포넌트의 customMedia에서 props가 사용될 때
