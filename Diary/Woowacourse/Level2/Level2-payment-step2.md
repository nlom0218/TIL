# 레벨2 페이먼츠 - step2

## 개요

|        미션         |        기간         |                               Repository                               |                              PR & Review                              |                        github page                        |                                               storybook                                               |
| :-----------------: | :-----------------: | :--------------------------------------------------------------------: | :-------------------------------------------------------------------: | :-------------------------------------------------------: | :---------------------------------------------------------------------------------------------------: |
| 페이먼츠 미션 1단계 | 23-04-24 - 23-05-01 | [Repo](https://github.com/nlom0218/react-payments/tree/nlom0218-step2) | [PR & Review](https://github.com/woowacourse/react-payments/pull/242) | [💳 페이먼츠](https://nlom0218.github.io/react-payments/) | [📚스토리북](https://nlom0218-step2--64438130ed1ba3bc955c84aa.chromatic.com/?path=/docs/button--docs) |

## 미션회고

페이먼츠 2단계 미션의 필수 요구사항은 다음과 같다.

1. 주요 컴포넌트에 대한 Storybook 상호 작용 테스트
2. Controlled & Uncontrolled Components에 입각하여 Form 핸들링
3. Context API를 활용하여 전역 상태 관리 및 계층 재구성

이중 가장 부족했던 부분은 `Storybook 상호 작용 테스트`이다. 현재 스토리북을 보면 컴포넌트가 개별적으로만 테스트하고 있다. 여러 컴포넌트를 묶어 상호 작용 테스트는 미흡하다고 할 수 있다. 부족한 것을 알았으니 보완하기 위해 따로 시간을 내어 투자해야 하는 부분이라고 생각한다. 미루지 말고 꼭 하자.

2단계 미션에서 가장 큰 중점을 두고 있는 부분은 `Custom Hook`과 `Context API`이다. 추가적으로 리액트 훅으로 최적화도 시도해 보았지만 눈에 띄는 최적화가 이루어지지 않았다. 이 부분은 앞으로 레벨 3, 레벨 4에서 꾸준히 가져가야 할 고민거리가 될 것이다.

### 🗂️ Context API

나는 평소 Context API는 `'전역적으로 상태를 관리하기 위해 필요한 것'으로` 생각하고 있다. 하지만 이번 2단계 미션을 진행하고 공통 피드백을 받으면서 Context API에 대한 생각이 바뀌게 되었다.

우선, 리액트 공식 문서에서 Context API에 대해 다음과 같이 설명하고 있다.

> context의 주된 용도는 다양한 레벨에 네스팅된 많은 컴포넌트에게 데이터를 전달하는 것입니다. context를 사용하면 컴포넌트를 재사용하기가 어려워지므로 꼭 필요할 때만 쓰세요. 여러 레벨에 걸쳐 props 넘기는 걸 대체하는 데에 context보다 컴포넌트 합성이 더 간단한 해결책일 수도 있습니다.

리액트 공식 문서에서 `전역`이라는 단어는 찾아볼 수 없다. 즉, 전역이 중요한 것이 아니다. 중요한 것은 `데이터 전달`이다. 이를 정리하면 Context API는 다음과 같이 생각할 수 있다.

`데이터를 다른 컴포넌트에 전달하는 또 다른 방법`

그리고 전역으로 사용하지 않아도 된다. Context API를 사용하면 전역에서만 사용해야 한다는 생각을 버리고 컴포넌트 묶음에서 테이터를 전달하고 업데이트하기 위해 사용한다고 생각을 하도록 하자.

그렇다면 이번 2단계 미션에서는 어떤 Context API를 사용했느냐? 두 가지를 Context API로 활용하였다. `모달을 위한 Context API`와 `카드 등록을 위한 Context API`가 그것이다.

#### 1️⃣ `모달을 위한 Context API`

모달은 하나의 앱에 하나만 있다는 전제하에 Context API로 만들었다. 3단계가 끝난 현재 시점에서 많은 오류가 있는 코드이지만 어떻게 Context API를 사용했는지 소개하려 한다.

먼저 Provider를 만들어야 한다. 다음은 ModalProvider이고 이는 최상단 컴포넌트인 `src/index.tsx`에서 사용되고 있다. 최상단 컴포넌트에서 사용하는 이유는 모달이 `특정 컴포넌트 묶음`에서만 사용되는 것이 아니라 `앱 전체`에서 사용된다고 생각하기 때문이다.

```typescript
// ModalProvider

function ModalProvider({ children }: PropsWithChildren) {
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [animation, setAnimation] = useState<AnimationTypes>(ANIMATION.appear);
  const [delayMsTime, setDelayMsTime] = useState(0);

  const openModal = () => setIsModalOpen(true);

  const closeModal = () => setIsModalOpen(false);

  const changeAnimationMode = (mode: AnimationTypes) => {
    setAnimation(mode);
  };

  const changeDelayMsTime = (time: number) => setDelayMsTime(time);

  const initValue = {
    // ...
  };

  return (
    <ModalContext.Provider value={initValue}>{children}</ModalContext.Provider>
  );
}

export default ModalProvider;

// src/index.tsx

import React from 'react';
import { createRoot } from 'react-dom/client';
import { RouterProvider } from 'react-router-dom';
import router from 'router';

import ModalProvider from '@Contexts/Modal/ModalProvider';

const root = createRoot(document.getElementById('root') as HTMLElement);
root.render(
  <React.StrictMode>
    <ModalProvider>
      <RouterProvider router={router} />
    </ModalProvider>
  </React.StrictMode>
);
```

ModalProvider를 등록했으니 그 다음 `useContext`를 사용하여 모달을 열고, 닫아야 한다. 직접 컴포넌트에서 useContext를 사용해도 되겠지만 useAnimationModal 훅을 만들어 사용하였다. useAnimationModal은 다음과 같다.

```typescript
const useAnimationModal = () => {
  const {
    isModalOpen,
    animation,
    delayMsTime,
    openModal,
    closeModal,
    changeAnimationMode,
    changeDelayMsTime,
  } = useContext(ModalContext);

  const closeModalWithTime = () => {
    changeAnimationMode(ANIMATION.disappear);
    setTimeout(() => {
      closeModal();
      changeAnimationMode(ANIMATION.appear);
    }, delayMsTime);
  };

  return {
    isModalOpen,
    animation,
    delayMsTime,
    openModal,
    closeModal: closeModalWithTime,
    changeDelayMsTime,
  };
};

export default useAnimationModal;
```

이렇게 만든 useAnimationModal 훅을 필요한 곳에 사용한다. isModalOpen은 어떤 컴포넌트에서도 같은 값을 가리키기 때문에 앱에서 단 하나의 모달을 보장할 수 있다.라고 그땐 생각했다. 렌더링이 된 다른 컴포넌트 혹은 같은 컴포넌트 내에서 isModalOpen 값을 비교하여 또 다른 모달을 오픈한다고 했을 때, 2개, 3개... 여러 개의 모달이 열릴 수 있다. 이러한 오류는 3단계에서 해결하였다.

#### 2️⃣ `카드 등록을 위한 Context API`

1단계에서는 /register 페이지에서만 카드 등록에 필요한 상태를 관리하였다. 하지만 2단계 미션에서는 카드 별칭도 추가해야 했기 때문에 /register/alias이라는 페이지를 만들었다. 이런 이유로 인해 /register 페이지에서 카드 번호, 이름, cvc번호, 비밀번호, 카드사와 같은 정보를 /register/alias 페이지에 넘긴 후 별칭을 추가한 뒤 카드를 등록해야 한다. 즉, 상태의 이동이 필요하다.

여러 가지 방법이 있겠지만, 당시에 가장 먼저 떠오르는 것은 Context API였다. 단 카드 등록에 필요한 상태는 다른 페이지에서는 필요 없는 상태이다. 때문에 `Provider를 /register, /register/alias 페이지에만 감싸면 된다`.

즉, 다음과 같이 CreditCardRegisterProvider를 감쌌다.

```typescript
// src/router.tsx

import App from 'App';
import { createBrowserRouter } from 'react-router-dom';

import CreditCardAlias from '@Pages/CreditCardAlias';
import CreditCardRegister from '@Pages/CreditCardRegister';
import Home from '@Pages/Home';

import CreditCardRegisterProvider from '@Contexts/CreditCardRegister/CreditCardRegisterProvider';

const router = createBrowserRouter(
  [
    {
      path: '/',
      element: <App />,
      children: [
        {
          path: '',
          element: <Home />,
        },
        {
          path: 'register',
          children: [
            {
              path: '',
              element: (
                <CreditCardRegisterProvider>
                  {' '}
                  // 해당 부분과
                  <CreditCardRegister />
                </CreditCardRegisterProvider>
              ),
            },
            {
              path: 'alias',
              element: (
                <CreditCardRegisterProvider>
                  {' '}
                  // 해당 부분
                  <CreditCardAlias />
                </CreditCardRegisterProvider>
              ),
            },
          ],
        },
      ],
    },
  ],
  {
    basename: '/react-payments',
  }
);

export default router;
```

이후, useContext를 사용하여 카드 등록에 필요한 정보를 업데이트하고 마지막 카드 등록을 하기 위해 필요한 상태를 꺼냈다.

### 💄 Custom Hook

페이먼츠 미션에서의 주된 관심사 중 하나가 바로 Custom Hook이다. 다시, 점심 뭐 먹지 미션을 진행할 때 보다 기준을 잡아가는 느낌이 좋다.

기존 Custom Hook을 분리하는 기준으로는 단순히 '범용성', '재사용성'으로만 생각했다. 하지만 공통 피드백과 코드 리뷰를 통해 다시 잡은 기준은 다음과 같다.

1. 가독성 향상
2. 관심사 분리
3. 테스트의 용이성
4. 재사용성

이러한 기준으로 Custom Hook을 분리한다면 `코드의 생산성`이 높아질 수 있다.

#### 1️⃣ `가독성 향상을 위한 Custom Hook 분리`

선언형 프로그래밍과 명령형 프로그래밍이 떠오른다. 이에 대한 자세한 내용은 이전에 따로 정리를 해두었다. [명령형 프로그램과 선언형 프로그래밍](https://noah-dev.gitbook.io/til/etc/imperativeanddeclarative)을 정리를 하면서는 언제? 어떻게? 사용하는지 감이 잘 잡히지 않았지만 가독성 측면에서의 Custom Hook을 생각하니 `선언형 프로그래밍`이 어떤 것인지에 대해 느낌이 온다.

선언형 프로그랭밍은 간단히 말하면 `어떻게` 보다 `무엇`이라는 것에 초점을 둔 프로그래밍이다. 즉, 사용하는 입장에서는 어떻게 코드가 돌아가는지는 관심이 없다. 다만 가져가다 잘만 사용하면 장땡이다. 적절한 곳에 무엇을 가져다가 사용할지만 잘 정하면 된다. 이러한 점이 Custom Hook과 관련이 있다는 것을 느꼈다.

예를 들어, useMyPet이라는 훅이 있다. 이 훅은 pet, addPet, removePet이라는 값과 함수를 반환한다. 그렇다면 우리는 pet이라는 값이 어떻게 만들어졌는지 따지지 않고 필요한 곳에 렌더링을 하면 된다.(그래도 string인지 object인지 array인지는 알아야겠지...) 또한 addPet, removePet이라는 함수의 내부 동작은 관심 없다. 단지 필요한 곳에 호출하여 사용하면 될 뿐이다.

다시 정리하자면 Custom Hook의 네이밍 규칙인 use 접두사는 `"무엇을 사용한다."`라고 생각하면 이는 선언형 프로그래밍과 관련이 있다. 또한 컴포넌트 내부에서 addPet, removePet과 같은 함수의 내부 동작을 작성하는 것이 아니라 불러와 사용하기 때문에 `코드의 양이 줄어든다.` 우리는 훅에서 불러온 함수의 이름에서 어떤 역할을 하는지 짐작만 할 뿐이다.

#### 2️⃣ `관심사 분리를 위한 Custom Hook 분리`

리액트 컴포넌트의 관심사란 무엇일까? 이번 미션을 시작하기 전에는 전혀 생각하지도 못했던 점이다. 그래서 Custom Hook 분리가 어렵게 느껴졌던 것 같다.

리액트 컴포넌트의 관심사는 다음과 같다.

1. `UI`
2. `이벤트`

이 외의 관심사에 대한 것들은 Custom Hook으로 분리하자. 예를 들어 다음과 같은 것들이 있을 것이다.

- input의 유효성을 검사하고 오류를 반환하는 로직
- 데이터를 불러오고 전달(저장)하는 로직

레벨1에서는 함수 분리에 대해 많은 고민을 하였다. 관심사 분리가 이와 비슷하다고 생각한다. 함수를 분리하는 목적은 함수가 `단 한가지의 역할`을 할 수 있도록 하는 것에 있다. 여기에 추가적으로 `함수의 네이밍과 함수가 하는 역할을 동일`하게 하는 것도 있다.

UI에 대해서 먼저 생각하자면 컴포넌트의 가장 큰 역할이라고 할 수 있다. return 이후에 나오는 JSX이 그것이다. 이걸 Custom Hook으로 분리하는 것은 뭔가 어색하다. 이벤트에 대해서는 JSX에서 onClick, onSubmit 같은 이벤트를 발생시키면 그것을 받는 것까지만 하는 것이다. 이것이 바로 `리액트 컴포넌트의 관심사(역할)`이다.

이를 제외한 관심사(역할)은 함수를 분리한 것처럼 Custom Hook으로 분리할 수 있도록 고민해 보자.

또한 리액트 컴포넌트와 가까운 관계인지도 함께 생각해 보자.

#### 3️⃣ `테스트의 용이성을 위한 Custom Hook 분리`

세 번째 기준은 테스트의 용이성이다. 리액트 컴포넌트 내에 있는 로직에 대해서 테스트하는 것은 어려울 것이다. 하지만 이를 Custom Hook으로 분리한다면 분리한 독립적인 로직에 대해 테스트를 진행하기 쉬워지고 이로써 검증도 가능하다.

테스트가 쉬워진다는 것은 코드의 관리가 편해지는 것을 의미하므로 코드의 생산성도 향상될 수 있다.

#### 4️⃣ `재사용성을 위한 Custom Hook 분리`

자주 사용되는 함수를 유틸 함수로 정해서 여러 곳에서 재사용하는지를 생각하면 이해하기 쉬울 것이다. 리액트 컴포넌트에서 똑같은 로직이 반복되는 것을 분리하고 있지 않다면 바로 분리해 보자.

분리한 Custom Hook이 얼마나 재사용할지는 당장은 모를 수 있다. 그래도 일단 위의 기준으로 분리를 해두고 개발을 하는 과정에서 같은 로직이 사용이 된다면 재사용하여 사용할 수 있도록 하자. 만약 재사용하기 어려운 훅이라면 리팩터링을 통해 Custom Hook이 재사용할 수 있도록 해보자. 다시, 점심 뭐 먹지의 useListFiltering처럼...(다시, [점심 뭐 먹지의 useListFiltering)](https://noah-dev.tistory.com/30#%E2%AD%90%EF%B8%8F%20Custom%20Hook-1)

### 🧹 최적화에 대한 고민

useMemo, useCallback 훅을 사용하여 최적화를 시도해 봤지만 눈에 띄는 성과가 없어 결국엔 적용하지 않았다. 다른 크루들은 적극 사용하여 최적화를 하였다는 이야기를 들을 땐, 내가 잘못 사용하는 것인가? 내가 놓친 것이 있는 것인가?라는 초조함이 찾아온다.

이런 이야기를 리뷰어에게 이야기했다. 이후, 코드 리뷰를 통해 내가 놓쳤던 것을 알 수 있었다. 내가 지금까지 계속 비교했던 것은 useMemo로 묶은 상태가 변할 때만 어떻게 재랜더링이 되는지만 확인했던 것이다. 하지만, 부모의 상태가 변할 때는 생각을 하지 않았다. 잠시 2단계 미션의 코드에서 이를 확인해 봤지만,,, 결론은 똑같다 😇😇😇😇😇😇😇😇😇😇😇😇😇 또다시 딜레마에 빠진다. 내가 놓친 게 있나? 내가 잘못 테스트하고 있는 것인가?

마지막 미션에서는 주변 크루들의 도움을 받아서라도 최적화에 대한 감을 잡을 수 있도록 해보자. 앞으로 우테코 기간 내에서 많은 미션과 프로젝트를 통해 최적화를 많이 다룰 것이라 생각한다. 그러기 때문에 현재는 답답하고 만족스럽지 않더라도 급급하게만 생각하고 행동하지 말자. 속으로든 겉으로든.

Custom Hook, Context API도 처음엔 잘 몰랐는데, 지금은 처음보단 좋아졌던 것처럼 최적화도 가까운 미래엔 지금의 나보단 더욱 잘 다룰 것이라 믿는다.

### 👩‍👩‍👧‍👦 사용자가 먼저다

결국 개발을 하는 이유는 무엇일까? 깔끔한 코드를 작성하고 어느 누구에게 보여줘도 칭찬을 듣는 것일까? 물론 이러한 것도 중요하다. 사실 중요한 것을 넘어서 매우 기쁠 것이다.

하지만 이보다 조금 더 중요한 것은 내가 개발한 기능이 사용자에게 좋은 경험을 주는 것이다. 어떤 웹사이트를 이용한다거나 어플을 이용하면서 불편했던 경험이 누군가에게나 있을 것이다. 그렇게 된다면 꾸준히 이용하지 않고 시간이 지나면 지날수록 머릿속에서 사라질 것이다. 즉, 사용자 경험이 최악이었다고 할 수 있다.

그렇다면 사용자 경험을 고려한 개발은 무엇일까? 이번 미션을 예로 들어보자. 내가 만든 카드 등록을 사용자가 이용했을 때 과연 편하게 등록할 수 있을까? 사용자가 잘못 입력했을 때 오류 메시지는 적절한가? 디자인은 서로 잘 어울리고 앱에 적절한가?를 고려하면서 미션을 진행했다면 사용자를 충분히 고려했다고 할 수 있다.

사용자 경험을 증진시키기 위한 여러 요소들 가운데 투자 대비 가성비가 좋은 것이 `글`이다. 다시 말해, 어떤 기능을 이용하는 사용자에게 `적절한 메시지`는 UX 디자인이라고 할 수 있고 이는 특별한 능력을 요구하지 않는다.

추가적으로 나는 객관적으로 생각했을 때, 디자인을 많이 못한다고 생각한다. 많은 레퍼런스를 보며 디자인에 대한 감각도 키울 수 있도록 해보자. 스토리북도 적극 활용해서 많은 피드백을 받아보자.

## 👍 잘한 점

1. Custom Hook에 대한 기준을 잡아가고 있다.
2. 리뷰어와 많은 핑퐁을 했다.
3. 카드사, 모달을 CDD를 통해 구현하였다.

## 👎 아쉬운 점

1. 컴포넌트의 상호 작용 테스트를 하지 못했다.
2. 유효성 검사에 대한 로직이 체계적이지 못하다는 것을 느꼈다.
3. Custom Hook 테스트를 제대로 하지 않은 느낌이다.

## 👊 앞으로의 각오

1. 사용자의 경험을 생각하며 개발을 하자.
2. 코드의 품질을 높이는 것은 당연, 테스트도 신경 쓰자.

---

📅 2023-05-09
