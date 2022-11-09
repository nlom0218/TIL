# 의미 있는 이름 짓기(명명법)

## 1. 개요

누군가가 나의 변수명, 함수명을 보았을 때 변수명, 함수명만 보고도 의미를 파악을 하면 더할 나위 없이 좋을 것이다.
하지만 그러기에는 충분한 연습이 필요하다. 프로그래머들이 가장 많은 시간을 보내는 것이 바로 `이름 짓기`라고 하는 이유가 다 있다.

![이름 짓기의 어려움](https://i1.daumcdn.net/thumb/C264x200/?fname=https://blog.kakaocdn.net/dn/brapcH/btqzfGnjMOr/zAPAX87XnkRMps3i5za1jk/img.png)

최소한 무의미한 이름이 아닌 의미를 가지는 이름을 짓기 위한 방법에 대해 정리한다.

---

## 2. Nameing Conventions

먼저 토스와 Airbnb에서 설명하고 있는 기본적인 명명 규칙에 대해 살펴보자. 의미를 가지기 전 컨벤션을 먼저 지키는 것이 중요하기 때문이다.

---

### 2-1. 한 문자로 된 이름은 피하라

한 문자로 된 이름은 아무래도 의도를 나타내기엔 무리가 있다.

```javascript
// bad
const a = "any";

// good
const animal = "any";
```

---

### 2-2. 객체, 함수, 인스턴스에는 캐멀케이스를 사용하라

`캐멀케이스(camelCase)`는 각 단어의 첫문자를 대문자로 표기하고 붙여쓰지만 맨 처음 문자는 소문자로 표기하는 방법이다.

```javascript
const stundentData = {};
function getScore() {}
```

---

### 2-3. 클래스나 생성자에는 파스칼케이스를 사용하라

`파스칼케이스(pascalCase)`는 첫 단어를 대문자로 시작하는 표기법이다. 모든 문자는 붙여 쓰고 각 단어의 첫 문자는 대문자로 표기한다.

```javascript
function Car(options) {
  this.price = options.price;
}

class User {
  constructor(options) {
    this.name = options.name;
  }
}
```

---

### 2-4. 예약어는 사용하지 말아라

예약어란 이미 쓰임이 정해져 있는 키워드를 말한다. 그러므로 변수의 이름으론 적절하지 않다.

```javascript
// bad
let class;
let extends;
let super;
let const;
let export;
let true;
let false;
let boolean;
```

---

### 2-5. 상수는 영문 대문자 스네이크 표기법를 사용하라

`스네이크 표기법(snake case)`란 단어를 밑줄문자로 구분하는 표기법이다.

```javascript
const API_ADDRESS = "www.ex.com";
```

---

## 3. 이름은 길어도 된다

짧은 이름이 항상 좋은 것은 아니다. 의미를 정확히 나타낼 수 있으면 이름을 길게 작성하는 것이 좋다.

```javascript
// bad
function getUserName() {}
function getUserName() {}
function getUserName() {}

// good
function getUserNameFromInputFiled() {}
function getUserNameFromDB() {}
function getUserNameFromSession() {}
```

---

## 4. 복수일 때 복수를 의미하는 단어를 붙여 작성한다

배열엔 여러 요소가 있다. 즉, 복수형이다. 이때 `-s`보다는 `List`, `Array`등과 같은 복수를 의미하는 단어를 붙여 작성하는 편이 헷갈리지 않는다.

```javascript
// bad
const animals = ["dog", "cat", "lion"];

// good
const animalList = ["dog", "cat", "lion"];
const animalArray = ["dog", "cat", "lion"];
```

---

## 5. 중요한 단어는 앞에 쓴다

여러 단어가 있는 이름일 때 중요한 단어를 앞에 쓴다.

```javascript
// bad
const totalVisitor = "any";

// good
const visitorTotal = "any";
```

위는 하나의 예시이다. 상황에 따라 중요한 단어가 달라질 수 있다.

---

## 6. 함수 이름 짓기

함수 이름으로 해당 함수가 어떤 동작을 하는지 파악을 할 수 있으면 주석은 필요없다. 즉, 주석이 필요 없는 함수명이 좋다고 생각한다.

함수 이름만 보고도 어떤 기능을 하는지 힌트를 얻는 방법 중 하나는 적절한 접두사를 사용하는 것이다.

| 함수이름  | 뜻                                | ex             |
| --------- | --------------------------------- | -------------- |
| show...   | 무언가를 보여줌                   | showMessage    |
| get...    | 값을 반환함                       | getScore       |
| calc...   | 무언가를 계산함                   | calcScore      |
| create... | 무언가를 생성함                   | createPost     |
| check...  | 무언가를 확인하고 불린값을 반환함 | checkUserInput |
| set...    | 무언가를 설정함                   | setAnswer      |

추가적으로 `boolean`을 반환하는 함수의 접두사론 위에서 설명한 `check` 뿐 아니라 `is`, `has`, `can` 등도 사용된다.

그리고 함수 이름을 지을 때 주의해야 할 점은 함수의 이름이 예를들어 `showMessage`이라면 정말 말 그대로 해당 함수는 메세지를 보여주는 역할만 해야한다는 것이다. 메세지를 생성하거나 바꾸는 것은 바른 함수에서 해야 한다.

---

## 7. 이름 짓기 Tip

1. 의도를 명확히 밝혀라
   - 의도가 없는 이름은 피하라.
   - `result`도 어떤 `result`인지 밝혀라.
2. 그릇된 정보를 피하자
   - 협업을 할 때 동료들에게 잘못된 정보를 주지마라.
   - `items`과 같은 포괄적인 단어도 그릇된 정보를 줄 수 있다.
3. 의미 있게 구분하자
   - `Data`와 `Info`, `a`, `the`와 같은 불용어는 되도록 피하자.
4. 발음하기 쉬운 이름을 짓자
   - 간장공장 공장장과 같은 이름을 짓는다면 누가 읽고 싶어하겠느냐
   - 커뮤니케이션을 고려햐여 이름을 짓자
5. 검색하기 쉬운 이름을 사용하자
   - 상위 범주를 이름에 포함하자.
   - 범주화하여 이름을 작성하자.
6. 한 개념에 한 단어를 사용하자.
   - `add`라는 단어가 포함된 이름을 지을 때, `add`의 의미를 하나로 정하자. 다른 의미를 가지는 변수는 다른 단어를 사용하여 이름을 짓자

---

## 8. Conclusion

> 코드를 작성하면 변수의 이름을 짓는 것은 반드시 필요하다. 어떤 이름이 나중에 다시 코드를 봐도 명확히 알 수 있는지, 누군가와 협업을 할 때 혼동을 줄 수 없는지를 항상 생각을 하면서 이름을 지어야 한다. 또한 전체 코드에서 통일성을 가진 이름을 지어야 코드의 흐름이 한 눈에 들어올 것이다. 이러한 이유에서 변수의 이름을 짓는 것은 중요하다. 위의 내용을 정리하면서 느낀 점은 지금까지 변수의 이름을 지으면서 지켰던 것도 있었지만 간과했던 것도 있다고 생각한다. 특히 `data`, `info`를 너무 남발하여 사용했다. 이런 불용어를 반드시 사용하지 말아야 하는 것은 아니지만 의미와 의도를 더 명확히 만들어 이름을 지어야 겠다.  
> 결국 이름 짓는 것은 습관이라고 생각한다. 습관이 잘못 길들여지면 후에 고생을 한다. 지금 가지고 있는 나쁜 습관을 버리고 좋은 습관을 가지도록 노력하자. 그리고 위의 내용이 아직 추상적으로 다가온다. 구체적으로 어떻게 해야 하는지 느낌이 오지 않는다. 많이 고민하여 이유있는 변수명을 작성하도록 하자.

---

## 참고

[Airbnb JavaScript 스타일 가이드()](https://github.com/parksb/javascript-style-guide#%EB%AA%85%EB%AA%85%EA%B7%9C%EC%B9%99-naming-conventions)  
[토스 - 코딩 컨벤션 명명 규칙](https://ui.toast.com/fe-guide/ko_CODING-CONVENTION#%EB%AA%85%EB%AA%85-%EA%B7%9C%EC%B9%99)  
[네이밍 컨벤션과 변수이름 짓기](https://velog.io/@humonnom/%EB%84%A4%EC%9D%B4%EB%B0%8D-%EC%BB%A8%EB%B2%A4%EC%85%98%EA%B3%BC-%EB%B3%80%EC%88%98%EC%9D%B4%EB%A6%84-%EC%A7%93%EA%B8%B0)  
[자바스크립트 함수 명명의 중요성 (특히 이름 짓기)](https://readyt0g0.tistory.com/entry/%ED%95%A8%EC%88%98-%EB%AA%85%EB%AA%85%EC%9D%98-%EC%A4%91%EC%9A%94%EC%84%B1-%ED%8A%B9%ED%9E%88-%EC%9D%B4%EB%A6%84-%EC%A7%93%EA%B8%B0)  
[2. 변수명과 함수명 짓기](https://dkje.github.io/2020/08/03/CleanCodeSeries2-copy/)

---

📅 2022-11-08
