# 형변화하기

## 1. 개요

문자열에서 숫자로 또는 그 반대로 형변화를 할 때 어떤 방법을 할지 정리한다.

---

## 2. 구문의 선두에서 형을 강제한다.

먼저 형변환이 필료한 경우 구문의 가장 처음 부분에서 형변환을 진행한다.

---

## 3. 숫자에서 문자열로 형변화하기

숫자에서 문자열로 형변화가 필요한 경우 `String()` 메서드를 사용한다.

```javascript
const score = 10;

// bad
const totalScore = new String(score);

// bad
const totalScore = score + '';

// bad
const totalScore = score.toString();

// good
const totalScore = String(score);
```

---

## 4. 문자열에서 숫자로 형변화하기

문자열에서 숫자로 형변화가 필요한 경우 `Number()` 메서드를 사용한다. `parseInt()` 메서드를 사용하는 경우 기수를 인자로 넘겨 사용한다.

`parseInt()`인 경우 숫자가 `0x` 또는 `0X` 문자로 시작하는 경우엔 16진수로 취급된다.

```javascript
const value = '3';

// bad
const numValue = new Number(value);

// bad
const numValue = +value;

// bad
const numValue = value >> 0;

// bad
const numValue = parseInt(value);

// good
const numValue = Number(value);

// good
const numValue = parseInt(value, 10);
```

---

## 4. Conclusion

> 이번 주제는 형변환이다. 문자열에서 숫자로 변환할 때, 숫자에서 문자열로 변화할 때 어떻게 하는지에 대해 Airbnb 코딩 컨벤션을 바탕으로 정리를 하였다. 내용은 정말 심플하다. 그렇다면 이를 정리한 이유는 무엇일까? 나의 습관에 대해 돌아볼 수 있었기 때문이다.  
> 과거의 난 문자열에서 숫자로 바꾸기 위해 parseInt() 메서드를 사용했다. 코딩테스트 공부를 하면서 parseInt() 메서드보다 Number() 메서드를 사용하는 것이 성능이 더 좋다는 것을 알고 난 뒤 Number() 메서드를 사용하고 있다. 반대로 숫자에서 문자열로 바꾸기 위해서는 "+ ''"을 사용하였다. 이러한 방법도 문자열로 바꿀 수 있지만 앞서 학습한 내용을 바탕으로 앞으론 String() 메서드를 사용해야겠다.

---

## 참고

[Airbnb JavaScript 스타일 가이드 - 형변환과 강제 (Type Casting & Coercion)](https://github.com/parksb/javascript-style-guide#%ED%98%95%EB%B3%80%ED%99%98%EA%B3%BC-%EA%B0%95%EC%A0%9C-type-casting--coercion)
