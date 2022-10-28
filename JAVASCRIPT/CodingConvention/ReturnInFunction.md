# 함수 내에서 반환은 적으면 적을수록 좋다

## 1. 개요

가독성이 좋은 코드를 작성하기 위해 함수 내에서 `return(반환)`을 사용하는 방법에 대해 살펴본다.

## 2. 함수 내에서 반환은 한 번만 한다.

조건에 따라 특정 값을 반환하는 함수인 경우, 함수 맨 마지막에서 한 번만 반환한다.

먼저 나쁜 예시부터 살펴보자.

```javascript
// Bad
function getResult() {
  if (conditionA) {
    // ...
    return reaultA;
  } else if (conditionB) {
    // ...
    return resultB;
  } else {
    // ..,
    return resultC;
  }
}
```

`getResult()` 함수 내에서 `return`이 3번 보인다. 조건에 따라 각기 다른 값을 반환하기 때문이다. 위의 코드로 작성하여도 큰 문제가 없지만 보다 가독성이 좋은 코드로 수정해보자.

```javascript
// Good
function getResult() {
  let result;
  if (conditionA) {
    // ...
    result = reaultA;
  } else if (conditionB) {
    // ...
    result = resultB;
  } else {
    // ...
    result = resultC;
  }

  return result;
}
```

위 코드에서는 함수 내에서 `return`를 단 한 번만 사용하여 값을 반환한다. `return`를 단 한 먼만 사용하여 얻을 수 있는 이점은 아래와 같다.

- 코드를 예측하기 쉬워진다.
- 흐름이 간결한 함수를 작성할 수 있다.

---

## 3. 예외로 빠져나가는 경우

예외상황으로 인해 함수를 빠져나가야 하는 경우 `return`를 사용하여 빠져나간다.

반드시 함수 내에서 `return`이 한 번만 사용하여 값을 반환해야 하는 것은 아니다. 예외상황은 언제든지 존재할 수 있다. 이럴 땐 두 번 이상 `return`를 사용할 수 있다.

> 사실 `return`이 얼마나 있든 잘못된 코드는 아니다. 단, 가독성, 코드의 예측, 간결한 흐름 측면에서 적으면 적을수록 좋다는 것이다.

```javascript
// Allow
function getResult() {
  if (exception) {
    return exceptionResult;
  }

  //...

  return result;
}
```

---

## 4. `return`문 바로 위는 한 칸 비워 놓는다.

```javascript
// Bad
function foo() {
  bar();
  return;
}

// Good
function foo() {
  bar();

  return;
}
```

다른 명령과 `return`문이 붙어있으면 가독성이 좋지 않으므로 `return`문 전에 한 줄 띄운다.

---

## 5. Conclusion

> 함수 내에서의 `return`을 사용할 때에도 컨벤션이 있다는 사실에 놀라웠다. 지금까지는 조건에 따라 많은 `return`를 사용하면서 코드를 작성하였다. 나의 방법이 틀린 것은 아니지만 다른 누군가가 코드를 읽었을 때를 생각하면 가독성은 좋지 않을 것이라는 생각이 든다. 가독성이 좋은 코드를 작성하는 것이 거창한 과정이 아니라 사소한 배려부터 시작이라는 것을 항상 생각하며 내 기준이 아닌 동료를 기준으로 작성하도록 하자.

---

## 참고

[Toss 코딩 컨벤션 - 반환하기](https://ui.toast.com/fe-guide/ko_CODING-CONVENTION#%EB%B0%98%ED%99%98%ED%95%98%EA%B8%B0)  
[padding-line-between-statements](https://eslint.org/docs/latest/rules/padding-line-between-statements)

---

📅 2022-10-28
