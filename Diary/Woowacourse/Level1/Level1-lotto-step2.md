# 레벨1 행운의 로또 - step2

## 개요

|       미션        |          기간           | Repository                                                                 | PR & Review                                                             | github page                                                      |
| :---------------: | :---------------------: | -------------------------------------------------------------------------- | ----------------------------------------------------------------------- | ---------------------------------------------------------------- |
| 행운의 로또 2단계 | `23-02-17` - `23-02-27` | [Repo](https://github.com/nlom0218/javascript-lotto-1/tree/nlom0218-step2) | [PR & Review](https://github.com/woowacourse/javascript-lotto/pull/230) | [🎱 행운의 로또](https://nlom0218.github.io/javascript-lotto-1/) |

---

## 🖼️ 도메인 로직은 그대로, UI는 변경

프리코스의 미션부터 지금까지의 미션은 콘솔에서 화면을 띄우고 콘솔에서 입력값과 출력값을 다루는 것이었다. 이번 행운의 로또 2단계에선 콘솔이 아니라 웹에 직접 화면을 그리는 미션이었다.

단, 1단계에서 작성하였던 도메인 로직은 최대한 수정하지 않으면서 UI를 변경할 수 있는 로직을 추가해야 했다. 때문에 먼저 도메인 로직을 아예 건들지 않으면서 미션을 수행하였다. 해당 과정을 거치면서 로또를 관리하는 객체가 더 필요하다는 것을 느끼게 되었다.

```javascript
  // LottoWebGame.js
  buyLottos(purchaseAmount) {
    this.lottos = [];

    new Array(purchaseAmount / LOTTO.price).fill().forEach(() => {
      this.lottos.push(this.publishLotto());
    });
  }

  publishLotto() {
    return new Lotto(this.generateLottoNumbers());
  }

  generateLottoNumbers() {
    const lottoNumbers = [];
    while (lottoNumbers.length < LOTTO.numbersLength) {
      const number = generateRandomNumber(LOTTO.minNumber, LOTTO.maxNumber);
      if (!lottoNumbers.includes(number)) lottoNumbers.push(number);
    }

    return lottoNumbers.sort((a, b) => a - b);
  }
```

위 코드는 로또를 사는 과정이 담긴 코드이다. 이 역할을 담당하는 객체가 있었다면 `LottoWebGame` 객체는 더욱 깔끔하게 도메인과 UI를 연결하는 역할로만 존재할 수 있다고 생각하였다. 때문에 2단계에서는 `LottoMachine` 객체를 만들어 로또를 사는 역할과 로또들의 번호, 순위를 계산하고 넘기는 역할을 부여했다. 이로써 `LottoWebGame` 객체는 단순히 도메인의 정보를 UI에 전달하는 역할과 UI에서 넘겨받은 정보를 바탕으로 도메인 로직이 실행될 수 있게 되었다.

---

## 📝 HTML 다루기

평소 나의 안 좋은 습관은 `div` 태그만 사용한다는 것이다. 물론 입력이 필요한 부분은 당연히 `form`, `input` 태그를 사용하지만 전체적으로 `div` 태그를 사용하는 빈도가 높다. 이것이 좋지 않은 습관이라는 것은 알지만 내용에 알맞는 태그를 찾아 작성하는 것에 귀찮음을 느껴 다양한 태그를 활용하지 않았다. 이번 미션에는 `div` 태그 뿐 아니라 다양한 태그를 사용했는데, 이 또한 다른 크루들보다 적극적으로 활용하지 못해 아쉬움이 많이 남는다.

앞으로 html 태그를 다루는 일이 많이 있을텐데, 시맨틱 태그를 적극적으로 사용해보자. 시맨틱 태그란? 의미가 있는 태그를 의미하는데, 이를 사용하면 가독성이 높아지고 웹 접근성도 높아질 뿐 더러 검색엔진이 html내의 태그를 분석할 수 있는 장점이 있다.

시맨틱 태그의 종류와 설명

| 태그        | 설명                                                           |
| ----------- | -------------------------------------------------------------- |
| `<header>`  | 사이트의 머리부분에 사용                                       |
| `<main>`    | 메인 콘텐츠를 나타내는데 사용                                  |
| `<section>` | 제목별로 나눌 수 있는 문서의 콘텐츠 영역을 구성하는 요소       |
| `<article>` | 개별 콘텐츠를 나타내는 요소                                    |
| `<aside>`   | 좌우측의 사이드 영역                                           |
| `<footer>`  | 사이트의 바닥부분, 주로 연락처나 제작자 정보등을 기술하는 부분 |
| `<hgroup>`  | 제목과 부제목을 묶어서 나타내는 요소                           |
| `<nav>`     | 웹 페이지 메뉴를 만들 때 사용                                  |

---

## 🎨 CSS 다루기

css를 다루기 위해선 html의 요소를 가져와야 한다. 여기서 보통 `id` 또는 `class`를 사용하게 된다. 단, 어떠한 요소에서 접근할 수 있는 것이라면 굳이 `id`나 `class`를 사용하지 않고 접근한다.

```css
#purchase-lotto ul {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}

#purchase-lotto li {
  display: flex;
  margin: 2px 0px;
  align-items: center;
}
```

위의 예시에서 `ul`과 `li`는 `id`가 purchase-lotto인 태그 내에 존재하기 때문에 `#purchase-lotto ul`, `#purchase-lotto li`를 사용하여 접근할 수 있다. 평소에 이런 접근을 많이 선호한다. `id`와 `class`를 무분별하게 부여하는 것 보다 더 깔끔하고 명료하다고 생각하기 때문이다. js에서 노드를 가져올 때도 마찬가지이다.

최소한의 `id`와 `class`만 사용하여 html에 접근하는 방법이 괜찮은 것인지는 아직 확실한 정답을 내리지 못한 상태이다. 앞으로 계속 고민해야 하는 부분이고 나의 기준을 적립하는 과정이 필요하다고 생각한다. 다른 크루들의 의견은 어떤지 궁금하다.

> 방금 도밥을 통해 css 스타일을 위해 id 선택자를 사용하면 좋지 않는 이유에 대해 설명을 들었다!ㅎㅎ 속시원한 느낌:) 조만간 해당 내용을 정리해보자.

---

## 👿 태그의 기본 속성이 제거되는 문제!

바로 위에서 모든 html 태그에 `id`, `class`를 부여하지 않으면서 태그에 접근을 하였다고 했다. 이런 이유로 인해 배포하는 과정에서 문제가 생겼다. 배포하는 과정에서 웹팩으로 인해 태그의 기본 속성이 삭제되었기 때문에 몇몇 css와 js에서 작성한 동작이 실행되지 않았던 것이었다.

예를 들어, 아래와 같은 css 코드가 있다고 생각해보자.

```css
#purchase-lotto input[type='text'] {
  color: white;
  font-size: 20px;
}
```

위의 css는 `pruchase-lotto`라는 id를 가진 태그 내의 `input`의 타입이 `text`인 요소를 가져와 작업을 한다. 하지만 배포를 하고 나니 `type="text"`는 `input`의 기본값이었기 때문에 삭제가 되어 위의 css속성이 적용이 되지 않았다.

이러한 점 때문에, 태그의 속성값을 바탕으로 요소를 가져오는 것이 옳바른 것인지에 의문을 품게되었다. html이 조금은 복잡해지더라도 html 태그에 `id`, `class`가 있으면 확실한 요소 선택이 가능하기 때문에 css, js에서는 보다 수월하게 가져올 수 있고 가독성도 뛰어날 수 있다는 생각을 하게 되었다.

해결방법은 아래와 같이 두가지가 있다.

1. 웹팩 설정 변경
2. `id`, `class`를 사용하여 css와 js에서 해당 요소에 접근

이 중 내가 선택한 방법은 웹팩의 설정을 변경하는 것이었다.

```javascript
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      template: './index.html',
      minify: {
        removeRedundantAttributes: false,
      },
    }),
  ],
```

웹팩의 플로그인 중 `HtmlWebpackPlugin`의 `minfy`에서 `removeRedundantAttributes`를 false로 바꾸었다. 아직 웹팩을 잘 다루지 못하여 이를 해결하기 위해 솔로스타의 도움을 받았다. 정답을 바로 알려주는 것이 아니라 함께 해답을 찾아가는 과정이었기 때문에 나에게 큰 도움이 되었다.

1차 리팩터링을 받은 후, `id`를 사용하여 요소에 접근을 할 수 있도록 수정하였다. 아무리 봐도 웹팩의 수정 보다는 `id`, `class`를 적극 사용하는 것이 옳다고 생각한다. 필요한 상황에 따른 현명한 선택을 내리도록 하자.

---

## 👍 잘한 점

1. 도메인 로직 변경을 최소화 하기 위해 노력하였다.
2. html 랜더링에 대해 고민하였다.(코드 리뷰에 이에 대한 자세한 내용이 있음)

---

## 👎 아쉬운 점

1. 원하는 만큼 리팩터링을 하지 못했다.
2. css 선택에 대해서 기준이 확실치 않다.
3. id와 class 사용에 대해 머리속이 혼잡하다.
4. 글을 어떻게 써야 하는지 아직 잘 모르겠다.
5. 글이 중구난방인 느낌이 든다.
6. 점점 초조함을 느낀다.

---

## 👊 앞으로의 각오

1. 초조함을 느끼지 말자.
2. 내 할 것을 정하고 이를 달성하기 위해 노력하자.
3. 양이 중요하지 않다.

---

📅 2023-02-27
