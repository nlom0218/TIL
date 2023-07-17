# 레벨2 다시, 점심 뭐 먹지 - step1

## 개요

|          미션           |        기간         |                             Repository                              |                            PR & Review                            |                        github page                         |
| :---------------------: | :-----------------: | :-----------------------------------------------------------------: | :---------------------------------------------------------------: | :--------------------------------------------------------: |
| 점심 뭐 먹지 미션 1단계 | 23-04-11 - 23-04-13 | [Repo](https://github.com/nlom0218/react-lunch/tree/nlom0218-step1) | [PR & Review](https://github.com/woowacourse/react-lunch/pull/29) | [🍚 점심 뭐 먹지](https://nlom0218.github.io/react-lunch/) |

## 🚀 미션 회고

레벨 2의 첫 번째 미션은 레벨 1에서 이미 했었던 `점심 뭐 먹지` 미션이다. 자바스크립트로 구현을 했던 레벨 1의 점심 뭐 먹지 미션을 이번에는 리액트로 구현을 해야 했다. 점심 뭐 먹지 미션은 다음과 같은 두 단계로 이루어져 있다.

1.  Class Component로 구현
2.  Function Component로 구현

이중 Class Component로 점심 뭐 먹지의 1단계를 진행하였다. React를 몇 번 다루어본 적이 있지만 Class Component를 직접 다루었던 적은 없었기 때문에 기대반 걱정반의 감정을 가지며 미션을 시작했다.

미션의 필수 요구 사항은 다음과 같다.

- 레벨 1을 참고하여 REQUIREMENTS.md에 요구 사항을 도출한다.
- Class Component를 사용한다.
- 초기 로딩 시 이전 값이 Local Storage에 존재한다면 초기 값으로 적용한다.
- mockData.json의 데이터를 이용하여, 초기 데이터를 셋팅한다.
- 최소 각 카테고리 별로 3개 이상의 음식점을 mockData에 등록한다.
- 카테고리별로 정렬하거나 이름순, 거리순으로 정렬할 수 있게 렌더링 한다.

레벨 1에서 다루었던 음식점 추가, 삭제, 자주 가는 음식점 등록 기능은 없다. 보다 리액트를 체험하며 기본적인 개념을 습득하는 것에 집중을 하기 위해 노력했다. 하지만 스타일 컴포넌트, 타입스크립트에 많은 시간을 쏟았다. 타입스크립트는 중요한 개념이기 때문에 시간을 많이 쏟는 것은 괜찮다고 생각한다. 하지만 스타일 컴포넌트 같은 외부 라이브러리에 집착을 한 점이 아쉽게 느껴졌다. 스타일 컴포넌트는 이후에 많이 다룰 수 있으니 현재는 리액트의 구조, 기본적인 개념에 더욱 집중을 하자.

eslint 설정에서도 어려움을 겪었다. react와 관련된 eslint를 설치하고 설정을 진행하였다. 순조롭진 않았지만 설정이 완성되었다고 생각하는 순간, 리액트 컴포넌트가 Function Component가 아니라는 eslint error를 마주하게 되었다. 이를 해결하는 것도 하나의 방법이지만 더 이상 eslint에 시간을 쏟지 않기로 했다. 때문에 eslint를 걷어내고 미션의 기능 구현을 시작했다. 2단계에서는 다시 eslint를 설정할 생각이다.

모달 컴포넌트의 구조에 대한 아쉬움이 남아있다. 단지 레스토랑의 정보를 위한 모달 컴포넌트이기 때문에 다른 정보를 담는 모달 컴포넌트로서의 역할을 하지 못한다. 재사용이 편하게 이를 2단계에서 개선해 볼 생각이다.

### 📂 폴더 구조

![폴더 구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fo8FNE%2FbtsaevRdJpm%2FKBqai9Xv1KQnTIywasLKP1%2Fimg.png)

앞으로 계속 고민해봐야 할 부분 중 하나가 폴더 구조라고 생각한다. 프로젝트가 커지면 커질수록 어떤 폴더 구조를 가지느냐에 따라 관리하기 수월해질 수 있고 어려워질 수 있기 때문이다. 실제로 티처캔 폴더 구조를 살펴보면 엉망 그 자체라 할 수 있다. 때문에 이번 기회에 좋은 폴더 구조에 대해 생각하고 이를 실제 티처캔에도 적용해 볼 생각이다.

components 폴더의 common 폴더는 모든 컴포넌트에서 공통으로 필요한 컴포넌트를 넣어준다. 그리고 components 폴더에는 common 폴더뿐 아니라 restaurant 폴더가 있는데 이는 page마다 하나씩 가지는 폴더라고 생각하면 된다. 현재 미션은 단일 페이지로 이루어져 있기 때문에 \`/\`가 home이다. 그래서 home으로 폴더 이름을 정할 수도 있었지만 조금 더 의미를 부여하기 위해 restaurant으로 하였다.

현재 모든 컴포넌트에서 styled-components를 사용하고 있는다. styled-components를 따로 분리를 해보는 것을 2단계에서 시도해보고자 한다.

constants, domain, style, utils는 역할에 맡게 나누기 위해 페어와 함께 고민을 했다.

### 🪄 상수를 타입으로 만들기

음식점의 카테고리는  "전체, 한식, 중식, 일식, 양식, 아시안, 기타"이다. 이는 외부 값에 의해 변경되지 않는 값이다. 때문에 상수로 관리를 해야 한다. 때문에 다음과 같이 상수화를 하였다.

```typescript
export const CATEGORIES = {
  ALL: '전체',
  KOREAN: '한식',
  CHINESE: '중식',
  JAPANESE: '일식',
  WESTERN: '양식',
  ASIAN: '아시안',
  ETC: '기타',
} as const;
```

이렇게 만든 상수 CATEGORIES를 통해 카테고리의 타입을 만들어 사용했다. 즉, 카테고리를 위한 타입을 따로 만드는 것이 아니라 이미 만들어진 상수를 활용하여 타입을 만들었다.

```typescript
// Bad
type Categories =
  | '전체'
  | '한식'
  | '중식'
  | '일식'
  | '양식'
  | '아시안'
  | '기타';

// Good
type Categories = (typeof CATEGORIES)[keyof typeof CATEGORIES];
```

이와 같은 사용의 장점이라면 카테고리가 추가된다 하더라도 타입을 수정하지 않아도 된다는 것이다.

### 🚚 타입 좁히기

아래의 내용은 미션을 제출하고 리팩터링한 것이다.

SelectBox는 재사용 가능한 컴포넌트이다. SelectBox 컴포넌트는 options이라는 props를 받는다. 때문에 options에 타입을 정해줘야 하는 상황이 생긴다. 처음에는 string\[\]으로 해주었다. 하지만 타입을 더 좁히고 싶었다.

```typescript
type Props = {
  options: string[];
};
```

카테고리의 options에는 전체, 한식, 중식... 이 있고 정렬의 options에는 이름순, 거리순이 있다. 이러한 options를 유니온을 만들었다.  어떠한 options이 넘어올지 모르기 때문에 제네릭을 사용했다.

```typescript
type Props<T> = {
  options: T[];
};

class SelectBox<T extends string> extends Component<Props<T>> {
  // ...
}
```

options는 string이어야 하기 때문에 `extends string`이 필요하다.

## 👬 페어프로그래밍 진행 과정

레벨 2의 첫 번째 페어는 파인이다. 파인과 함께 폴더 구조를 생각하고 어떻게 컴포넌트를 나누어야 할지 이야기를 하였다. 이전의 나의 나쁜 습관이라고 하면 이야기 없이 바로 코드부터 작성하는 것이다. 하지만 이번에는 그러지 않았다. 먼저 그림을 통해 내가 생각하는 구조를 설명하고 파인의 의견을 들으며 서로의 생각을 맞춰나갔다.

타입스크립트를 공부하면서 타입스크립트를 잘 쓰고 싶다는 생각이 계속 머릿속에 맴돌고 있는 상태이다. 아직 타입에 대해 익숙하지 않기 때문에 타입에 대한 나의 생각을 파인에게 전달하는 것이 힘들었다. 내가 제대로 설명을 하지 못하니 파인도 많은 혼란을 가졌을 것이다. 이러한 점이 이번 페어프로그래밍에서 아쉬운 점이다.

목요일 제출날이 되고, 파인이 모달 컴포넌트에 대한 아쉬움을 말했다. 재사용을 할 수 없는 모달 컴포넌트이기 때문에 이를 개선하고 싶다는 것이었다. 나도 동의를 했지만 곧 제출 시간이 다가오기 때문에 미리 배포를 해야 하고 PR, 피드백을 작성해야 했기 때문에 이를 2단계에서 하기로 했다. 스타일 컴포넌트의 ThemeProvider에 시간을 할애하지 않았더라면 충분히 할 수 있는 시간이 있었을 것이다. ThemeProvider도 내가 하자고 해서 했지만 결국은 하지 않은 기능이다. 내 욕심이 너무 컸던 페어프로그래밍이었다. 학습 목표와 관련 없는 것엔 욕심을 가지지 말자. 페어프로그래밍 자체에 집중해 보자.

## 👍 잘한 점

1. 타입의 재사용에 대해 많은 고민을 했다.

2. 폴더 구조, 컴포넌트 구조에 대해 페어와 생각을 맞춰나갔다.

## 👎 아쉬운 점

1. eslint 설정을 중간에 포기했다.

2. 모달 컴포넌트를 재사용할 수 없다.

3. 학습 목표에 벗어난 것에 집중을 하여 시간을 낭비했다.

---

📅 2023-04-14