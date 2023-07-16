# 레벨2 페이먼츠 - step3

## 개요

|        미션         |        기간         |                               Repository                               |                              PR & Review                              |                        github page                        |                                   storybook                                   |
| :-----------------: | :-----------------: | :--------------------------------------------------------------------: | :-------------------------------------------------------------------: | :-------------------------------------------------------: | :---------------------------------------------------------------------------: |
| 페이먼츠 미션 1단계 | 23-05-02 - 23-05-08 | [Repo](https://github.com/nlom0218/react-payments/tree/nlom0218-step3) | [PR & Review](https://github.com/woowacourse/react-payments/pull/308) | [💳 페이먼츠](https://nlom0218.github.io/react-payments/) | [📚스토리북](https://nlom0218-step3--64438130ed1ba3bc955c84aa.chromatic.com/) |

## 미션회고

페이먼츠 미션의 마지막 단계의 필수 요구사항은 다음과 같다.

1. 사용하던 모달을 분리해서 npm으로 배포하고, 그 라이브러리를 집접 import 해서 사용하기
2. 문서로서 스토리북을 고도화하기 위해 리팩터링
3. '카드를 등록 중입니다.' 스피너 추가

2, 3번의 요구사항보다 1번 요구사항에 초점을 두고 3단계를 진행하였다. 간략히 말하자면 스토리북은 7 버전으로 업데이트를 한 뒤, autodocs 기능을 사용하였. '카드를 등록 중입니다.' 스피너는 피그마에 있는 요구사항을 그대로 반영하였다. 다른 크루들의 스피너를 보면 멋진 애니메이션이 있어 눈이 즐거운데 난 그렇지 못해 아쉬운 마음이 있다.

거의 화요일부터 토요일까지 모달을 분리하고 npm 배포를 하였다. 하도 많이 바뀌었다 보니 Major 버전도 하나 올라가고 Patch 버전도 수도 없이 업데이트되었다. `모달을 쉽게 등록하고 쉽게 사용하기 위한 방법`을 찾기 위해 나름 노력했다고 생각한다.

## noah-modal

이번 미션에서 내가 배포한 모달의 이름은 `noah-modal`이다. 아래의 주소에서 해당 라이브러리를 확인할 수 있다.

[npm - noah-modal 바로가기](https://www.npmjs.com/package/noah-modal)

[githubRepo - noal-modal 바로가기](https://github.com/nlom0218/noah-modal)

문서를 통해 사용법을 알려주기 위해 나름 READMD도 작성해 보았다. 다른 개발자들이 사용하진 않겠지만 영어로 작성했다면 조금 더 멋진 문서가 만들어지지 않았을까 싶다.

### 😵‍💫 방향을 잡지 못한 0버전의 noah-modal

2단계 미션에서 모달을 만들 때, contextAPI를 적용하였다. 이후 모달을 라이브러리로 분리하기 전, 오류가 없는지 확인해 보았다. 그 결과 같은 컴포넌트 내에서 또 다른 모달을 띄우게 된다면 모달이 하나만 나타나는 것이 아니라 두 개가 나타나는 오류가 있었다. 때문에 contextAPI를 그대로 유지해야 할지 고민이었다.

그래서 처음엔 useModal, Modal(리액트 컴포넌트)를 사용할 수 있는 라이브러리를 만들고 이를 불러와 필요한 곳에 import 하여 사용할 수 있도록 했다.

예를 들어 다음과 같다.

```typescript
function Something() {
    const { isModalOpen, animation, delayMsTime, openModal, closeModal, changeAnimationMode } = useModal()

    // ...

    return (
        // ...
        {isModalOpen && <MyModal
            animation={animation}
            closeModal={closeModal}
            delayMsTime={delayMsTime}
            // ...
         />}
    )
}
```

실제 사용과는 정확히 같진 않지만 대략 위와 같은 형식이다. useModal를 통해 모달을 만드는데 필요한 여러 상태를 불러온다. 이렇게 되면 사용하는 입장에서 신경 써야 할 부분이 이만저만이 아니다. 보다 쉽게 모달을 만들고 모달을 사용하고 싶은 생각이 들었다. 또한 contextAPI, Provider를 통해 모달을 한 곳에서 관리하고 싶은 마음도 있었다.

### 🌁 Provider를 통해 모달을 등록할 순 없을까? - 2버전의 noal-modal

provider를 적용하고 싶은 생각이 머리속을 지배했다. 그래서 기존의 방법이 아니라 다른 방법을 찾아 나섰다. 마치 react-router-dom에서 route를 등록하는 것처럼 modal도 등록할 순 없을까?라는 생각을 하게 되었고 다음과 같은 방향으로 생각을 해보았다.

1. ModalProvider에 서비스에서 사용하게 된 모달 등록하기
2. 모달에 자신을 식별할 수 있는 name 추가하기
3. openModal, closeModal 함수에 name 인자를 넣어 필요한 곳에서 사용하기

1번이 가장 까다로웠다. 2, 3번은 1번이 완료되면 쉽게 구현이 가능했다.

### 1️⃣ ModalProvider에 서비스에서 사용하게 된 모달 등록하기

1번을 해결하기 위해 일단 문제를 두 개로 나누었다.

#### `첫 번째는 "어떻게 등록할 것인가?"이다.`

처음에 생각했던 방법은 다음과 같다.

```typescript
// Index.tsx

function Index() {
    //...
    return (
        <>
            // ...
            <ModalProvider>
                <App />
            </ModalProvider>
        </>
    )
}

// App.tsx

function App() {
    const { registerModal } = useModal()

    useEffect(() => {
        registerModal(myModal)
    }, [])

    // ...
    return (
        // ...
    )
}
```

App 컴포넌트를 ModalProvider로 감싸고 App 컴포넌트 내부에서 useModal에서 제공하는 registerModal 함수를 통해 모달을 등록하는 방법이다. 이를 같은 컴포넌트 단에서 하게 된다면 해당 컴포넌트는 ModalProvider로 감싸여있지 않기 때문에 모달을 등록해도 모달을 불러와 사용할 수 없기 때문에 두 개의 컴포넌트로 분리하였다.

이를 한 번에 하는 방법을 고민을 하다 솔로스타가 "ModalProvider에 modal를 props로 넘겨주면 되지 않느냐?" 하는 해결법을 제시해 주었다. 그래서 최종적으로 ModalProvider를 사용하는 방법은 다음과 같다.

```typescript
function App() {
  // ...
  return (
    <>
      // ...
      <ModalProvider modal={myModal}>
        <App />
      </ModalProvider>
    </>
  );
}
```

#### `두 번째는 "어떻게 모달을 관리할 것인가?"이다.`

ModalProvider에게 내가 서비스에 등록할 modal 객체를 넘겼다. 이후에 내가 원하는 모달을 불러와야 한다. 때문에 다음과 같이 리액트 컴포넌트를 useModal 훅을 통해 반환을 할 수 있도록 하고 싶었다.

```typescript
function App() {
  const { Modal } = useModal();

  return (
    <>
      // ...
      {Modal && Modal}
    </>
  );
}

export default App;
```

Modal은 undefined 일 수도 있기 때문에 `&&`를 활용하여 있을 경우에만 랜더링을 하게 된다. 여기에서 중요한 점은 Modal은 함수여야 한다는 것이다. 즉 다음과 같이 useModal을 통해 Modal를 만들어야 한다.

```typescript
return () => (
  <Modal
    title={title}
    name={name}
    isAbleBackdropClick={isAbleBackdropClick || true}
    delayMsTime={delayMsTime || 0}
    animation={animation}
    closeModal={closeModal}
    children={component}
  />
);
```

### 2️⃣ 모달에 자신을 식별할 수 있는 name 추가하기

그렇다면 모달을 등록하기 위해선 어떤 값이 필요할까? 모두 중요하지만 식별할 수 있는 name이 제일 중요하다. 해당 name으로 등록된 모달을 열고 닫을 수 있기 때문이다. 아무리 여러 개의 모달을 등록해도 name이 없으면 안 된다. 당연히 typescript이기 때문에 필수값이 없으면 에러가 난다.

name가 더불어 다양한 값이 필요하다. 다음은 페이먼츠 미션에 등록한 모달이다.

```typescript
[
  {
    title: '카드사 등록',
    component: <CreditCardCompanyModal />,
    name: 'creditCardCompany',
    delayMsTime: 500,
  },
];
```

이중 titls, component, name은 필수이고 delayMsTime 옵션값이다. 이뿐만 아니라 isAbleBackdropClick 속성도 있다. 이와 관련한 자세한 설명은 REDAME, npm noah-modal 라이브러리에서 볼 수 있다.

### 3️⃣ openModal, closeModal 함수에 name 인자를 넣어 필요한 곳에서 사용하기

지금까지 등록을 위한 과정이었다면 이번엔 모달을 열고 닫는 과정이다. openModal, closeModal는 useModal 훅을 통해 사용할 수 있다. 사용하는 방법은 간단하다. 2번에서 등록한 모달의 name를 인자로 넘기면 된다.

```typescript
function someThing() {
  const { openModal, closeModal } = useModal();

  const fn1 = () => openModal('myModal');

  const fn2 = () => closeModal('myModal');

  // ...
}
```

만약 잘못된 name를 넘기면 당연히 모달을 나타나지 않고 console에서 해당 오류를 확인할 수 있다.

### 💣 noah-modal에 대해 아쉬운 점

1. name의 타입이 string이다.(유니온이었으면 더욱 좋았을 것!)
2. 중복된 name에 대한 방어코드가 없다.
3. animation이 끝나기 전 모달을 다시 연다면 모달이 열리지 않는다.

## 👍 잘한 점

1. 사용성 높은 모달을 만들기 위해 계속 고민을 하였다.
2. 스피너를 생각보다 어렵지 않게 만들었다.
3. 무엇보다 npm 배포를 성공했고 이를 실제로 사용해 본 것!

## 👎 아쉬운 점

1. 모달 라이브러리에 많은 시간을 사용했기 때문에 다른 요구사항을 소홀히 했다.
2. noah-modal 커밋을 중구난방으로 했다.
3. 리뷰어와 많은 핑퐁을 하지 못했다.

## 👊 앞으로의 각오

1. 지금처럼 요구사항에 대해 최대한 집중하자.

---

📅 2023-05-15
