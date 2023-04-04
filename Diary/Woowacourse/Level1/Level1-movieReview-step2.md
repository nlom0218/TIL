# 레벨1 영화 리뷰 - step2

## 개요

|      미션       |          기간           | Repository                                                             | PR & Review                                                                   | github page                                                         |
| :-------------: | :---------------------: | ---------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| 영화 리뷰 1단계 | `23-03-17` - `23-03-27` | [Repo](https://github.com/nlom0218/javascript-movie-review/tree/step2) | [PR & Review](https://github.com/woowacourse/javascript-movie-review/pull/57) | [🎬 영화 리뷰](https://nlom0218.github.io/javascript-movie-review/) |

---

## 🚀 미션 회고

영화 리뷰의 2단계 미션 목표는 다음과 같다.

- 영화 상세정보 조회
  - API에서 제공하는 항목을 활용하여 상세 정보를 보여주는 모달 창을 구현한다.
  - 키보드의 ESC 키를 누르면 모달 창을 닫을 수 있는 등 사용성을 고려한다.
- 별점 매기기
  - 사용자는 영화에 대해 별점을 줄 수 있으며 새로고침하더라도 사용자가 남긴 별점은 유지되어야 한다.
  - 별점은 5개로 구성되어 있으며 한 개당 2점이며 1점 단위는 고려하지 않는다.
- UI⁄UX 개선하기
  - 영화 목록과 영화 상세 정보가 뜨는 모달창에 대한 반응형 레이아웃을 구성한다.
  - 영화 목록에서 더보기 버튼을 눌렀을 때 페이징하는 방식에서 무한 스크롤 방식으로 변경한다.

이중 기억에 남는 구현에 대해 글로 남기고자 한다.

### 📺 모달 창

모달 창을 구현하기 위해 `position: fixed;` css 속성을 사용하였다. 이는 저번 미션인 점심 뭐 먹지와 같은 방법이었다. 때문에 모달 창을 구현하는 것은 어렵지 않았다. css를 사용하여 모달 창을 만드는 것 뿐 아니라 html 태그를 사용하여 모달 창을 조금 더 쉽게 만들 수 있다는 것을 다른 크루들의 PR에서 알게 되었다. 해당 태그는 `dialog` 태그이다. 처음 보는 태그이기 때문에 MDN에서 이를 찾아보았다.

기본적으로 `dialog` 태그의 `open` 속성을 활용하여 모달 창을 열고 닫을 수 있다. 여는 방법은 `HTMLDialogElement`의 인스턴스 메서드인 `showModal`를 사용한다. 닫는 방법은 더욱 신박하다. `ESC`를 키를 누르면 자동으로 닫힌다. 이를 `dialog` 태그 없이 구현하려면 자바스크립트가 필요하다. 하지만 `dialog` 태그는 기본적으로 `ESC`키에 대한 반응을 제공하니 편리할 것 같다는 느낌이 든다. 모달 창을 닫는 또 다른 방법은 모달 창 내에서 `form` 태그를 이용할 때 편리하게 적용할 수 있다. `form` 태그의 `method` 속성을 `dialog`로 지정하고 `submit` 버튼을 만드면 된다. 이때 `dialog`의 `returnValue` 속성은 from를 제출할 때 사용한 button의 `value`로 설정된다.

이와 같은 `dialog`를 사용하면 보다 편리하게 모달 창을 만들 수 있을 것 같다. 또한 앞으로 리액트를 다루게 될 텐데, `dialog` 태그를 사용하여 모달 창 라이브러리를 직접 만들 수 있을 것만 같은 느낌이 든다. 단, 아직 `dialog` 태그는 IE 브라우저에서는 지원을 하지 않으므로 이 점은 유의해야 한다.

### 💽 별점 저장하기

별점은 로컬스토리지에 저장이 되어야 한다. 때문에 다음과 같은 객체를 만들어 별점을 저장하고 불러오는데 활용하였다.

```typescript
const handleLocalStorage = {
  getAllMovieStar: (): { -readonly [k in keyof ArrayLike<number>]: number } => {
    return JSON.parse(localStorage.getItem('movieStar') as string) || {};
  },

  getMovieStar: (movieId: number) => {
    return handleLocalStorage.getAllMovieStar()[movieId] || 0;
  },

  setMovieStar: (movieId: number, starCount: number) => {
    const movieStar = handleLocalStorage.getAllMovieStar();
    movieStar[movieId] = starCount;

    localStorage.setItem('movieStar', JSON.stringify(movieStar));
  },
};

export default handleLocalStorage;
```

처음엔 `Map` 객체를 로컬스토리지에 저장을 하려고 했다. 중복된 키를 가지지 않고 속성을 추가, 제거, 수정하기 편리했기 때문이다. 하지만 `Map` 객체에 `JSON.stringify` 메서드를 사용하게 되면 빈 객체가 반환된다. 이 때문에 `Map` 객체를 활용하지 못하였다. 이점이 아쉬웠다. 타입도 간결하게 `Map<number, number>`로 할 수 있었기 때문이다.

`Map` 객체를 활용하지 못한 아쉬움을 뒤로 하고 그 다음으로 어떻게 로컬스토리지에 영화 아이디와 별점을 저장할 수 있을지 고민을 하였다. 결국 number 인덱스 시그니처의 타입을 사용하기로 하였다. 그 이유는 어떤 영화 아이디가 올지 모르기 때문에(동적인 데이터이기 때문에) 인덱스 시그니처가 적절하기 때문이다.

추가적으로 키가 number인 인덱스 시그니처를 다룰 땐, Array, 튜플, ArrayLike와 같은 타입을 사용하면 된다. 접근에 초점을 둔다면 `ArrayLike`가 가장 올바른 타입이 된다. 단, `ArrayLike`은 `readonly` 속성이므로 자유롭게 속성을 추가, 제거, 수정하기 위해선 `readonly` 속성을 제거해야 한다.

먼저 다음은 모든 영화의 별점을 가져오는 메서드이다.

```typescript
const handleLocalStorage = {
  getAllMovieStar: (): { -readonly [k in keyof ArrayLike<number>]: number } => {
    return JSON.parse(localStorage.getItem('movieStar') as string) || {};
  },
};
```

타입을 보면 뭔가 신선? 이상?하다.. 내가 생각하여 만든 타입인데 `ArrayLike`의 키는 사용되고 있지만 값은 사용되고 있지 않다. 뭔가 어설프지만, 타입스크립트를 배우고 있는 과정에서 여러 고민을 했다는 것에 의의를 두고 싶다. `readonly` 속성을 제거하기 위해 매핑된 타입 앞에 `-readonly` 를 작성하였다.

다음은 특정 영화의 별점을 가져오고, 저장하는 메서드이다.

```typescript
const handleLocalStorage = {
  getMovieStar: (movieId: number) => {
    return handleLocalStorage.getAllMovieStar()[movieId] || 0;
  },

  setMovieStar: (movieId: number, starCount: number) => {
    const movieStar = handleLocalStorage.getAllMovieStar();
    movieStar[movieId] = starCount;

    localStorage.setItem('movieStar', JSON.stringify(movieStar));
  },
};
```

위 두개의 메서드는 크게 어려운 것 없다.

전체적으로 봤을 때 타입을 따로 선언하는 것이 좋을 것 같다는 생각을 하였다. 또한 메서드를 정의할 때 화살표 함수가 아니라 함수 선언식으로 작성하여 메서드 내부에서 객체에 접근할 때 `this` 키워드를 사용할 수 있도록 하면 좋을 것 같아 다음과 같이 리팩터링을 하였다.

```typescript
type LocalStorageMovieStar = {
  -readonly [k in keyof ArrayLike<number>]: number;
};

const handleLocalStorage = {
  getAllMovieStar: function (): LocalStorageMovieStar {
    return JSON.parse(localStorage.getItem('movieStar') as string) || {};
  },

  getMovieStar: function (movieId: number) {
    return this.getAllMovieStar()[movieId] || 0;
  },

  setMovieStar: function (movieId: number, starCount: number) {
    const movieStar = this.getAllMovieStar();
    movieStar[movieId] = starCount;

    localStorage.setItem('movieStar', JSON.stringify(movieStar));
  },
};

export default handleLocalStorage;
```

### ⭐️ 별점에 마우스 올리기

별점을 매기기 전, 다음과 같이 별점위에 마우스를 올리면 클릭을 하지 않아도 마우스를 올린 별점까지 모든 별들의 배경이 채워지는 것을 구현하고 싶었다.

![별점에 마우스 올리기](/image/Diary/WoowaCourse/Level1-movie.gif)

처음엔 별 하나하나에 이벤트를 추가하였다. 그 이후의 로직은 다음과 같다.

- 별의 data-set 어트리뷰트 값에 따라 별이 새롭게 그려진다.
- 별이 새롭게 그려지기 때문에 이벤트가 사라지기 때문에 다시 이벤트를 추가한다.

그 다음 과정만 봐도 비효율적이라고 느껴지지 않는가? 마우스를 움직일 때 마다 계속해서 새로운 이벤트를 추가해줘야 한다. 로직도 복잡해질 뿐더러 작동도 예상하는 데로 동작하지 않았다.

이를 해결하기 위해 이벤트 위임을 사용하였다.

별 아이콘을 묶는 컨테이너를 만들고 이벤트를 추가하였다. 추가한 이벤트는 다음과 같다.

```javascript
$votedStarContainer.addEventListener(
  'mousemove',
  this.hoverStarIcon.bind(this)
);
$votedStarContainer.addEventListener(
  'mouseleave',
  this.leaveStarIcon.bind(this)
);
$votedStarContainer.addEventListener('click', this.toggleVoteStar.bind(this));
```

마지막 이벤트는 클릭하는 이벤트이다. 나머지 두 이벤트가 별 아이콘을 묶는 컨테이너 위에서 마우스가 움직이거나 떠나갈 때 실행되는 이벤트이다.

좀더 자세히 살펴보자. 먼저 `mousemove` 이벤트에 대한 콜백함수이다.

```typescript
class VoteMovie {
  // ...
  hoverStarIcon(event: Event) {
    const star = event.target as HTMLImageElement;

    if (!star.matches('.vote-star')) return;

    const hoverStarCount = Number(star.dataset.starCount) as Score;
    this.insertStar(hoverStarCount);
  }
}
```

`vote-star`를 클래스로 가지는 노드의 `data-starCount` 어트리뷰트 값을 바탕으로 `insertStar` 메서드가 실행된다.

그 다음은 `mouseleave` 이벤트에 대한 콜백함수이다.

```typescript
class VoteMovie {
  // ...
  leaveStarIcon() {
    this.insertStar(this.score);
  }
}
```

현재의 점수 `this.score`를 바탕으로 `insertStar` 메서드가 실행된다. `insertStar` 메서드는 다음과 같다.

```typescript
class VoteMovie {
  // ...
  insertStar(hoverStarCount: number) {
    const $voteStarContainer = this._node.querySelector<HTMLDivElement>(
      '.voted-star-container'
    );

    if (!$voteStarContainer) return;

    const starHTML = this.createStarNodes(hoverStarCount);

    $voteStarContainer.innerHTML = '';
    $voteStarContainer.insertAdjacentHTML('beforeend', starHTML);
  }
}
```

`innerHTML` 프로퍼티를 사용하여 별점 컨터네이너 내부의 노드를 제거하고 `insertAdjacentHTML` 메서드를 사용하여 새로운 별점을 넣는다. 위와 같은 과정이 별점 컨테이너 내부에서 마우스가 움직이거나 밖으로 벗어날 때 실행된다. 물론 위와 같은 과정도 마우스가 움직일 때 마다 계속 실행되기 때문에 `innerHTML`, `insertAdjacentHTML` 가 계속 실행된다. 이는 효율적이라고 할 수 없는 부분이다.

---

## 📝 영화 리뷰에 대한 전체적인 회고

레벨1의 마지막 미션이 끝났다. 이전에는 구조에 대해 많은 신경을 썼다면 이번 미션에서는 사용자에게 친절한 서비스를 제공하는 것에 보다 더 신경을 썼다.

1단계에서는 더보기 버튼이 있었다면, 2단계에서는 더보기 버튼이 아니라 무한 스크롤로 영화를 추가 로드하였다. 또한 별점위에 마우스를 올림으로써 발생하는 이벤트, 반응형 디자인 등등... 이러한 기능들을 통해 UX에 대해 고민하는 시간을 가졌다.

API 통신을 통해 영화 정보를 가져와 서비스에 맞는 도메인을 만드는 과정에서 실제로 누군가와 통신하는 과정도 경험하였다. 해당 과정에서 불러온 영화 정보를 그대로 쓰는 것이 아니라 정보를 재가공하는 이유도 깨닫게 되었다. 짧게 말하면 '어떤 데이터가 들어와도 내 서비스에서 사용하는 데이터(엄밀히 말하면 객체의 속성)는 항상 동일하다!'이다.

API 통신이 실패 했을 때, 이에 대한 오류 메시지도 함께 구현하였다. 하지만 여기서 아쉬운 점은 클라이언트 에러, 서버 에러에 대한 각각의 오류 메시지가 아니라 모든 오류를 하나로 취급했다는 것이다. 다시 돌이켜 생각해보면 조금 더 부지런했다면 충분히 고려했을 만한 것이다. 반성 하자😑

지금까지 스켈레톤 UI에 대해 지금까지 생각해 본 적이 없었다. 유튜브의 첫 로딩이 스켈레톤 UI라는 것도 몰랐고 이도 하나의 기능이라는 것도 몰랐다. 지금까지 로딩이라고 하면 무한히 돌아가는 동그라미밖에 생각이 나지 않는다. 아직 완벽한 프론트 개발자가 될려면 멀었다.

내가 편하게 개발하는 환경은 중요하다. 하지만 내가 만든 서비스를 이용하는 사용자들의 편의성을 우선적으로 생각하는 마인드를 키우자.

---

## 👍 잘한 점

1. 타입에 대해 나름 고민을 하였다.
2. 웹 컴포넌트가 아니라 class로 만들기 위해 고민하였다.
3. eslint 설정을 스스로 해보았다.

## 👎 아쉬운 점

1. 구현을 하다 보니 MVC 패턴의 모습을 보인다.
2. 반응형 UI를 깔끔하게 하지 못했다.
3. any를 사용한 부분을 미처 리팩터링 때 발견하지 못했다.
4. 동기, 비동기에 대한 학습이 필요하다.
5. `Intersection Observer API`를 가져다 쓰기만 했다. 무슨 원리인지에 대한 학습이 부족하다.

---

## 👊 앞으로의 각오

1. 구현도 중요하지만 개념부터 잘 짚고 넘어가자.
2. 학습목표가 항상 중요함을 잊지 말자.

---

📅 2023-04-04
