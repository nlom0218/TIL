# 레벨2 다시, 점심 뭐 먹지 - step2

## 개요

|          미션           |        기간         |                             Repository                              |                            PR & Review                            |                        github page                         |
| :---------------------: | :-----------------: | :-----------------------------------------------------------------: | :---------------------------------------------------------------: | :--------------------------------------------------------: |
| 점심 뭐 먹지 미션 1단계 | 23-04-14 - 23-04-17 | [Repo](https://github.com/nlom0218/react-lunch/tree/nlom0218-step2) | [PR & Review](https://github.com/woowacourse/react-lunch/pull/66) | [🍚 점심 뭐 먹지](https://nlom0218.github.io/react-lunch/) |

## 미션회고

2단계 미션의 필수 요구 사항은 1단계와 동일하지만 프로그래밍 요구사항은 다음과 같이 수정되었다.

1. Step1의 Class Component를 `Function Component`로 마이그레이션 한다.
2. `Custom Hooks`을 이용하여 재사용 가능한 기능을 분리한다.

이에 따라 `Function Component`와 `Custom Hooks`에 초점을 두어 미션을 진행했다.

Function Component으로 마이그레이션 하는 과정은 크게 어렵지 않았다. 똑같은 작업을 반복하는 것이기 때문에 금방 완료할 수 있었다. 문제는 Custom Hooks을 만드는 것에 있었다. 일단 무엇부터 훅으로 만들어야 할지 감이 잡히지 않았다. 유틸은 아니지만 여기저기에서 재사용할 수 있는 그런 것... 상태 변화를 스스로 감지하여 새로운 무언가를 다시 리턴하는 무언가... 마치 레벨 1에서 도메인을 어떻게 나누어야 하는가? 이 역할은 누가 책임을 가져야 하는가? 와 같은 고민의 연속이었다. 더 많은 고민을 통해 결론을 도출해도 좋지만, 일단 연습이라는 생각으로 리스트를 필터링하는 부분을 훅으로 만들었다. 이 부분은 아래에서 자세히 다룬다.

2단계에서는 앱의 title과 favicon도 수정하였다. 실제로 배포되고 사용자가 있는 서비스는 아니지만, public내의 파일을 정리하면서 index.html의 내용도 서비스의 성격에 맞게 변경하였다. 나름? 괜찮아 보인다.

![점심 뭐 먹지의 title과 favicon](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F03o06%2FbtsbA5wX1x2%2FPB0RfN1lTh5gkrmSTR1aOK%2Fimg.png)

Class Component에서 Function Component으로 왜 바뀌었는지(이에 따른 장점은 무엇인지), Custom Hooks는 왜? 언제? 등과 같은 부분에선 아직 학습이 부족하다. 정리해야겠지?

### 📁 폴더 구조

폴더 구조가 대대적으로 바뀌었다고 해도 과언이 아니다. 이전의 폴더 구조와 바뀐 부분은 다음과 같다.

1. 개별적인 컴포넌트 폴더 생성
2. 스타일 컴포넌트 분리

![폴더 구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDSI5j%2FbtsbEmdlWet%2FMyfTPSOGC62iF6l4ywBl6k%2Fimg.png)

이와 같이 폴더 구조를 변경한 이유는 다음과 같다.

1. 컴포넌트 별로 폴더를 만들어 같은 주제에 관련한 파일을 한 곳에서 관리한다. (스타일과 앞으로 추가될 테스트 코드 등)
2. 리액트 컴포넌트에서 스타일에 관련된 코드를 제거한다. (스타일 컴포넌트를 맨 아래로 내리고 싶진 않았다.)

지금은 페이지가 없어 page에 관한 component가 없다. 때문에 현재 해당 역할을 restaurant 폴더가 하고 있다. 앞으로 page가 추가된다면 page 폴더 하위에 폴더(page이름)를 만들고 해당 폴더에서 page에서 사용되는 컴포넌트를 관리할 예정이다. components 폴더는 어떤 page에서든 재사용 가능한 컴포넌트를 모아두면 좋을 것 같다. 이렇게 되면 common 폴더는 필요 없을 것이다.

### ⭐️ Custom Hook

무엇을 훅으로 분리할까? 어떻게 하면 다른 컴포넌트에서도 유연하게 사용할 수 있을까? 하는 고민을 2단계 미션에서 가장 오랫동안 했다. 그래서 내린 결론은 `'당장은 훅으로 따로 분리할 것은 없다.'`였다. 만약 서비스가 컸더라면 분명 중복되는 것이 있었을 것이고 해당 기능을 훅으로 분리했을 것이다. 예를 들어, 모달창이 열려있는지 닫혀있는지, 필터링된 리스트를 보여주는 것 등등...

그래도 연습 겸 훅을 분리하였다. 점심 뭐 먹지에서 만든 훅은 `useListFiltering`이다. list(배열)과 필터링하는 객체를 받는다. 필터링하는 객체의 키는 필터링의 주제, 값은 필터링 함수이다. 해당 훅을 통해 어떤 리스트를 필터링할 수 있다.

#### ⬇️ 범용성이 떨어지는 useListFiltering

완성된 코드를 보기 전, 범용성이 떨어지는 `useListFiltering부터` 살펴보면 다음과 같다.

```typescript
type FilterFnCallbackFn = (_res: Restaurant[]) => Restaurant[]; // 음식점 목록을 반환하는 콜백만 받을 수 있다.
type FilteringFn = Record<string, FilterFnCallbackFn>; // 필터링 객체의 키값이 string이다.

const useFilteringList = (
  list: Restaurant[], // 매개변수로 음식점 목록 리스트만 받을 수 있다.
  filteringFn: FilteringFn
): [
  Restaurant[],
  (subject: string, filteringFn: FilterFnCallbackFn) => void
] => {
  const [_filteringFn, setFilteringFn] = useState(filteringFn);

  const filteringPipe = () => {
    return Object.values(_filteringFn).reduce((_list, fn) => fn(_list), list);
  };

  // subject가 string이기 때문에 잘못된 값을 받으면 예상하지 못한 결과를 받을 수 있다.
  const changeFilteringFn = (
    subject: string,
    filteringFn: FilterFnCallbackFn
  ) => {
    // ...
  };

  return [filteringPipe(), changeFilteringFn];
};

export default useFilteringList;
```

위 `useFilteringList` 훅은 2가지 문제를 가지고 있다.

1. 음식점 목록 리스트가 아닌 다른 리스트에 대해 필터링을 할 수 없다.
2. 음식점 목록 리스트를 반환하는 콜백만 받을 수 있다.
3. 필터링 함수의 키가 string이기 때문에 잘못된 subject를 넘겼을 때, 원하지 않는 결과가 나타날 수 있다.

결국 음식점 목록에만 한정되어 `useFilteringList`를 사용할 수 있다. 이렇게 되면 여러 컴포넌트에서 재사용할 수 있는 훅이 될 수 없다.

현재 점심 뭐 먹지 서비스에는 지금만으로도 충분하지만 범용성을 높여 어떠한 리스트가 들어와도 필터링된 리스트를 보여줄 수 있도록 개선하고 싶었다. 연습이니까 😇

#### ⬆️ 범용성을 높인 useFilteringList

범용성을 높이기 위해 타입스크립트의 제네릭을 사용하였다. 이 제네릭 때문에 주말의 절반이 날아갔다. 개선된 코드는 다음과 같다.

```typescript
import { useState } from 'react';

type FilterFnCallbackFn<T> = (arg: T) => T;
type FilterFn<K, U> = Record<keyof K, U>;

const useFilteringList = <
  List,
  CallBackFn extends FilterFnCallbackFn<List>,
  K extends FilterFn<K, CallBackFn>
>(
  list: List,
  filteringFn: FilterFn<K, CallBackFn>
): [
  List,
  (subject: keyof K, filteringFn: FilterFnCallbackFn<List>) => void
] => {
  const [_filteringFn, setFilteringFn] = useState(filteringFn);

  const filteringPipe = () => {
    return (Object.values(_filteringFn) as CallBackFn[]).reduce(
      (_list, fn) => fn(_list),
      list
    );
  };

  const changeFilteringFn = (
    subject: keyof K,
    filteringFn: FilterFnCallbackFn<List>
  ) => {
    // ...
  };

  return [filteringPipe(), changeFilteringFn];
};

export default useFilteringList;
```

중간에 단언문이 보이긴 하지만,, 일단은 정상 동작한다. List, CallBackFn, K와 같은 제네릭을 사용하여 어떤 리스트, 필터링 함수가 들어오더라도 유연하게 대처할 수 있게 되었다.

다음은 `useFilteringList` 훅의 간단한 예시이다.

```typescript
type Friend = { name: string; age: number; gender: string }[];

const friend = [
  { name: 'HD', age: 30, gender: '남자' },
  { name: 'KH', age: 28, gender: '여자' },
  { name: 'YB', age: 31, gender: '남자' },
  { name: 'GY', age: 32, gender: '남자' },
  { name: 'SJ', age: 29, gender: '여자' },
];

const friendFiltering = {
  gender: (list: Friend) => list.filter((item) => item.gender === '남자'),
  age: (list: Friend) => list.sort((a, b) => a.age - b.age),
};

const [list] = useFilteringList(friend, friendFiltering);
console.log(list);

// {name: 'HD', age: 30, gender: '남자'}
// {name: 'YB', age: 31, gender: '남자'}
// {name: 'GY', age: 32, gender: '남자'}
```

제네릭 사용에 익숙하지 않아 시간이 오래 걸렸다. 일단 돌아가게끔은 했지만 과연 잘 사용했는지 의문이 든다. 앞으로 훅을 만들 때, 제네릭을 사용하여 연습을 자주 해보자.

## 📚 1단계, 2단계 코드 리뷰 내용

### 리액트 컴포넌트와 스타일 컴포넌트

스타일 컴포넌트와 리액트 컴포넌트를 하나의 파일에 작성하게 되면 코드가 100줄이 넘는 경우가 생긴다. 이는 개인적인 판단으로 가도 좋다. 즉, 코드의 길이가 길어도 하나의 파일에서 작성하는 것이 좋으면 하나의 파일에 작성하고 긴 코드가 불편하다면 분리하여도 좋다.

이와 함께 더 생각할 부분은 사람들이 코드를 읽었을 때, 리액트 컴포넌트와 스타일 컴포넌트 중 어떤 컴포넌트를 먼저 보고 싶어 하는지이다. 아무래도 리액트 컴포넌트이지 않을까 싶다. 때문에 스타일 컴포넌트를 하단으로 옮기거나 파일을 분리하는 것이 좋다고 생각한다.

피드백을 받고 난 이후, 스타일 컴포넌트와 리액트 컴포넌트를 분리하였다. 각각의 관심사를 분리하였다. 스타일도 프론트를 다루는 입장에선 중요한 부분이기 때문에 무엇이 더 중요하다고 할 수 없다. 모두 다 중요하기 때문에 각각의 파일에서 관리하기로 했다. 추가로 리액트 컴포넌트와 스타일 컴포넌트를 구분하기 위해 스타일 컴포넌트를 불러오기 위해 다음과 같은 방법을 사용했다.

```typescript
import * as S from './style';
```

이런 방법으로 스타일 컴포넌트 앞에 `S.` 을 붙여 리액트 컴포넌트와 구분할 수 있다. 하지만 S 자체가 Style인지? State인지? 구분을 하는 것이 좋기 때문에 이에 대해선 조금 더 고민해야 한다.

### setState를 어떻게 자식 컴포넌트에 넘길까?

setState를 자식 컴포넌트에 넘기는 방법에도 정답은 없다. 하지만 나름의 기준은 만들 수 있다고 생각한다. 다음과 같은 두 가지 방법이 있다. 하나는 상태를 변경하는 setState를 자식 컴포넌트에 전달하는 방법이고 다른 하나는 setState를 활용한 함수를 자식 컴포넌트에 전달하는 방법이다.

```typescript
function App() {
  const [state, setState] = useState();

  const fn = () => {
    setState(something);
  };

  return (
    <>
      <Children someProp={setState} />
      <Children someProp={fn} />
    </>
  );
}
```

setState를 활용한 함수를 어디에서 사용하느냐에 따라 방법이 달라진다. 나는 보통의 경우 함수를 만들어 이를 자식 컴포넌트에 전달한다.

하지만 같은 setState를 사용하면서 여러 개의 자식 컴포넌트에서 사용하는 함수가 다르다면 어떻게 해야 할까? 위의 App 컴포넌트내에서 하나가 아닌 여러 개의 함수(같은 setState를 활용한 함수)를 만들어야 한다. 이렇게 된다면 하나의 컴포넌트에 코드의 양은 많아질 것이다. 이럴 땐 setState를 전달하여 자식 컴포넌트에서 setState를 제각기 활용하는 것이 좋아 보인다.

혹은 하나의 컴포넌트에서 관리하고 있는 상태가 많을 경우 setState를 활용하는 함수도 많아진다. 비슷한 함수가 많아질수록 유지보수성과 가독성은 떨어진다. 그리고 휴면에러도 나지 않을 것이라는 확신은 없다. 이와 같은 경우도 setState를 직접 자식 컴포넌트에 넘겨 자식 컴포넌트에서 setState를 활용한 함수를 만드는 것이 좋다.(레벨 2의 페이먼츠 미션을 이와 같은 방법으로 했다.)

다시 말하지만 선호도의 차이이다.

### CRA를 이용할 때,

Create React App은 리액트 프로젝트를 손쉽게 사용할 수 있다. CRA를 사용하여 리액트를 시작하면 기본적으로 생기는 파일이 있다. 만약 필요 없는 파일이라면 지워야 하는데, 무엇인지 모르고 지울 순 없다. 이중 생소한 파일은 무엇일까?

- manifest.json: 웹 애플리케이션의 정보를 JSON 텍스트 파일로 제공해서 웹 앱의 다운로드를 네이티브 앱과 유사한 형태로 제공할 수 있게 도와준다.
- robots.txt: 대부분의 웹 사이트의 소스 파일에 포함되어 있으며 웹 클롤러(검색 엔진 봇)와 같은 봇의 활동을 관리하기 위한 파일
- reportWebVitals.js: React의 성능 측정 도구
- setupTests.js: React Testring library를 사용하기 위한 파일

참고 사이트 - [[React] CRA 시작 파일 정리](https://myung-ho.tistory.com/101)

추가적으로 CRA에서는 대부분의 라이브러리가 dependencies에 존재한다. 이는 CRA는 모든 라이브러리를 번들된 이후 사용하기 때문에 dependencies와 devDependencies의 구별이 무의미하기 때문이다.

참고 사이트 - [CRA에서 모든 라이브러리가 dependencies에 들어가 있는 이유는?](https://zereight.tistory.com/984)

### <React.Fragment>와 <>

`<React.Fragment>`는 DOM에 별도의 노드를 추가하지 않고 여러 자식을 그룹화할 수 있다. 이를 단축하여 `<>`로 사용할 수 있다. 그렇다면 단축 문법인 `<>`만 사용하면 될까? 그것은 아니라고 한다. 보통의 경우엔 `<>`를 사용하겠지만, Fragments에 key가 있다면 `<React.Fragment>` 문법을 사용하는 것이 좋다. 그래야 key를 사용하여 매핑할 수 있으니까 말이다.

참고 사이트 - [리액트 공식 문서: Fragments 단축 문법](https://ko.legacy.reactjs.org/docs/fragments.html#short-syntax)

하나 더 추가! 리액트의 public/index.html를 보면 `noscript` 태그가 있다. 이는 자바스크립트를 지원하지 않는 브라우저에서는 앱이 동작하지 않을 때, 사용자에게 메시지를 전달하는 역할을 한다. 때문에 `noscript` 태그는 필요하다.

### 스타일 컴포넌트를 통해 공통 스타일 재사용하기

`공통 스타일`을 관리하기 위해 태그에 클래스를 추가하는 것이 아닌 스타일 컴포넌트의 `css` 메서드를 사용하여 스타일을 만들어 사용할 수 있다. 다음은 공통 스타일이다.

```typescript
export const textTitle = css`
  font-size: 20px;
  line-height: 24px;
  font-weight: 600;
`;

export const textSubTitle = css`
  font-size: 18px;
  line-height: 28px;
  font-weight: 600;
`;
```

이렇게 만들어진 textTitle, textSubTitle은 다음과 같이 활용가능하다.

```typescript
export const RestaurantDistance = styled.span`
  ${textTitle}
`;

export const RestaurantDescription = styled.p`
  ${textSubTitle}
`;
```

### 리액트에서 key에는 index 지양하기

렌더링 한 항목이 변하지 않을 것이라는 확신을 가지지 않는 한 index를 key로 사용하는 것이 좋지 않다. 즉, 항목의 순서가 바뀔 수 있는 경우 key에 인덱스를 사용하는 것은 좋지 않다. id와 값은 고유한 값을 key로 사용하자.

참고 사이트 - [리액트 공식 문서: 리스트와 Key](https://ko.legacy.reactjs.org/docs/lists-and-keys.html#gatsby-focus-wrapper)

### 가독성을 위해 제네릭에 type 선언을 하자.

다음은 useFilteringList 훅의 처음 제네릭 형태이다.

```typescript
const useFilteringList = <
  T,
  U extends (arg: T) => T,
  K extends Record<keyof K, U>
>(
  list: T,
  filteringFn: Record<keyof K, U>
): [T, (subject: keyof K, filteringFn: (arg: T) => T) => void] => {
  // ...
};
```

T, U, K와 같이 의미가 불분명한 네이밍으로 인해 가독성이 떨어진다. 이를 개선하기 위해 T, U, K가 아닌 해당 타입을 명시할 수 있도록 type 선언을 해야 하자.

## 👍 잘한 점

1. 스스로 훅을 구현하였다.
2. 제네릭이 두려웠지만 도전하였다.
3. 파일구조에 대해 많은 고민을 하였다.

## 👎 아쉬운 점

1. 무엇을 훅으로 만들어야 할지 아직 감이 잡히지 않는다.
2. 꼼꼼하게 코드를 리팩터링 하지 않았다.

## 👊 앞으로의 각오

1. 커스튬 훅의 기준을 잡아보자.
2. 코딩할 땐 코딩에만 집중하자.

---

📅 2023-04-21
