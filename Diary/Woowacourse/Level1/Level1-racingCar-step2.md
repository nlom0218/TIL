# 레벨1 자동차 경주 - step2

## 개요

|       미션        |          기간           | Repository                                                                     | PR & Review                                                                 |
| :---------------: | :---------------------: | ------------------------------------------------------------------------------ | --------------------------------------------------------------------------- |
| 자동차 경주 1단계 | `23-02-10` - `23-02-13` | [Repo](https://github.com/nlom0218/javascript-racingcar-1/tree/nlom0218-step2) | [PR & Review](https://github.com/woowacourse/javascript-racingcar/pull/242) |

---

## 🚀 미션 회고

첫번째 미션의 2단계는 추가된 기능 구현이 없고 공통 피드백 및 코드 리뷰를 바탕으로 코드를 리팩터링하는 것으로 진행하였다. 아마 우테코의 시스템에 적응하는 온보딩 미션이라 간단했다고 생각한다. 두번째 미션부터는 step2가 무려 주말 포함 9일이니 과연 어떤 어려움이 있을까 두근거린다.

## 🛠️ 리팩터링

- feat: 자동차 이름이 공백일 경우 에러 반환하기
- feat: 자동차의 최종 위치를 구하기
- feat: 자동차의 이동를 결정하는 숫자 배열 만들기
- refactor: 도메인 로직과 UI 로직 폴더 분리하기
- refactor: ES6 모듈 적용하기
- refactor: 유효성 검사 메서드 이름 수정하기
  - 이름을 통해 해당 메서드가 하는 역할을 알 수 있도록 한다.
- refactor: 자동차 게임을 관리하는 객체 만들기
  - 해당 객체에서 자동차 객체를 관리한다.
  - 해당 객체에서 자동차 게임의 전반적인 역할을 담당한다.
- refactor: 자동차 객체 생성을 GameManager에서 실행하기
- refactor: GameManager에 시도횟수 저장하기
- refactor: 자동차의 이동을 GameManager에서 실행하기
- refactor: 자동차의 상태를 가져오기 위한 함수를 GameManager에서 실행하기
- refactor: 자동차 경주 게임의 우승자를 가져오기 위한 함수를 GameMananger에서 실행하기
- refactor: 우승자의 자동차 이름을 반환하는 함수를 만들어 출력하기
- refactor: 명령형 프로그래밍을 선언형 프로그래밍으로 수정하기
- refactor: 매직 넘버 상수화하기
- refactor: generateRandomNumber 재사용성 높이기

위와 같은 목록을 바탕으로 리팩터링과 추가 기능 구현을 진행했다. 리팩토링 내용 중 몇가지를 정리한다.

### 유효성 검사 메서드 이름 수정하기

```javascript
// 기존 코드

const Validator = {
  // ...

  isCarNameGreaterThanFive(names) {
    return names.some((name) => name.length > 5);
  },

  isCarNameLowerCase(names) {
    return names.every((name) => {
      return name.search(/[^a-z]/g) === -1;
    });
  },

  // ....
};
```

위의 메서드의 이름을 보면 `is~`라는 접두사 붙여있다. 이는 `~이다`라는 것으로 직역이 된다. 즉, `isCarNameGreaterThanFive`은 '자동차의 이름이 5보다 크다'라고 직역을 할 수 있다.

또한 메서드 이름에 특정 숫자와 조건이 들어가게 되어 유연하지 않다는 피드백을 받게 되었다. 즉, 코드의 내용이 바뀌면 메서드의 이름도 수정되어야 한다. 5보다 큰다는 것은 무엇을 의미하는지 생각을 할 필요가 있다.

해당 메서드 이름이 과연 좋은지? 코드 리뷰를 통해 의문을 품게되었고 보다 좋은 이름을 생각을 하면서 리팩터링을 진행했다.

```javascript
// 리팩터링 후 코드

const Validator = {
  // ...

  isValidCarNameLength(names) {
    return names.every((name) => name.length <= 5);
  },

  isValidCarNameCharacter(names) {
    return names.every((name) => {
      return name.search(/[^a-z]/g) === -1;
    });
  },

  // ....
};
```

`is~` 접두사 뒤에 바로 `Valid`를 붙여 해당 메서드가 검사의 역할을 한다는 추측을 충분히 할 수 있도록 리팩터링을 하였다.

그리고 `isValidCarNameLength`의 코드를 보자. 자동차 이름의 길이가 5랑 같거나 작다는 것은 자동차 이름의 길이가 유효한지를 판단하는 기준이 된다. 자동차 이름의 길이와 관련된 코드이므로 `CarNameLength`을 붙여 메서드 이름을 정했다.

이러한 메서드 이름은 자동차의 길이의 요구 사항이 바뀌어도 수정하지 않아도 되므로 유연하다고 할 수 있다. 단지, 내부의 코드만 바꾸면 된다!

### 자동차 경주 게임을 관리하는 객체 생성

이전 코드를 보면 자동차 객체에는 `getWinner` 메서드가 있었다. 이 메서드는 자동차들을 요소로 가지는 배열을 인자로 받은 후 가장 멀리 이동한 자동차의 이름을 반환하는 역할을 한다. 이러한 역할은 과연 자동차 객체와 어울리는 메서드일까? 아니라고 생각한다. 우승자를 찾는 역할은 자동차 객체가 아니라 자동차 경주 게임을 관리하는 객체에서 더 어울린다고 생각하여 이를 위해 자동차 경주 게임을 관리하는 객체를 생성했다.

객체의 이름은 `GameManager`이다.

`GameManger`에서 우승자를 찾는 것 뿐 아니라, 아래의 역할을 가지게 된다.

- 자동차 생성 및 추가
- 이동 횟수 저장
- 자동차 이동 시키기
  - 요소가 숫자(랜덤)인 배열 생성
- 자동차의 현재 위치 및 이름 가져오기
- 우승자 찾기
- 가장 멀리 이동안 자동차의 위치 찾기

협력하는 관계를 생각하여 역할을 객체에게 적절히 분배하자!

### generateRandomNumber 재사용성 높이기

```javascript
// 기존 코드

const generateRandomNumber = {
  generator() {
    return Math.floor(Math.random() * RANDOM_NUMBER_RANGE);
  },
};
```

기존 코드를 먼저 살펴보자. 어떤 문제가 있을까?

`generateRandomNumber` 객체의 `generator` 메서드는 언제 어디서든 1 ~ 9사이의 숫자만 반환을 한다. 이게 무엇이 문제일까? 만약 자동차의 이동 조건이 0 ~ 13 사이의 숫자 중 6 이상일 경우로 바뀌게 된다면 새로운 함수를 작성하거나 상수를 수정해야 한다. 이러한 방법보다 `generator`에 인자를 전달하여 범위를 설정하는 것이 `generator` 메서드의 재사용성을 높이는 방법이다.

```javascript
// 리팩터링 후 코드
const generateRandomNumber = {
  generator(minNumber, maxNumber) {
    return minNumber + Math.floor(Math.random() * (maxNumber - minNumber + 1));
  },
};
```

`minNumber`와 `maxNumber`를 인자로 전달하여 해당 범위 내의 숫자를 반환하도록 코드를 리팩터링하였다. 이러한 방법을 통해 요구 사항이 바뀌었더라도 해당 메서드를 사용하는 곳에서 인자만 쓰-윽 바꾸기만 하면 된다!

---

## 👍 잘한 점

1. 다른 미션 보다 짧은 step2였지만 시간 내에 코드 리뷰 요청을 마무리 하였다.
2. 코드 리뷰 내용, 공통 피드백 내용을 최대한 반영하도록 노력하였다.

---

## 👎 아쉬운 점

1. 촉박하게 step2를 진행한 듯 하여 아쉽다.
2. 코드 리뷰, 피드백에 대한 내용 공유를 적극적으로 하지 않았다.
3. 리뷰어 피드백을 늦게 작성하였다.

---

## 👊 앞으로의 각오

1. 고민할 부분이 있으면 함께 나누기(함께 성장하기)
2. 여유를 가지고 미션 진행하기
3. 항상 네이밍에 신경쓰기

---

📅 2023-02-14
