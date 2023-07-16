# 레벨2 장바구니 협업 - step1

## 개요

|      미션      |        기간         |                                    Repository                                    |                                  PR & Review                                   |                            github pages                             |                                   storybook                                    |
| :------------: | :-----------------: | :------------------------------------------------------------------------------: | :----------------------------------------------------------------------------: | :-----------------------------------------------------------------: | :----------------------------------------------------------------------------: |
| 장바구니 1단계 | 23-05-23 - 23-05-26 | [Repo](https://github.com/nlom0218/react-shopping-cart-prod/tree/nlom0218-step1) | [PR & Review](https://github.com/woowacourse/react-shopping-cart-prod/pull/87) | [🛒 장바구니](https://nlom0218.github.io/react-shopping-cart-prod/) | [📚 스토리북](https://nlom0218-step2--646f1b197e7cf65a7e5c4bb2.chromatic.com/) |

---

## 미션 회고

벌써 레벨 2의 마지막 미션이다. 레벨 2의 마지막 미션은 바로 이전 미션이었던 장바구니 미션의 연장선이다. 지금까지 서버 없이 mock 데이터로 개발을 했다면, 장바구니 협업 미션은 백엔드 크루와 함께 협업을 하면서 서버를 연동을 해야 했다.

바로 이전 미션에서 MSW를 사용하여 실제 API 통신을 하는 것과 같은 환경을 만들어 두면서 개발을 하였다. 때문에 서버만 잘 완성이 된다면 큰 무리 없이 서버를 연동을 할 수 있었다. 로컬에서만 확인하는 것이 요구 사항이었는데, 함께 협업을 하는 백엔드 크루인 베베와 에단이 서버를 배포를 해주어서 실제 배포 페이지에서도 서버를 연동할 수 있었다. 서로 동 떨어져 있지만 API로 연결이 되니 더욱 재밌게 미션을 진행할 수 있었다.

장바구니 협업 미션의 1단계 요구 사항은 다음과 같다.

1. 클라이언트
   - 서버 연동을 통해 상품 목록을 보여준다.
2. 사용자 별 장바구니 기능이 정상적으로 동작하게 만든다.
   - 장바구니에 담긴 아이템 목록을 확인할 수 있다. (장바구니 아이템 목록 조회)
   - 상품을 장바구니에 추가할 수 있다. (장바구니 아이템 추가)
   - 장바구니에 담긴 아이템의 수량을 변경할 수 있다. (장바구니 아이템 수량 변경)
   - 장바구니에 담긴 아이템을 삭제할 수 있다. (장바구니 아이템 삭제)
3. 웹 프론트엔드 요구사항
   - 사용자는 여러 서버 중 하나를 선택할 수 있다.
   - 사용자 인증 정보를 저장한다.

서버와 연동하여 기존의 기능인 아이템 목록 조회, 아이템 추가, 수량 변경, 삭제는 큰 어려움이 없었다. 단순히 에단과 베베의 서버 주소로 바꾸었더니, 잘 되었다.(적어도 그렇게 기억한다.)

보다 초점을 두 내용은 다음과 같다.

- Suspense와 ErrorBoundary
- 서버상태와 UI상태를 동기화하기

하나하나 많은 내용이 담길 것으로 예상된다. 때문에 '페어프로그래밍 진행 과정' 다음에 자세히 되돌아본다.

---

## 👬 페어프로그래밍 진행 과정(With BN)

우테코에서의 마지막 페어프로그래밍이다. 마지막은 우테코의 인기스타인 도리와 함께하였다. 도리에게는 많은 고마움이 있었다. 그중 한 가지는 이전 장바구니 미션코드를 가져오기 위해 나와 도리 중 하나의 코드를 선택해야 했는데, 도리가 나의 코드로 협업 미션을 진행하자고 적극적으로 이야기를 해준 것이다. 처음부터 함께 작성 한 코드라면 코드의 구조와 흐름을 파악하는데 여러움은 없었을 것이다. 하지만 각자 이전 장바구니 미션을 진행하였다. 그래서 서로의 코드를 보고 이해하는 데에는 어느 정도 비용이 든다고 생각한다. 때문에 코드를 보고 이해, 파악하는 것은 여간 힘든 일이 아니었을 것이다. 나의 코드를 보고 열심히 분석하고 이해하기 위해 힘쓴 도리에게 고맙다는 이야기를 하고 싶다.(정말 상태의 흐름이 꽝이었을 텐데 말이다...)

이전 미션을 진행하면서 나름 Suspense를 도입하고자 노력했지만, 마음대로 되지 않아 속상한 마음을 지닌 채 Suspense를 대신하여 조건부 렌더링을 하였다. 하지만 찜찜한 마음은 가시지 않았다. 하지만 이번 도리와의 페어프로그래밍을 통해 Suspense를 어떻게 적용하는지 배울 수 있었다. 간단한 소감은 '생각보다 Suspense를 감싸야할 곳이 많다. 즉, 코드는 깔끔해지지만 귀찮다.'라는 것이다. 앞으로 유용하게 사용할 뿐 아니라 Suspense가 어떻게 적용되는지도 알아가 보자.

장바구니 미션에서 진행한 코드를 바탕으로 백엔드 크루들과 협업하여 서버를 연동하는 것이 이번 1단계 미션의 목표였다. 때문에 새로운 컴포넌트, 페이지를 만들 필요가 없기 때문에 나와 도리 모두 크게 부담감을 가지지 않고 진행하였다. MSW를 통해 이미 통신을 얼추? 흉내를 냈기 때문에 베베와 에단이 서버를 보내주기 전까지 Suspense, ErrorBoundary, 전역 상태 관리에 대해 이야기를 나누며 코드 리팩터링을 진행하였다. 또한 서버를 선택할 수 있어야 했기 때문에 이를 위한 기능을 추가하였다.

수요일에는 선릉 캠퍼스에서 백엔드 크루인 베베와 에단과 함께 협업을 진행하였다. 사실 1단계에서는 함께 이야기를 나눌 부분이 없었다. 이미 명세서가 정해져 있었기 때문이다. 그래서 그날엔 간단히 연동이 잘 되는지, 어떤 오류가 나타나는지에 대해 이야기를 나누면서 함께 미션을 진행하였다. 베베와 에단도 함께 이야기를 잘해주었기 때문에 어려운 것 없이 스무스하게 협업을 진행할 수 있었다.

---

## Suspense와 ErrorBoundary

이번 미션에서 가장 먼저 진행한 작업이 바로 Suspense와 ErrorBoundary를 적용하는 것이었다. 보다 선언적으로 리액트를 사용하기 위해 Suspense와 ErrorBoundary를 꼭 활용하고 싶었지만, 생각대로 잘 되지 않아 지금까진 조건부 렌더링을 통해 데이터가 페칭 하는 과정(pending, rejected, fulfiled)에 따른 컴포넌트를 렌더링을 하였다. 당연히 아쉬웠다. 다행히 도리의 도움을 통해 이번 미션에서는 적용할 수 있었다.

아래에선 Suspense 위주로 이야기를 풀어본다.

---

### Suspense란?

우선 내가 적용하고 싶었던 Suspense가 무엇인지부터 짚고 넘어가 보자. Suspense에 대해 리액트 공식문서에서는 다음과 같이 소개하고 있다.

> <Suspense> lets you display a fallback until its children have finished loading.

즉, Suspense는 하나의 컴포넌트이다. 이를 사용하면 자식의 로딩이 끝가지 전까지 자식 대신 fallback을 보여줄 수 있다. 때문에 children, fallback을 Props을 가진다. 자세한 내용은 공식문서를 참고!

[React Suspense](https://react-ko.dev/reference/react/Suspense)

---

### 난 왜 Suspense를 도입하고 싶어 하는가?

충분히 조건부 렌더링을 통해 데이터를 fetching(비동기 통신)하는 과정을 나타낼 수 있다. 하지만 리액트를 보다 선언적으로 사용하기 위해 Suspense를 항상 사용하고 싶었다. 선언형의 반대는 명령형이다. 지금까지 내가 해온 방식은 명령형이라고 할 수 있다. 즉, 어떻게에 초점을 두고 코드를 작성하였다. 아래의 예시를 살펴보자.

```typescript
if (status === 'pending') {
  // 또는 loading
  return <Loading />;
}

return <Children />;
```

status의 상태에 따라 다른 컴포넌트를 렌더링 한다. status의 상태를 if문을 통해 내가 직접 판단을 하고 이에 따른 컴포넌트를 return 해야 한다.

위와 같은 명령형 프로그래밍을 Suspense를 사용하게 되면 보다 더 직관적으로 바뀌게 된다. 즉, 더 이상 status라는 상태를 관리하지 않아도 된다. 단지 Suspense를 선언함으로써 알아서 loading일 때의 상황을 나타낼 수 있게 된다. 이런 점이 매력적으로 느껴졌다. 다음과 같이 말이다.

```typescript
return (
  <Suspense fallback={<Loading />}>
    <Children />
  </Suspense>
);
```

어떻게 나눌지는 생각하지 않아도 된다. 그저 Suspense를 사용하기만 하면 된다. 마치 우리가 레시피를 보며 음식을 만드는 것이 아니라 음식점에서 음식을 주문하는 것처럼 말이다.

토스의 SLASH 21 컨퍼런스 중 이와 관련된 내용이 있다. 정말 많은 도움이 되었던 내용이었고 이를 보고 Suspense를 더 사용하고 싶다는 생각이 들었다.(이를 미리 알았더라면,,)

[![토스의 SLASH 21 컨퍼런스](https://img.youtube.com/vi/FvRtoViujGg/0.jpg)](https://www.youtube.com/watch?v=FvRtoViujGg)

---

### 그러면 지금까지 왜 Suspense를 도입하지 않았는가?

정확히 말하자면 도입하지 않은 것이 아니라, 지식이 부족하여 도입하지 못했다. Suspense가 어떻게 동작하는지를 파악하지도 않고 빨리 미션에 도입하고 싶었기 때문에 Suspense를 더 어렵게 생각하지 않았나 싶다.

일반적인 fetch 메서드를 가지고는 Suspense를 사용할 수 없다. 여러 기술 블로그에서 찾아본 결과 fetch 메서드를 사용하는 것과 더불어 Promise 객체를 throw해주는 무언가가 필요하다. 이러한 것을 모르고 단순히 fetch 메서드만 사용을 하니 당연히 Suspense를 도입하지 못했던 것이다.

---

### 이제, Suspense를 도입해 보자!

Suspense 도입을 위해 Promise 객체를 throw해주는 무언가를 포함한 fetch 유틸 혹은 훅을 만들어야 하는가? 아니다! 아직 Suspense를 도입하기 위한 요구 사항은 불안정하고 문서화되지 않았다고 한다. 천천히 기다려보자.

대신, 리액트 공식문서에서는 이미 Suspense를 적용할 수 있는 프레임워크를 사용하는 것을 제안하고 있다.(아래는 공식문서의 내용이다.)

> Data fetching with Suspense-enabled frameworks like Relay and Next.js

Relay, Next.js뿐 아니라 Recoil에서도 Suspense를 도입할 수 있다. selector와 함께 사용할 수 있는데, 아래는 해당 내용이 담긴 Recoil 공식문서의 내용이다.

[recoiljs.org](https://recoiljs.org/ko/docs/guides/asynchronous-data-queries/#asynchronous-example-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%98%88%EC%A0%9C)

이를 바탕으로 Suspense를 미션에 도입할 수 있었다.

---

### Suspense를 도입하기 위한 준비과정

지금까지 미션에서 recoil를 사용하면서 상태를 관리하기 위해 atom를 사용했다. atom을 사용하여 상태를 렌더링 하고 업데이트를 진행한다. 하지만 비동기를 상태로 가지고 있을 순 없다. 정리하자면, 다음과 같다.

1. 나는 비동기를 상태로 가지고 싶다.
2. 즉, 상태가 pending, fulfilled, rejected인지 알고 싶다.
3. atom은 상태를 관리하기 최적이지만 위와 같은 기능을 수행할 수 없다.
4. 결론은 atom으론 Suspense를 사용할 수 없다.

위와 같은 문제를 해결하는 것이 Suspense를 도입하는 첫걸음이었다. Recoil 공식문서에서도 알 수 있듯이 Suspense를 사용하기 위해선 selector를 사용해야 한다. 하지만,,, 여기서 또 문제..! selector는 읽기 전용이기 때문에 업데이트를 할 수 없다. set 메서드을 사용 하면 쓰기도 할 순 있지만 보다 편리한 방법으로 atom과 selector를 연결했다. 바로 atom의 default를 selector의 값으로 사용하는 것이다.

```typescript
export const cartItemsStateSelector = selector({
  key: 'cartItemsStateSelector',

  get: ({ get }) => {
    const server = get(serverState);

    return fetchData({ url, method, server }); // 비동기 통신 과정
  },
});

const cartItemsState = atom({
  key: 'cartItemsState',
  default: cartItemsStateSelector,
});
```

위 코드는 실제 미션에서 사용한 코드이다. 장바구니 물품의 목록을 불러와 상태로 만드는 과정이다. 상태를 바탕으로 렌더링을 해야 하고 업데이트도 해야 하기 때문에 atom을 사용하였고 Suspense를 사용하기 위해 default를 selector를 바탕으로 만들었다.

selector의 get 메서드를 보면 리턴하는 값은 Promise 객체가 된다. 이를 바탕으로 Promise 객체가 pending인지 fulfilled인지 rejected인지를 Suspense와 ErrorBoundary가 판단하게 된다.

뭔가,, 더 고민을 하고 학습을 진행하게 되면 굳이 atom을 사용하지 않고 selector로만 해도 될 것 같다. 또한 서버 상태와 UI 상태를 동기화하는 과정이 반드시 필요하기 때문에 recoil이 아니라 비동기 통신에 더욱 특화된 라이브러리인 react query를 사용하면 좋지 않을까? 하는 생각이 든다. 물론, 아직 사용해 본 적은 없지만 그렇다고 들었다:)

아무튼 Suspense를 사용할 준비를 맞췄다.

---

### 드디어 Suspense를...!

위 코드에서 cartItemsState(장바구니 물품)를 실제로 사용하는 곳으로 가보자. 가장 먼저 떠오르는 곳은 장바구니 페이지가 될 것이다. 장바구니 물품을 렌더링을 담당하는 컴포넌트는 다음과 같이 3가지이다. 아래로 갈수록 자식 컴포넌트이다.

- CartList: 장바구니 전체 페이지
- CartItems: 장바구니 물품 모두
- CartItem: 장바구니 물품 하나

위 컴포넌트들 중, cartItemsState를 가져오는 곳은 CartItems 컴포넌트이다. 때문에 다음과 같이 컴포넌트를 작성하였다.

```typescript
function CartItems() {
  const cartItems = useRecoilValue(cartItemsState);

  return (
    <>
      {cartItems?.map((item) => (
        <CartItem
          cartId={item.id}
          product={item.product}
          key={item.product.id}
        />
      ))}
    </>
  );
}
```

Suspense가 보이는가? 보이지 않는다. 그렇다면 어디에 Suspense를 사용해야 하는가? 보류 중인 데이터인 cartItems가 존재하는 컴포넌트의 부모 컴포넌트에서 사용해야 한다. 즉, CartList에서 Suspense를 사용하여 CartItems를 감싸주면 된다. 만약 감싸주지 않는다면 다음과 같은 오류가 나타난다.

[Suspense 오류](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FxSx1X%2FbtskMntMRK4%2FxAWXElsFRAX7nKP78cx7n1%2Fimg.png)

이를 해결하기 위해 다음과 같이 Suspense로 CartItems를 감싸주자.

```typescript
function CartList() {
  return (
    <S.Container>
      // ...
      <Suspense
        fallback={Array.from({ length: 3 }, (_, index) => (
          <SkeletonCartItem key={index} />
        ))}
      >
        <CartItems />
      </Suspense>
      // ...
    </S.Container>
  );
}
```

이로써 cartItems이 pending일 때(아직 데이터를 불러오고 있을 때) Suspense의 fallback에 정의된 컴포넌트가 대신 렌더링이 된다.

[Suspense가 적용된 모습](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FMwJSP%2FbtskLNsA9b2%2F6KxzxkeVexIiJ3a68s1S40%2Fimg.png)

앞으로 Suspense를 잘 다루고 싶다. 스켈레톤 UI가 이쁘다,,

---

### ErrorBoundary는 어떻게?

ErrorBoundary는 어떻게 할까? 비동기 통신이 실패하게 되면 Promise객체는 rejected가 된다. 이때 에러를 발생시키고 이런 에러를 ErrorBoundary가 캐치하게 된다. 미션에서는 모든 에러를 잡아내기 위해 가장 최상단에 Outlet 컴포넌트를 ErrorBoundary로 감쌌다.

```typescript
function App() {
  return (
    <>
      <GlobalStyle />
      <Suspense fallback={<LoadingHeader />}>
        <Header />
      </Suspense>
      <CommonPageStyle>
        <ErrorBoundary fallback={NotFound}>
          // 요부분!!
          <Outlet />
        </ErrorBoundary>
        <Suspense>
          <QuickMenu />
          <QuickMenuMobile />
        </Suspense>
      </CommonPageStyle>
    </>
  );
}

export default App;
```

재밌는 점은 ErrorBoundary은 함수형 컴포넌트가 아니라 클래스형 컴포넌트라는 것이다. 에러를 케치 하는 getDerivedStateFromError 생명주기 함수를 사용하기 위함으로 생각된다. 자세한 코드는 아래에서 확인할 수 있다.

[github.com](https://github.com/nlom0218/react-shopping-cart-prod/blob/nlom0218-step1/src/pages/ErrorBoundary/index.tsx)

---

## 서버상태와 UI상태를 동기화하기

리액트를 배우면서 가장 많이 한 고민이 바로 상태이다. 무엇이 상태이고 어떻게 상태를 관리해야 하는지 등 고려해야 할 점이 한두 가지가 아니다. 앞으로 계속 정복해 가야 할 부분이라고 생각한다. 그중 이번 미션에서는 서버상태, UI상태에 대해 나름? 고민을 해보았다. 이를 정리한 내용이다.

우선 내가 생각하는 서버상태는 데이터 베이스에서 받아온 것이다. 그리고 UI상태는 화면에 렌더링 되고 시간에 따라 변화할 수 있는 것이다. 이 두 상태는 모두 변할 수 있다. 데이터의 성격에 따라 다양한 상황이 발생할 수 있다. 일정 시간마다 새로운 데이터를 서버에서 받아와야 하는 경우, 사용자의 이벤트에 의해 업데이트된 새로운 데이터를 서버에서 받아와야 하는 경우가 있을 것이다. 또한 이런 서버에서 받아온 데이터가 바로 렌더링이 되어 즉각적으로 사용자에게 알려줘야 하는 경우 등등 변수는 다양할 것이다. 가장 흔한 경우는 '사용자의 이벤트에 따라 데이터가 업데이트되고 이런 최신화된 데이터를 즉각적으로 사용자에게 보여줘야 하는 경우'일 것이다.

서버에서 데이터를 왜 받아올까? 가장 큰 이유는 화면에 렌더링을 하기 위해서이다. 그렇다면 상태로 만들 필요가 있을까? 데이터를 읽기만 한다면 상태로 만들지 않아도 된다고 생각한다. 하지만 데이터가 업데이트가 된다면 어떻게 해야 할까? 당장 떠오르는 해결책은 상태로 만들어 관리하는 것이었다. 또한 이번 미션의 경우 productsState은 상태로 만들지 않아도 되었겠지만 Suspense 도입을 위해 recoil의 selector을 사용하여 상태로 만들었다.

---

### 얼마나 자주 동기화해야 할까?

장바구니에 담긴 물품의 수량을 예로 들어보자. 사용자가 장바구니에 물품을 추가하거나 물품의 수량을 증감을 한다. 이때 서버와 통신이 일어나게 되고 DB에는 최신 데이터가 들어가게 된다. 여기에 더불어 사용자에게 보여지는 화면에도 최신 데이터를 보여줘야 한다.

만약 이 둘이 다르게 된다면 어떻게 될까? 장바구니에 모든 물품을 담고 주문하기 위해 장바구니 페이지에 들어갔는데 내가 생각했던 물품이 없고 수량이 없다. 당연히 당황할 수밖에 없는 상황이다. 그래서 서버와 통신이 일어날 때, 화면에 보여지는 상태값도 함께 업데이트를 해야 한다.

즉, 민감한 내용이 아니더라도 서버상태와 UI상태를 항상 똑같이 유지하는 것이 중요하다고 생각한다.

---

### 아직은 무언가 비효율적인 동기화

서버상태와 UI상태를 항상 똑같이 유지하기 위해 사용자가 서버상태를 업데이트하는 이벤트를 발생시켰을 때, 업데이트하는 로직뿐 아니라 새로운 데이터를 받아오는 로직도 함께 실행할 수 있도록 하였다. 다음은 사용자가 장바구니에 물품을 담거나 수량을 증감하거나 삭제할 때 사용되는 함수이다.

```typescript
const updateCartItem: UpdateCartItem = async (url, method, body) => {
  // 장바구니 업데이트
  await fetchData<{ ok: boolean }>({ url, method, body, server });

  // 업데이트 된 장바구니 데이터가져오기
  const data = await fetchData<CartItemType[]>({
    url: FETCH_URL.cartItems,
    method: FETCH_METHOD.GET,
    server,
  });

  const newCartItems = data.map((cartItem) => {
    return {
      ...cartItem,
      isSelected: isSelected(cartItem.id),
    };
  });

  // UI상태 업데이트
  setCartItems(newCartItems);
};
```

위 함수는 다음과 같은 순서를 가진다.

1. 장바구니 업데이트를 위한 비동기 통신을 진행한다.
2. 업데이트된 최신의 장바구니 데이터 가져오기 위한 비동기 통신을 진행한다.
3. 위 데이터를 바탕으로 화면 렌더링에 필요한 UI상태를 업데이트한다.

수량을 1 올리기 위해 위의 함수가 실행되는 것이다. 10을 올리기 위해선 10번이 실행되고, 50을 올리기 위해선 50번의 실행... 역시 다시 생각해도 너무 비효율적이다. 한 번에 묶어서 비동기 통신이 한 번만 실행되게 하는 것이 깔끔해 보인다.

이 부분은 Throttling, Debouncing을 통해 해결하거나 UI를 미션에서 요구하는 사항과 다르게 구현함으로써 해결할 수 있을 것이다.

어떻게 적용을 해야 할까?는 앞으로 내가 가지고 가야 숙제이다.

---

## 👍 잘한 점

1. 생각보다 어렵지 않게 서버를 연결하였다.
2. 마지막에 배포하는 것이 아니라 중간에 배포를 하여 백엔드 크루들에게 중간 상황을 알려주며 미션을 진행하였다.

## 👎 아쉬운 점

1. 마지막 페어프로그래밍이여서 그런지 시간을 딱 정하지 않고 해서 혼란스러웠다.
2. 회고를 작성하는데 글이 이상하다고 느껴진다. 자신감이 부족해지는 기분이다.

## 👊 앞으로의 다짐

1. 연습을 많이 하자.
2. 내 생각을 말하고 글로 표현하자.
3. 기술이 아니라 내 생각

---

📅 2023-06-20
