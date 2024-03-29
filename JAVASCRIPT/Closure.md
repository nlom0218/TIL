# 클로저(closure)

## 1. 개요

클로저는 자바스크립트 고유의 개념이 아니다. 그렇다면 클로저라는 것은 무엇인가? MDN에는 아래와 같이 정의하고 있다.

> "A Closure is the combination of a function and the lexical environment within which that function was declared."

즉, 클로저는 `함수`와 그 함수가 선언된 `렉시컬 환경`과의 조합이다. 이를 중점으로 자바스크립트의 클로저를 알아보자.

---

## 2. 렉시컬 스코프

자바스크립트에서 함수의 상위 스코프는 함수가 호출된 위치가 아니라, `함수가 정의된 위치`에 따라 결정된다. 아래의 코드를 살펴보자.

```javascript
const x = 1;

function foo() {
  const x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // 1
bar(); // 1
```

`foo 함수`와 `bar 함수`는 모두 전역에서 생성된 함수이므로 모두 전역을 상위 스코프를 가진다. 때문에 `bar 함수`를 `foo 함수` 내부에서 호출을 한다 할지라도 변수 `x`는 전역 스코프에서 할당된 `1`를 가르키게 된다. 즉, 함수의 호출은 함수의 상위 스코프 결정에 어떠한 영향을 주지 못한다. 중요한건 `정의된 위치`다.

다시 정리하자면, 렉시컬 스코프란? 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정된다는 것을 의미한다.

---

## 3. 클로저와 렉시컬 환경

클로저의 정의를 살펴보면 `함수`와 그 함수가 선언된 `렉시컬 환경`과의 조합이라고 위에서 소개했다. 그렇다면 따지고 보면 자바스크립트의 모든 함수는 전역 스코프를 가지고 있기 때문에 모두 클로저라고 불릴 수 있다. 하지만 이는 잘못된 내용이다. 클로저는 좀더 특별환 상황에서 사용된다. 클로저는 다음을 만족해야 한다.

1. 중첩 함수가 상위 스코프의 식별자를 참조하고 있다.
2. 중첩 함수가 외부 함수보다 더 오래 유지되어야 한다.

### 3-2. 두번재 코드

```javascript
function foo() {
  const x = 1;

  function bar() {
    console.log(x);
  }

  bar();
}

foo(); // 1
```

중첩 함수 `bar`는 상위 스코프를 식별자를 참조하고 있지만 이번엔 `foo` 함수 보다 생명주기가 짧다.때문에 생명 주기가 종료된 외부 함수의 식별자를 참조할 수 있다는 클로저의 본질에 부합하지 않는다.

### 3-3. 세번째 코드

```javascript
function foo() {
  const x = 1;

  function bar() {
    console.log(x);
  }

  return bar;
}

const bar = foo(); // foo 함수의 생명주기가 끝난다.
bar(); // bar 함수의 생명주기가 끝난다.
```

이번엔 중첩 함수 `bar`가 상위 스코프의 식별자를 참조하고 외부 함수 `foo` 보다 생명 주기가 길다. 이때 `bar` 함수를 클로저라고 한다.

---

## 4. 일반적으로 함수는 기억력이 없습니다. 클로저는 어떻게 상태를 가질 수 있을까?

위의 코드를 다시 살펴보자.

```javascript
function foo() {
  const x = 1;

  function bar() {
    console.log(x);
  }

  return bar;
}

const barFn = foo(); // foo 함수의 생명주기가 끝난다.
bar(); // bar 함수의 생명주기가 끝난다.
```

외부 함수인 `foo`의 실행 컨텍스트는 중첩 함수인 `bar`의 실행 컨텍스트 보다 먼저 실행 컨텍스트 스택에서 제거 된다. 때문에 `foo` 함수의 렉시컬 환경 또한 사라질 것이라고 생각된다. 하지만 클로저를 사용할 경우 이는 잘못되었으며 이는 클로저가 상태를 가지고 있는 것과 관련된다. 그렇다면 어떻게 클로저는 외부 함수의 `foo`의 렉시컬 환경을 기억하고 있는 것일까? 이는 `bar` 함수가 정의될 때, 자신의 내부 슬롯 `[[Environment]]`에 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 `foo` 함수의 렉시컬 환경을 상위 스코프로서 저장하기 때문이다.

`foo` 함수의 렉시컬 환경은 `bar` 함수의 `[[Environment]]` 내부 슬롯에 참조되고 있고 `bar` 함수는 전역 변수 `barFn`에 의해 참조되고 있으므로 가비지 컬렉션의 대상이 되지 않는다. 가비지 컬렉터는 누군가가 참조하고 있는 메모리 공간을 함부로 해제하지 않는다. 이러한 이유로 클로저는 상태를 가질 수 있는 것이다.

---

## 5. 클로저의 활용

클로저의 활용은 아래와 같은 상황에서 빛을 발한다.

1. 상태를 안전하게 변경하고 유지하는 상황
2. 특정 함수에게만 상태 변경을 허용

아래의 코드를 살펴보자.

```javascript
function CounterMaker() {
  let num = 0;

  const counter = {
    increase() {
      num += 1;
      console.log(num);
    },

    decrease() {
      num -= 1;
      console.log(num);
    },
  };

  return counter;
}

const counter = new CounterMaker();

counter.increase(); // 1
counter.increase(); // 2
counter.increase(); // 3
counter.decrease(); // 2
counter.decrease(); // 1
```

`new CounterMaker()` 가 실행되면 `CounterMaker` 함수가 중첩 함수 `counter`를 반환하고 실행 컨텍스트에서 제거된다. 즉, `num` 상태를 외부에서 변경할 수 없게 된다. 또한 `counter` 객체에는 두 개의 메서드가 존재하는데, 이를 통해서만 `num` 상태를 조작할 수 있다.

---

## 6. Conclusion

> 모던 자바스크립트 Deep Dive 책을 보며 클로저에 대해 나름...정리해보았다. 정리하면서 느낀 점은 자바스크립트에 대한 기초 지식이 너무나 부족하다는 것을 깨닫게 되었다. 실행 컨텍스트, 렉시컬 환경, 생성자 함수와 같이 아직 모르는 개념이 너무 많다. 그래서 그런지 읽어도 이해는 커녕 제대로 이해를 하고 있는지 조차 의문이 들었다. 부족한 점을 알게되었으니 차근차근 계획을 세우고 공부를 해보자.

---

## 참고

도서 - 모던 자바스크립트 Deep Dive 24장 클로저
[MDN - 클로저](https://developer.mozilla.org/ko/docs/Web/JavaScript/Closures)
