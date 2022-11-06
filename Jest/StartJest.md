# Jest 시작하기(설치 및 실행하기)

## 1. 개요

Jest를 설치하고 테스트를 하기 위한 환경 설정에 대해 정리한다.

---

## 2. Jest 설치하기

Jest는 `npm`, `yarn` 명령어를 통해 설치할 수 있다.

1. `npm`을 사용할 경우

   > $ npm install --save-dev jest

2. `yarn`을 사용할 경우

   > $ yarn add --dev jest

---

## 3. package.json 설정 변경하기

Jest를 설치하기만 해서 테스트를 실행할 수 있는 것은 아니다. `package.json` 파일에서 `scripts`를 변경해줘야 한다.

아래와 같이 변경한다.

```json
{
  "scripts": {
    "test": "jest"
  }
}
```

변경 후 `$ npm test` 또는 `$yarn test` 명령어를 통해 테스트를 실행할 수 있다.

특정 테스트만 하고 싶은 경우 해당 테스트가 포함되어 있는 파일명을 뒤에 붙여쓰면 된다.

> $ npm test my-test

---

## 4. Jest CLI를 사용하여 테스트 실행하기

Jest CLI를 사용하여 테스트를 실행할 수 있다. 이는 필수사항은 아니므로 반드시 해야하는 것은 아니다. 다만, 이러한 방법이 있다는 것을 정리함으로써 이후에 필요한 상황이 오면 활용할 수 있도록 준비하자.

먼저 jest를 전역으로 설치해야 한다.

> $ npm install jest --global

또는

> $ yarn global add jest

관리자 권한이 필요할 땐 `sudo`을 명령어 맨 앞에 추가하면 된다.

설치가 끝났으면 `jest` 명령어를 사용하여 테스트를 실행할 수 있다.

> $ jest

또한 `jest` 명령어 뒤에 특정 파일 이름을 적어 원하는 파일만 테스트를 진행할 수 있다.

> $ jest my-test

---

## 5. 테스트 실행하기

공식문서에서 설명하고 있는 예제 테스트이다. 먼저 간단한 `sum` 함수를 작성한다.

```javascript
// sum.js
sum = (a, b) => a + b;

module.exports = sum;
```

위 `sum` 함수를 테스트하기 위해서는 테스트를 위한 파일을 만들고 코드를 작성해야 한다. 테스트 파일 명은 `sum.test.js`이다.

```javascript
// sum.test.js
const sum = require("./sum");

test("숫자 더하기 테스트", () => {
  expect(sum(1, 2)).toBe(3);
});
```

여기까지 작성이 끝났다면 `$ npm test` 명령어을 통해 테스트 결과를 확인하면 된다.

![sum 함수 테스트 결과](/image/Jest/StartJest/sumTestResult.png)

---

## 6. 테스트 파일 관리 방법

### 6-1. tests 폴더 안에서 관리하기

첫 번째는 `tests` 폴더 안에서 관리하는 방법이다. 이런 바업은 테스트 파일만 한 곳으로 모아서 관리하기 때문에 전체적인 테스트 파일에 접근하기 유용하다.

디렉토리 구조 예시

```bash
├── __tests__
│   ├── sum.js
│   ├── minus.js
├── src
│   ├── index.js
│   ├── sum.js
├── package-lock.json
└── package.json
```

---

### 6-2. .test.js 확장자를 가진 파일로 관리하기

두 번째는 `파일명.test.js`로 관리하는 방법이다. 이 방법은 테스트 할 파일을 실제 코드와 같은 위치에서 관리하기 때문에 쉽고 빠르게 접근을 할 수 있다.

디렉토리 구조 예시

```bash

├── src
│   ├── index.js
│   ├── index.test.js
│   ├── sum.js
│   ├── sum.test.js
├── package-lock.json
└── package.json
```

---

## 7. Conclusion

> Jest를 사용하기 위한 세팅을 정리해보았다. 가장 중요한 것이 내가 원하는 환경을 세팅하는 것이라고 생각한다. 현재는 바벨, 타입스크립트에 대해서는 정리를 하지 않았지만 이후에 필요한 상황이 생기면 챕터를 하나 더 만들어 정리할 계획이다.  
> Jest로 테스트 코드를 작성할 때 불편한 점이 하나 있다. 자동완성이 안된다는 것이다. 여러 블로그들의 글을 보면 `@jest/types` 패키지를 설치하면 자동완성이 된다하던데, 나는 자동완성이 되지 않아 조금은 답답하였다. 해결책을 찾을 수 없어 정리를 못하였지만 알게된다면 추가적으로 작성을 해야겠다. 혹은,,, 타입스크립트를 사용해야만 가능한건지도 모르겠다.ㅠㅠ

---

참고

[Jest - 자바스크립트 유닛 테스트](https://velog.io/@akk0504/Jest-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9C%A0%EB%8B%9B-%ED%85%8C%EC%8A%A4%ED%8A%B8)  
[Jest공식홈페이지 - Getting Started](https://jestjs.io/docs/getting-started)

---

📅 2022-11-06
