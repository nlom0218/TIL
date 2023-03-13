# 레벨1 점심 뭐 먹지 - step2

## 개요

|        미션        |          기간           | Repository                                                               | PR & Review                                                          | github page                                                     |
| :----------------: | :---------------------: | ------------------------------------------------------------------------ | -------------------------------------------------------------------- | --------------------------------------------------------------- |
| 점심 뭐 먹지 1단계 | `23-02-28` - `23-03-03` | [Repo](https://github.com/nlom0218/javascript-lunch/tree/nlom0218-step1) | [PR Review](https://github.com/woowacourse/javascript-lunch/pull/63) | [🍚 점심 뭐 먹지](https://nlom0218.github.io/javascript-lunch/) |

---

## 🚀 미션 회고

이번 미션의 목표였던

1. UI를 컴포넌트 단위로 생각하고 개발하는 연습
2. 재사용할 수 있는 컴포넌트를 고민해보기
3. 사용자 관점에서 중요하다고 생각하는 기능을 스스로 정의하고 E2E 테스트로 검증해보기 (2단계)

3가지에 도달할 수 있도록 많은 고민을 하였다. 특히 리액트가 아닌 바닐라자바스크립트에선 컴포넌트에 대해 한 번도 생각을 한 적이 없기 때문에 처음에는 어떤 식으로 접근을 해야 할지 감이 잡히지 않았다. 컴포넌트의 역할이 무엇인지를 스스로 생각하고 정의 내리는 것이 이번 미션의 핵심 포인트였지 않나 싶다.

E2E도 처음 사용해봤는데, 실제로 앱이 실행되는 것을 보니 콘솔로 테스트하는 것보다 훨씬 흥미롭고 재미있었다.

그리고 이번 미션을 통해 타입스크립트의 무서움에 대해 깨닫게 되었다. `지금까지 내가 배웠던 타입스크립트는 그냥 유치원 수준이었구나..` 라는 깨달음을 얻었다. 동기부여가 되어서 타입스크립트 스터디에 참여하게 되었으니 열심히 배워보자.

[타입스크립트 스터디 레포](https://github.com/woowacourse-study/2023-effective-typescript-study)

1단계 미션에서는 웹 컴포넌트와 함께 쉐도우 돔도 함께 사용했다. 이것에 초점을 두다간 학습 목표에 도달하지 못할 것 같은 느낌이 들어 2단계 미션이 시작할 때, 깔끔히 쉐도우 돔을 모두 걷어냈다. 때문에 컴포넌트에 대해 더 많은 생각을 할 수 있는 시간이 있지 않았나 싶다.

## 🗂️ 내가 생각하는 컴포넌트란?

`점심 뭐 먹지` 미션에서 가장 많은 고민을 한 부분이 `컴포넌트란 무엇인가?`라고 확신할 수 있다. 이런 본질적인 궁금증을 리액트를 사용할 때에도 하지 않았다. 그렇기 때문에 더 많은 고민과 생각이 있었던 것 같다.

미션에는 다양한 컴포넌트들이 존재한다. 물론 이것도 어디까지 나누냐에 따라 다르지만, 대부분의 크루들이 모두 비슷하게 나누었을 것이다.

### 모달 컴포넌트

나눈 컴포넌트들이 어떤 역할을 하는지 생각을 하였고 컴포넌트에 해당 역할을 수행할 수 있도록 책임을 부여했다. 예를 들어 모달창인 경우 아래와 같은 기본 역할을 가지고 있다.

1. 배경이 어두워진다.
2. 모달창이 등장한다.
3. 배경을 클릭하면 모달창이 사라진다.
4. action이 closeModal인 버튼을 클릭하면 모달창이 사라진다.

이것이 모달창이 가지고 있는 기본 역할(기능)이다. 어떤 모달이라도 위와 같은 기능을 가져야 한다. 위의 역할 중 1,2번은 UI에 관한 것이고 3,4번은 모달창이 가져야 할 기능이다.

이런 모달창을 가지고 있는 컴포넌트들은 위의 기능을 기본적으로 포함하고 있다. 좀 더 자세히 모달 컴포넌트를 살펴보자. 참고로 이번 미션은 웹 컴포넌트로 진행하였다.

```javascript
import errorHandler from '../../utils/errorHandler';
import CustomElement from '../CustomElement';

class Modal extends CustomElement {
  renderTemplate(): string {
    return `
      <div class="modal">
        <div class="modal-backdrop"></div>
        <div class="modal-container">
          ${this.innerHTML}
        </div>
      </div>
    `;
  }

  render = () => {
    super.render();

    this.initEventHandlers();
  };

  close = () => {
    if (!this.parentElement) return errorHandler.doesNotExistElement();

    this.parentElement.remove();
  };

  initEventHandlers = () => {
    const $modalBackdrop =
      this.querySelector < HTMLDivElement > '.modal-backdrop';
    const $closeModalButton =
      this.querySelector < HTMLButtonElement > 'button[action="closeModal"]';

    if (!$modalBackdrop) return errorHandler.doesNotExistElement();
    if (!$closeModalButton) return errorHandler.doesNotExistElement();

    $modalBackdrop.addEventListener('click', this.close);
    $closeModalButton.addEventListener('click', this.close);
  };
}

customElements.define('r-modal', Modal);

export default Modal;
```

핵심이 되는 기능만 빠르게 살펴보자. 위의 코드에서 `close` 메서드는 모달창을 닫는 기능을 가진다. 만약 이런 모달의 기능을 사용하고 싶으면 `<r-modal>{원하는 다른 내용}</r-modal>` 와 같이 작성하면 된다. 미션에서는 음식점 생성 모달과 음식점 상세 페이지 모달이 있다.

### 즐겨 찾기 아이콘 컴포넌트

하나의 컴포넌트만 더 알아보자. 이번엔 `FavoriteIcon` 컴포넌트이다. 해당 컴포넌트의 기본 역할은 아래와 같다.

1. 별 아이콘을 가지고 있다.
2. 클릭 할 수 있다.
3. 클릭하면 즐겨찾기 아이콘이 토글이 되어 디자인이 바뀐다.

위의 역할을 수행하기 위해 `FavoriteIcon` 컴포넌트가 어떤 책임을 가지고 있는지는 아래의 코드에서 확인할 수 있다.

```javascript
import errorHandler from '../../utils/errorHandler';
import CustomElement from '../CustomElement';

class FavoriteIcon extends CustomElement {
  static get observedAttributes() {
    return ['favorite'];
  }

  private get isFavorite() {
    return this.hasAttribute('favorite');
  }

  private get restaurantName() {
    return this.getAttribute('restaurantName');
  }

  renderTemplate = () => {
    return `
      <div
        id="favorite-icon"
        name="${this.restaurantName}"
        class="favorite-restaurant-icon ${
          this.isFavorite ? 'favorite-icon' : 'not-favorite-icon'
        }">${this.isFavorite ? '★' : '☆'}</div>
      `;
  };

  render = () => {
    super.render();

    this.initEventHandlers();
  };

  clickFavoriteIcon = () => {
    if (this.isFavorite) {
      this.removeAttribute('favorite');
    } else {
      this.setAttribute('favorite', '');
    }

    this.dispatchEvent(
      new CustomEvent('toggleFavorite', {
        bubbles: true,
        detail: {
          restaurantName: this.restaurantName,
        },
      }),
    );
  };

  initEventHandlers = () => {
    const $favoriteIcon = this.querySelector('.favorite-restaurant-icon');

    if (!$favoriteIcon) return errorHandler.doesNotExistElement();

    $favoriteIcon.addEventListener('click', this.clickFavoriteIcon);
  };
}

customElements.define('r-favorite-icon', FavoriteIcon);

export default FavoriteIcon;
```

이번에도 코드가 길다. 빠르게 핵심만 짚고 넘어가자. 위 코드에서 `clickFavoriteIcon` 메서드를 보자. 해당 메서드에선 두 가지의 일을 한다.

첫번째로는 자기 자신의 `favorite` 어트리뷰트를 생성하고 제거한다. 이는 즐겨찾기 아이콘의 UI 변경과 관련되어 있다.

두번째는 `CustomEvent`를 사용하여 루트에 이벤트를 보낸다. 즉, 여기서는 단지 클릭을 했다는 이벤트만 보낼 뿐 음식점의 정보를 직접 바꾸지 않는다. 음식점의 정보를 바꾸는 것은 해당 컴포넌트가 아니라 더 상단으로 올라가야 한다고 생각했기 때문이다. 직접 도메인의 데이터를 바꾸는 것은 한 곳에서 이루어지길 바랬다.

### 결론

컴포넌트의 기본 역할을 생각하였고 이를 컴포넌트에서 구현하였다. 다른 컴포넌트들은 단순히 이를 가져다 씀으로써 추가 구현 없이 같은 기능을 사용할 수 있을 뿐이다.

---

## 🧚‍♀️ CustomEvent

이번 미션에서 가장 유용하게 사용한 기능은 바로 `CustomEvent`라고 할 수 있다. 컴포넌트에서 `CustomEvent`를 발생시켜 상단으로 보내 도메인 작업과 화면 렌더링 작업을 다른 곳에서 관리할 수 있도록 하였다.

처음 사용하는 기술이라 어려움이 있을 것이라 생각하였는데, 막상 사용해보니 어렵지 않아서 많이 사용했다. 기본 사용은 아래와 같다. 일단 아래의 `this`는 `HTMLElement`이여야 한다.

```javascript
this.dispatchEvent(
  new CustomEvent(이벤트 이름, {
    bubbles: true, -> 버블링 여부
    detail: {
      이벤트를 통해 전달할 데이터
    },
  })
);
```

이벤트의 이름, 이벤트를 통해 전달할 데이터를 스스로 만들 수 있다. 이것이 매력적으로 다가왔다. 지금까지 사용한 이벤트를 보면 `click`, `submit` 과 같이 범용적인 단어였지만 `CustomEvent`에선 내 입맛대로 바꿀 수 있었다. 때문에 난 이벤트가 어떤 동작을 할지를 더 명확하게 알려주기 위해 이벤트의 이름을 자세하게 적었다. 아래의 코드를 참고!

```javascript
window.addEventListener('load', this.initLoad);
document.addEventListener(
  'openRegisterRestauranModal',
  render.openRegisterRestaurantModal
);
document.addEventListener('changeFilter', this.changeRestaurantFilter);
document.addEventListener('changeSort', this.changeRestaurantSort);
document.addEventListener('createRestaurant', this.addRestaurant);
document.addEventListener(
  'openRestaurantDetailModal',
  this.openRestaurantDetailModal
);
document.addEventListener('deleteRestaurant', this.deleteRestaurant);
document.addEventListener('toggleFavorite', this.toggleRestaurantFavorite);
document.addEventListener('chagneRestaurantType', this.chagneRestaurantType);
```

이벤트의 이름만 보고도 충분히 어떤 이벤트가 발생할 것인지를 예상할 수 있지 않는가? 이런 점이 좋았다. 음식점을 추가, 삭제, 즐겨찾기에 추가, 삭제 등과 같은 도메인 로직에 필요한 정보, 화면 렌더링에 필요한 정보도 전달할 수 있어 해당 책임을 다른 객체에 부여할 수 있었다.

---

## 🎨 render 함수의 분리

2단계에서는 `render`와 관련한 함수를 분리하여 html이 랜더링하는 과정을 관리하였다. 기존에는 app.ts에서 도메인 관리, 이벤트 처리, 랜더링을 모두 했었기 때문에 복잡함이 있었는데 `render` 폴더를 만듦으로써 역할을 나누어 주었다.

이와 같이 구조를 바꾼 이유는 아래와 같다.

1. 여러번 반복되고 여러 곳에서 사용되는 렌더링이 있을 수 있다.
   - 예를 들면, 이벤트에 대한 성공 실패 여부를 보여주는 것은 한 곳이 아니라 여러 곳에서 사용될 수 있다.
   - 이런 경우 같은 코드를 함수로 만들어 분리해두면 편하다고 생각하였다.
2. app.ts는 화면에서 발생시키는 이벤트에 따라 데이터만 관리한다.
3. 바뀐 데이터에 따른 화면 렌더링은 다른 곳에서 하는 것이 적합한 역할 분리라고 생각한다.

2,3번 이유를 아래의 코드를 통해 더 살펴보자.

```javascript
// app.ts
toggleRestaurantFavorite = ({ detail }: CustomEvent) => {
  const { restaurantName } = detail;

  this.#restaurants.forEach((restaurant) => {
    if (restaurant.getName() === restaurantName) restaurant.toggleFavorite();
  });

  const targetRestaurant = this.#restaurants.filter(
    (restaurant) => restaurant.getName() === restaurantName
  )[0];

  render.toggleRestaurantFavorite(this.#restaurantListType, targetRestaurant);

  saveRestaurants(this.#restaurants);
};
```

위 함수는 toggleFavorite 이벤트가 발생했을 때 실행되는 함수이다.. toggleFavorite 이벤트를 통해 음식점의 이름을 알아내고 해당 음식점의 즐겨 찾기 여부만 바꾸는 역할을 하고 있고 바뀐 결과에 따른 화면 렌더링과 로컬스토리지에 바뀐 음식점 목록을 저장하는 것은 모두 다른 함수에서 진행한다.

이렇게 render 을 통해서만 화면이 랜더링이 되도록 하였다.

---

## ⛓️ 옵셔널 체이닝

아래와 같이 DOM 객체에 접근하려 한다.

```javascript
const $targetRestaurant = document.querySelector<CustomRestaurantItem>(
      `r-restaurant[name="${restaurant.getName()}"]`,
    );
```

위 `$targetRestaurant`는 CustomRestaurantItem 또는 null 값을 가진다. null이 될 수도 있으므로 바로 이벤트를 달 수 없는 상황이 생긴다. 이를 해결하기 위한 해결법 중 하나가 바로 옵셔널 체이닝이다. 하지만 이것이 과정 올바른 것일까? 라는 생각을 들었다.

어떠한 이유로 인한 화면 렌더링의 문제로 DOM 자체가 없을 수 있고 코드 상의 오류로 접근을 못할 수도 있다. 때문에 옵셔널 체이닝으로 있다라는 것을 가정하여 이벤트를 다는 것이 아니라 그 전에 없을 때를 대비하여 오류를 발생시키기로 하였다. 아래처럼 말이다.

```javascript
if (!$targetRestaurant) return errorHandler.doesNotExistElement();
```

실제로 이와 같은 타입 가드로 인해 하나의 오류를 발견할 수 있어고 해결하였다.

여담으로 타입스크립트를 사용하면서 타입을 강제하는 것(타입 단언)보다 타입 선언을 통해 타입을 정하여 사용하자.

---

## 🔥 적절한 메서드 사용

아래의 코드를 보자.

```javascript
const targetRestaurant = this.#restaurants.filter(
  (restaurant) => restaurant.getName() === restaurantName
)[0];
```

위 코드는 특정 이름을 가지는 음식점을 찾는 코드이다. `Array.filter()` 메서드를 사용하고 있고 배열의 값에 접근하기 위해 `[0]`를 추가하였다. 과연 `Array.filter()` 메서드가 단일 요소를 찾는데 알맞는 메서드일까?

물론 찾을 수 있다. 하지만 이보다 더 적절한 메서드가 있다. 바로 `Array.find()`이다. 위의 코드를 아래와 같이 리팩터링을 시도했다.

```javascript
const targetRestaurant = this.#restaurants.find(
  (restaurant) => restaurant.getName() === restaurantName
);
```

지금까지 단일 요소까지도 `Array.filter()` 메서드를 사용한 이유를 생각해보면 별다른 이유 없이 무지성으로 사용했던 것 같다. 정말 지금까지 아무런 생각없이 `Array.filter()` 메서드만 사용하였고 해당 메서드를 통해서도 값을 잘 찾아왔기 때문이다. 이러한 습관 때문에 더 다양한 그리고 상황에 알맞은 메서드에 대해 고민할 수 있는 기회가 없었다. 항상 의문을 가지며 메서드를 사용하는 습관을 들여보자. 무엇이 더 좋은 메서드인지! 고민!

---

## 🎈Falsy 한 값 활용하기

자바스크립트에선 `false`만 `false`인 것이 아니다. 이 말이 무엇이냐? 숫자 0, 빈문자열 또한 불리언으로 `false`로 바뀔 수 있다는 것이다.

아직 느낌이 확 와닿지 않아서 지금까진 `===`를 사용하여 직접 비교하며 조건문을 사용했었다. 하지만 앞으로 더 익숙해지도록 노력해봐야겠다.

예를 들어 배열의 길이가 0일 때, 에러를 발생시키고 싶다. 그렇다면 평소에는 아래와 같이 코드를 작성했을 것이다.

```javascript
if (array.length === 0) throw new Error();
```

여기에서 0이 `false`라는 것을 이용하면 아래 처럼 리팩터링을 할 수 있다.

```javascript
if (!array.length) throw new Error();
```

추가적으로 아래는 MDN에서 설명하는 Javascript falsy values이다.

| Value        | Type      | Description                                                                                                       |
| ------------ | --------- | ----------------------------------------------------------------------------------------------------------------- |
| null         | Null      | The keyword null - the abscence of any value.                                                                     |
| undefined    | Undefined | undefined — the primitive value.                                                                                  |
| false        | Boolean   | The keyword false.                                                                                                |
| NaN          | Number    | NaN — not a number.                                                                                               |
| 0            | Number    | The Number zero, also including 0.0, 0x0, etc.                                                                    |
| -0           | Number    | The Number negative zero, also including -0.0, -0x0, etc.                                                         |
| 0n           | BigInt    | The BigInt zero, also including 0x0n, etc. Note that there is no BigInt negative zero — the negation of 0n is 0n. |
| ""           | String    | Empty string value, also including '' and ``.                                                                     |
| document.all | Object    | The only falsy object in JavaScript is the built-in document.all.                                                 |

---

## 👍 잘한 점

1. 웹 컴포넌트에 대한 좋은 경험을 하였다.
2. 타입스크립트 학습에 동기부여가 되었다.
3. 컴포넌트가 무엇인지에 대해 나름 많은 생각을 했다.
4. 메시지를 커스튬하여 사용하였다. 이 또한 컴포넌트로 만들었따.

---

## 👎 아쉬운 점

1. 쉐도우 돔에 대한 지식이 부족하여 이를 제거하였다.
2. E2E를 제대로 모르는 상태에서 사용해서 찜찜하다.

---

## 👊 앞으로의 각오

1. 학습 목표에 도달하기 위해 노력하기
2. 추가적인 학습도 중요하지만 일단은 학습 목표이다!

---

📅 2023-03-14
