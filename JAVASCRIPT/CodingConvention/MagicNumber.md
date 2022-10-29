# 매직 넘버는 상수화하여 관리하자

## 1. 개요

매직 넘버란 무엇이며 왜 매직 넘버를 상수화하여 관리하는지에 대해 살펴본다.

---

## 2. 매직 넘버란?

매직 넘버란 `0`, `1`, `Hello world`와 같이 의미를 한번에 알 수 없는 `숫자`나 `기호`를 의미한다.

예를 들어 아래와 같은 코드가 있다고 하자.

```javascript
function playGame(a, b) {
  window.alert("Hello world");

  let result;
  if (a > b) result = 1;
  else result = 0;

  return result;
}
```

위의 코드에서 `Hello world`가 어떤 의미를 담긴 메시지인지 바로 알 수 있는가? 또한 `0`과 `1`이 어떤 의미인지 한번에 알기란 어렵다. 이러한 `숫자`나 `문자열`를 매직 넘버라고 한다.

---

## 3. 매직 넘버 상수화하기

위의 예시에서 나타난 매직 넘버를 상수화를 시켜 코드를 작성해보자.

```javascript
const WIN = 1;
const LOSE = 0;
const WELCOME_MESSAGE = "Hello world";

function playGame(a, b) {
  window.alert(WELCOM_MESSAGE);

  let result;
  if (a > b) result = WIN;
  else result = LOSE;

  return result;
}
```

`스네이크 표기법(Snake case)`를 사용하여 매직 넘버를 상수화를 시켜 `1`, `0`, `Hello world`의 의미가 분명해졌다.

주의해야 할 점은 상수화를 시킨다고 해서 `1`를 `ONE`으로 이름을 짓는 의미없는 상수 변환은 피하도록 하자.

상수화된 상수들은 따로 파일을 만들어 저장하기도 한다.(예를들어, `constants.js`)

---

## 4. 매직 넘버를 상수화하는 이유

- 매직 넘버는 정확한 용도를 파악하기 어려워서 **유지보수가 어렵다**
- 매직 넘버 값이 바뀌었을 때 하나하나 찾아 변경해줘야 하기 때문에 안정성을 떨어뜨리게된다.
- 코드에서 상수로 선언되어 있지 않은 숫자, 문자열은 **무엇을 의미하는지 확신할 수 없다.**
- 무엇을 의미하는지 알 수 없기 때문에 코드의 흐름을 이해하기 위한 시간과 노력이 필요하다.
- 상수화를 통해 불분명한 값들이 이름을 가지게 되어 **어떠한 역할을 하는지 알 수 있게된다.**

---

## 5. Conclusion

> 강의를 듣다 보면 어떤 숫자를 상수(const)로 선언하는 경우가 종종 보였다. 그 당시엔 왜 번거롭게 상수로 선언을 하고 사용하는지 의문이 들었다. 하지만 이번 공부를 통해 매직 넘버를 상수화하는 과정이 코드의 의미를 분명하기 위함이라는 것을 알게되었고 앞으로 의미 없는 숫자와 문자는 상수화를 하여 의미를 부여하기로 하자.

---

## 참고

[[Code Review] NEXT_STEP 코드 리뷰 1주차 정리 및 회고](https://velog.io/@miot2j/Code-Review-NEXTSTEP-%EC%BD%94%EB%93%9C-%EB%A6%AC%EB%B7%B0-1%EC%A3%BC%EC%B0%A8-%EC%A0%95%EB%A6%AC-%EB%B0%8F-%ED%9B%84%EA%B8%B0)  
[1. 의미가 불분명한 매직 넘버를 상수로 선언하라.](https://javabom.tistory.com/28)  
[[ES2015] const로 상수 선언하기](https://www.daleseo.com/js-es2015-const/)

---

📅 2022-10-27
