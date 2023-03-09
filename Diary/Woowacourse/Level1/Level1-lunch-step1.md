# 레벨1 점심 뭐 먹지 - step1

## 개요

|        미션        |          기간           | Repository                                                               | PR & Review                                                          | github page                                                     |
| :----------------: | :---------------------: | ------------------------------------------------------------------------ | -------------------------------------------------------------------- | --------------------------------------------------------------- |
| 점심 뭐 먹지 1단계 | `23-02-28` - `23-03-03` | [Repo](https://github.com/nlom0218/javascript-lunch/tree/nlom0218-step1) | [PR Review](https://github.com/woowacourse/javascript-lunch/pull/38) | [🍚 점심 뭐 먹지](https://nlom0218.github.io/javascript-lunch/) |

---

## 🚀 미션 진행 과정

레벨1의 세번째 미션의 제목은 `점심 뭐 먹지`이다. 카테고리, 이름, 캠퍼스로 부터 거리, 설명, 참고 링크와 같은 음식점에 대한 값을 바탕으로 음식점 목록을 생성하고 이를 카테고리별로 필터링을 하고 이름순, 거리순으로 정렬하는 기능을 만드는 것 까지가 1단계였다. 엄청 심플해 보였다! 하지만.. 타입스크립트를 도입해야 했고, 컴포넌트 단위로 모듈화하여 개발, 컴포넌트 단위 테스트도 함께 고민하여 미션 속에서 이를 녹여내야 했었다.

타입스크립트! 오케이 지금 티처캔 프로잭트를 진행하면서 `잘`은 아니지만 `어느정도` 써 봤으니 금방 적응할 수 있을 것이란 생각이 들었다. 그건 틀렸다. 모르는 타입스크립트 문법이 너무 많았다. 단순히 타입을 지정하는 것이 아닌 타입스크립트를 `잘`쓰기 위한 공부를 이번 3월에 병행하며 미션들을 진행해야겠다.

컴포넌트 단위로 모듈화! 음 감이 잡히지 않았다. 여기저기에서 어떻게 하는 것이 좋다. 웹 컴포넌트, 쉐도우 돔 등 다양한, 그렇지만 난 잘 모르는 용어들이 마구 들리기 시작했다. 고생을 할 것이라고 생각을 했고 이는 정답이었다. 결국 페어인 솔로스타와 함께 웹 컴포넌트를 사용하기로 결심하였고 `Custom elements`와 `shadow DOM`를 처음으로 사용하게 되었다. 다행히 솔로스타는 이전 미션을 웹 컴포넌트로 했기 때문에 옆에서 많은 것을 배울 수 있었다. 단지 내가 너무 느려 피해를 주지 않았을까 하는 미안한 마음이 든다.. 구조를 이해하며 넘어갈려면 속도는 늦어질 수 밖에... 😢 세번째 미션을 진행하면서 웹 컴포넌트에 더 익숙해 지도록 하자. 그리고 웹 컴포넌트가 정답은 아닐 수 있다. 그리고 그렇게 대중화된 기능이 아닐 수 있다. 하지만 한 번 써보면서 공부했던 것과 아예 모르는 것은 하늘과 땅 차이라고 생각한다. 그러므로 성실하게 공부하자.

컴포넌트 단위 테스트! 시간 부족으로 인해 미 작성... 😭😭😭 리팩터링 기간에 열심히 작성하자! 아마 그리고 다음주 2단계를 진행할 동안 컴포넌트 단위 테스트도 계속 공부할 듯 하다

`점심 뭐 먹지` 미션을 통해 마치 처음 프로그래밍 공부를 하는 때로 돌아가는 듯한 느낌을 받았다. 오랜만에 머리가 터질 것 같은 느낌을 받으면서 고통을 받기도 하였지만 이것이 내가 프로그래밍을 좋아하는 이유이지 않을까 싶다. 하나하 배우면서 지식을 넓히는 것이 즐겁다고 생각한다. 남은 3월 동안 부족한 자바스크립트 개념을 채우고 타입스크립트에 익숙해 지도록 노력하자.

---

## 👬 페어프로그래밍 진행 과정

`점심 뭐 먹지` 미션의 페어는 솔로스타이다. 평소 솔로스타와 더 친해지고 싶은 마음이 있어는데 이번 기회에 더 가까워 질 수 있는 기회였다. 물론 페어프로그래밍이기 때문에 어떠한 상황이 찾아올지는 모르겠지만, 페어프로그래밍이 끝난 지금은 이전보다 더 가까워졌다고 생각한다. 물론 나만 그럴수도 있지만..ㅎㅎ

정말 많은 도움을 얻었다. 진정한 고수를 만난 느낌이랄까...? 지금까지 해오던 프로그래밍의 시야를 한 단계 더 넓어지는 느낌을 받았다. 아직은 시야만 넓어졌을 뿐 넓은 시야로 가진 못해서 꾸준한 노력이 필요하다. 자바스크립트의 문법을 사용하는 것 부터 시작하여 타입스크립트의 다양한 문법을 활용하여 타입을 정의하는 것을 옆에서 보고 질문을 하고 싶은 마음이 굴뚝 같았지만 많이 하지 못한 아쉬움이 남는다.

시야가 넓어졌다. 라는 것을 조금더 설명하자면 처음 자바스크립트 공부를 시작하고 배열 메서드를 배울 때, `forEach` 메서드와 `map` 메서드의 차이점에 대해 명확히 구분되지 않아 고생했던 적이 있었다. 그러다가 어느 순간 이 둘의 차이를 알고 적절한 상황에서 메서드를 활용할 수 있었던 것 또한 시야가 넓어졌다고 할 수 있다. 이번 페어프로그래밍에서도 이와 같은 경험을 하게 되었다. 미션에서 정렬와 필터 기능을 구현하기 위해 아래와 같은 클래스 필드를 만들어 사용하였다.

```javascript
class App {
  #filterPipes: Partial<
    Record<'filter' | 'sort', (restaurants: Restaurant[]) => Restaurant[]>
  > = {};
}
```

- `Partial(파셜)`: 각 속성을 선택적으로 만드는데 사용
- `Record`: 키 타입에 유연성을 제공하는 제너릭 타입

`#filterPipes` 객체를 이용하면 위의 타입스크립트의 타입을 먼저 이해해야 한다. 이러한 타입스크립트의 타입은 솔로스타와 페어프로그래밍을 통해 알게 된 것으로 이에 대해 조금더 공부하여 TIL에 정리할 계획인다. 아무튼! `#filterPipes`를 통해 정렬과 필터 기능을 동시에 구현을 할 수 있다.

먼저 필터링하는 부분이다.

```javascript
if (value === '전체') {
  this.#filterPipes.filter = (_restaurants: Restaurant[]) =>
    Restaurants.getSorted(_restaurants, Restaurants.byName);
} else {
  this.#filterPipes.filter = (_restaurants: Restaurant[]) =>
    Restaurants.filterByCategory(_restaurants, String(value));
}
```

`value`를 기준으로 `#filterPipes` 객체의 `filter` 값이 정해진다. 전체일 경우 필터링을 하지 않기 때문에 이름순으로 정렬하는 함수가 값으로 되고 전체가 아닐 경우 특정 카테고리를 기준으로 필터링 되는 함수가 값이 된다.

이번엔 정렬하는 부분이다.

```javascript
const sortFilter = (_restaurants: Restaurant[]) =>
  Restaurants.getSorted(
    _restaurants,
    $rSelect.getSelectedOption()?.value === 'name'
      ? Restaurants.byName
      : Restaurants.byDistance
  );

this.#filterPipes.sort = sortFilter;
```

정렬은 이름순, 거리순 중 하나로 하기 때문에 `value`가 `name`일 때와 그렇지 않을 때 전달하는 두번째 인자가 달리진다. 아무튼...! 위의 과정에서 `#filterPipes` 객체의 `sort`의 값이 정해진다.

이로써 알 수 있는 사실은 `#filterPipes`의 키들은 함수를 값으로 가지고 있는 것이다. 이 함수는 `Restaurant[]`를 인자로 받는 것이고..!

마지막으로 음식점 목록이 렌더링되는 부분을 살펴보자. 이 부분이 개인적으로 많이 흥미로웠다.

```javascript
this.$restaurantList.setRestaurants(
  Object.values(this.#filterPipes).reduce(
    (filteredRestaurants, filter) => filter(filteredRestaurants),
    this.#restaurants
  )
);
```

여기에서 `this.$restaurantList`은 DOM 객체이고 이는 `custom Elements`이다. 해당 객체에는 `setRestaurants`라는 메서드가 있는데 이 메서드를 통해 음식점 목록이 바뀌고 비뀐 음식점 목록이 랜더링되는 것이다.

아래는 `setRestaurants` 메서드의 인자인 음식점 목록(`Restaurant[]`)을 구하는 과정이다.

1. `Object.values()` 메서드를 사용하여 `this.#filterPipes` 객체의 값을 배열로 만든다. 이때, 배열의 요소는 함수가 된다.
2. `reduce()` 메서드를 사용한다. 이때 초기값은 모든 음식점 목록이 된다.
3. `filter`는 필터링, 정렬에 필요한 함수(1번 과정에서의 배열의 요소)이다. 이는 음식점 목록을 인자로 받고 필터링, 정렬을 거친 음식점 목록을 반환한다.

이렇게 객체의 값을 함수로 사용하면서 `reduce` 메서드를 통해 원하는 값을 얻어내는 과정에서 시야가 넓어진 느낌이 든다. 이러한 과정은 혼자서는 하지 못했을 것이다. 하지만 운이 좋게 이번 페어프로그래밍을 통해서 경험하게 되었고 앞으로 나의 성장에 큰 도움이 될 수 있을 것이라 믿는다.

---

## 👍 잘한 점

- 보다 익숙한 방법으로 미션을 진행할 수 있었지만, 새롭게 배운다는 생각으로 처음 접하는 개념을 배우면서 미션을 진행했다.
- 포기하지 않았다... 만으로도 잘했다고 생각한다.

## 👎 아쉬운 점

- 시간이 촉박해서 그런지... 질문을 많이 하지 못했다.
- 타입스크립트에 대한 개념이 부족하다.
- 테스트 코드 작성이 미흡하다.

## 👊 앞으로의 각오

- 이번 미션을 통해 배운 것들을 내것으로 만들자. TIL를 통해 정리도 하자.
- 레벨 1이 끝나기 전, 이팩티스 타입스크립트를 보며 공부하자.

## 🛠️ 리팩터링

### components 폴더 세분화

![components 구조](https://user-images.githubusercontent.com/57981252/223123487-67f3e87d-2e5d-488c-8e2a-a33fe39c34bb.png)

다른 컴포넌트에서 사용할 수 있는 컴포넌트들은 `shared` 폴더로 묶어 관리할 수 있도록 하였다. 여기에 있는 컴포넌트들은 단독으로는 사용할 수 없고 서로 묶기거나 자식 elements를 가질 수 있다. 이렇게 여러 곳에서 재사용할 수 있는 컴포넌들이 있는 곳인 `shared`이다.

반대로 이름을 가진 폴더에 존재하는 컴포넌트들은 실제로 웹에 랜더링 되는 컴포넌트들이다.

추가적으로 `R-`를 제거하여 파일명을 통해 컴포넌트가 하는 역할을 더 쉽게 파악할 수 있도록 리팩터링하였다.

### 초기 데이터의 활용

`r-select` 태그에는 초기값이 필요하다. 그 이유는 옵션 값이 필요하기 때문이다. 이를 `fixtures.ts` 폴더에서 불러와 사용할 수 있도록 하였다.

리팩터링 전 코드 👇

```javascript
// app.ts
class App {
  // ...

  initSelect() {
    this.$restaurantFilterSelect.setOptions([
      { value: '전체', label: '전체' },
      ...Restaurant.CATEGORIES.map((category) => ({
        value: category,
        label: category,
      })),
    ]);

    this.$restaurantSortSelect.setOptions([
      { value: 'name', label: '이름순' },
      { value: 'distance', label: '거리순' },
    ]);
  }

  initModalSelect() {
    this.$restaurantModalCategory.setOptions([
      { value: '', label: '선택해주세요' },
      ...Restaurant.CATEGORIES.map((category) => ({
        value: category,
        label: category,
      })),
    ]);

    this.$restaurantModalDistance.setOptions([
      { value: '', label: '선택해주세요' },
      ...Restaurant.DISTANCE_BY_MINUTES.map((distance) => ({
        value: distance,
        label: `${distance}분 내`,
      })),
    ]);
  }

  // ...
}
```

리팩터링 후 코드 👇

```javascript
class App {
  // ...

  // app.ts
  initSelect() {
    this.$restaurantFilterSelect.setOptions(DEFAULT_FILTER_OPTIONS);
    this.$restaurantSortSelect.setOptions(DEFAULT_SORT_OPTIONS);
  }

  initModalSelect() {
    this.$restaurantModalCategory.setOptions(DEFAULT_MODAL_CATEGORY_OPTIONS);
    this.$restaurantModalDistance.setOptions(DEFAULT_MODAL_DISTANCE_OPTIONS);
  }

  // ...
}

// fixtures.ts
export const DEFAULT_FILTER_OPTIONS = [
  { value: FILTER.value.entire, label: FILTER.label.entire },
  ...Restaurant.CATEGORIES.map((category) => ({
    value: category,
    label: category,
  })),
];

export const DEFAULT_SORT_OPTIONS = [
  { value: SORT.value.name, label: SORT.label.name },
  { value: SORT.value.distance, label: SORT.label.distance },
];

export const DEFAULT_MODAL_CATEGORY_OPTIONS = [
  { value: DEFAULT_OPTION.value, label: DEFAULT_OPTION.label },
  ...Restaurant.CATEGORIES.map((category) => ({
    value: category,
    label: category,
  })),
];

export const DEFAULT_MODAL_DISTANCE_OPTIONS = [
  { value: DEFAULT_OPTION.value, label: DEFAULT_OPTION.label },
  ...Restaurant.DISTANCE_BY_MINUTES.map((distance) => ({
    value: distance,
    label: `${distance}분 내`,
  })),
];
```

### components/index 파일 역할 부여

단순히 파일을 불러오는 것이 아닌 불러운 파일을 객체로 묶고 타입이 필요한 컴포넌트들에 대해서는 타입을 export를 하는 역할을 주었다.

```javascript
import Restaurant from './restaurant/Restaurant';
import RestaurantList from './restaurant/RestaurantList';
import Modal from './modal/Modal';
import Button from './shared/Button';
import FormItem from './shared/FormItem';
import Input from './shared/Input';
import Select from './shared/Select';
import Textarea from './shared/Textarea';

export type CustomRestaurantListElement = RestaurantList;

export type CustomSelectElement = Select;

export type CustomModalElement = Modal;

export default {
  Restaurant,
  RestaurantList,
  Modal,
  Button,
  FormItem,
  Input,
  Select,
  Textarea,
};
```

### 이벤트핸들러 콜백함수 분리

평소에는 열심히 분리하였던 이벤트핸들러의 콜백함수였지만, 이번에는 시간이 부족하여 분리를 하지 못하였다. 이를 리팩터링 때 완료하였다!

### 매직넘버 상수화

매직넘버의 상수화도 마찬가지로 미쳐 완료하지 못했다. 평소에 습관을 만들어야지... 왠만하면 콜백함수도..

### constructor 추가

지금까지 클래스를 다루면서 인스턴스를 생성할 때 별다른 인자가 없으면 `constructor`를 사용하지 않았다. 하지만 클래스를 사용한다 했을 때, 다른 개발자들은 `constructor`를 먼저 찾으면서 구조를 파악할 수 있다는 생각을 하게 되었다. 이로 인해 `constructor`를 추가하게 되었다. 그렇다면 이번에 사용한 웹 컴포넌트들은 어떻게 할까? 모두 클래스인데 모두 부모를 상속받고 있다. 당연히 인자는 없고! 과연..?

### querySelector 실패 시 분리하기

```javascript
?.querySelector<HTMLSelectElement>('#select')
      ?.addEventListener('change', (event) => {})
```

위와 같은 코드가 있다. `$select`는 element일 수도 있고 `null`일 수도 있다. `null`인 경우 이벤트를 등록할 수 없기 때문에 `?`를 붙여야 한다. 하지만 오류로 인해 항상 있다고 생각한 `$select`이 없다면 이벤트는 발생하지 않을 것이고 발생하지 않은 이벤트로 인해 사용자는 물론 오류를 수정하고자 하는 개발자에게도 큰 어려움이 찾아올 것이다. 이때, 확실한 예외 처리를 하는 것이 좋다.

때문에 아래와 같이 리팩터링을 진행하였다. 아직 예외처리는 하지 않았지만 2단계 미션을 통해 예외처리를 완성할 것이다.

```javascript
const $select = $shadowRoot.querySelector < HTMLSelectElement > '#select';
if (!$select) return;

$select.addEventListener('change', (event) => {});
```

---

📅 2023-03-05
