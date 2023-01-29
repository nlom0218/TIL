# 정규 표현식과 함께 사용하는 String의 메서드

## 0. 요약

| 메서드       | 설명                                                                              |
| ------------ | --------------------------------------------------------------------------------- |
| match()      | 문자열이 정규식과 매치되는 부분을 검색한다.                                       |
| matchAll()   | 문자열 중 정규식과 매치되는 모든 부분을 담은 배열을 반환합니다.                   |
| search()     | 문자열에서 일치하는 부분을 탐색합니다.                                            |
| replace()    | 문자열에서 일치하는 부분을 탐색하고, 그 부분을 대체 문자열로 바꿉니다.            |
| replaceAll() | 문자열에서 일치하는 부분을 모두 탐색하고, 모두 대체 문자열로 바꿉니다.            |
| split()      | 정규 표현식 또는 문자열 리터럴을 사용해서 문자열을 부분 문자열의 배열로 나눕니다. |

---

## 1. 개요

지난 번엔 `RegExp`의 메서드에 살펴보았다. 이번엔 문자열 `String`의 메서드 중 정규 표현식과 함께 사용할 수 있는 메서드들에 대해 살펴보자.

---

## 2. match()

`match()` 메서드는 문자열이 정규식과 매치되는 부분을 검색한다. `g` 전역 플래그가 있지 않을 경우 `RegExp.exec()` 메서드와 같은 결과를 반환하고 `g` 전역 플래그가 있는 경우 모든 결과를 담은 배열을 반환한다. 두 경우 모두 일치되는 부분이 없을 경우 `null`를 반환한다.

### 2-1. match() 메서드 사용 예시1

```javascript
const regExp = /wo{2}/; // "woo"와 매칭
const string = 'wowoowoo';

string.match(regExp); // ['woo', index: 2, input: 'wowoowoo', groups: undefined]
```

`RegExp.exec()` 메서드와 같은 형식의 객체를 반환한다.

### 2-2. match() 메서드 사용 예시2

이번엔 매칭되는 결과가 없는 경우를 살펴보자.

```javascript
const regExp = /wo{3}/; // "wooo"와 매칭
const string = 'wowoowoo';

string.match(regExp); // null
```

`wowoowoo` 문자열에서 눈을 씻고 찾아봐서 `wooo`는 보이지 않는다. 때문에 `null`를 반환한다.

### 2-3. match() 메서드 사용 예시3

이번엔 `g` 전역 플래그와 함께 사용되는 경우를 살펴보자.

```javascript
const regExp = /[ad]\w{2}/g;
const string = 'What are you doing? i drink water';

string.match(regExp); // ['are', 'doi', 'dri', 'ate']
```

정규 표현식 부터 살펴보자. `/[ad]\w{2}/g`는 아래를 조건을 만족하는 문자열을 찾는다. `g` 전역 플래그를 사용하고 있기 때문에 결과는 복수일 수도 있다.

- [ad]: `a` 또는 `d`(1개)
- \w{2}: 밑줄을 포함한 영문자(2개)

문자열에서 해당 조건을 만족하는 문자열은 `are`, `doi`, `dri`, `ate`이므로 4개의 문자열이 배열에 담겨져 반환된다.

---

## 3. matchAll()

`matchAll()` 메서드는 정규 표현식과 매칭되는 문자열을 모두 반환한다. 반환되는 값은 `iterator`이다. 그러므로 순회가능하다. `g` 플래그와 함께 사용해야 하면 그렇지 않은 경우 타입 에러가 발생한다.

### 3-1. matchAll() 메서드 사용 예시

```javascript
const regExp = /[ad]\w{2}/g;
const string = 'What are you doing? i drink water';

string.matchAll(regExp); // RegExpStringIterator {}

const result = [...string.matchAll(regExp)];
```

`result` 객체에는 4개의 요소가 존재한다. 각각 정규 표현식에 매칭된 결과가 담겨져 있다.

- ['are', index: 5, input: 'What are you doing? i drink water', groups: undefined]
- ['doi', index: 13, input: 'What are you doing? i drink water', groups: undefined]
- ['dri', index: 22, input: 'What are you doing? i drink water', groups: undefined]
- ['ate', index: 29, input: 'What are you doing? i drink water', groups: undefined]

---

## 4. search()

`search()` 메서드는 정규 표현식과 문자열간에 같은 것을 찾기 위한 검색을 실행한다. 첫번째로 매칭되는 것의 인덱스를 반환한다. 만약 찾지 못하면 -1를 반환한다.

해당 메서드의 인자로 들어가는 정규 표현식에 `g` 플래그를 사용한다 하더라도 매칭되는 모든 것들의 인덱스를 찾지 않고 첫번째로 매칭 되는 것의 인덱스만 찾아 반환한다.

### 4-1. search() 메서드 사용 예시1

```javascript
const regExp = /wo{2}/;
const string = 'wowoowooo';

string.search(regExp); // 2
```

정규 표현식과 매칭되는 문자열은 `woo`이므로 `w`의 인덱스인 `2`를 반환한다.

### 4-2. search() 메서드 사용 예시2

```javascript
const regExp = /zo{2}/g;
const string = 'wowoowooo';

string.search(regExp); // -1
```

정규 표현식과 매칭되는 것이 없으니 `-1`를 반환한다.

---

## 5. replace()

`replace()` 메서드는 어떤 패턴에 일치하는 일부 또는 모든 부분이 교체된 새로운 문자열을 반환한다. 해당 메서드는 두 개의 인자를 받는다.

- 첫번째 인자: 문자열 또는 `정규 표현식`
- 두번째 인자: 교체할 문자열

즉, 첫번째 인자를 두번째 인자로 교체한다.

`g` 플래그를 사용할 경우 정규 표현식과 매칭되는 모든 것들을 교체하고 그렇지 않을 경우 첫번째로 매칭되는 것만 교체한다.

### 5-1. replace() 메서드 사용 예시1

```javascript
const regExp = /wo{2}/;
const string = 'wowoowooo';

string.replace(regExp, 'zoo'); // 'wozoowooo'
```

`woo`가 `zoo`로 교체된다.

### 5-2. replace() 메서드 사용 예시2

```javascript
const regExp = /cat/gi;
const string = 'I like cat. Cat is very cute';

string.replace(regExp, 'dog'); // 'I like dog. dog is very cute'
```

`cat`이 `dog`로 교체한다. 단, `g` 플래그를 사용하였기에 모든 `cat`이 교체되고 `i` 플래그를 사용하였기에 대소문자를 구별하지 않는다.

---

## 6. replaceAll()

`replaceAll()` 메서드는 `replace()` 메서드와 마찬가지로 첫번째 인자로 받은 문자열 또는 정규 표현식과 매칭되는 것들을 두번째 인자로 교체한다. 단 `All`이 붙인 것과 같이 매칭되는 모든 것들을 교체한다. 단, `replaceAll()` 메서드에 정규 표현식을 사용한다면 반드시 `g` 플래그를 반드시 붙여야 한다. 그렇지 않으면 에러가 발생한다.

`replaceAll()`메서드는 `replace()` 메서드의 첫번째 정규 표현식에 `g` 플래그를 사용한 것과 같은 결과를 반환한다. 하지만 메서드의 이름을 보다 직관적이고 가독성 높은 코드를 작성하기 위해 단 하나만 교체할 경우엔 `replace()` 메서드를 모두 교체할 경우에는 `replaceAll()` 메서드를 사용하도록 하자.

### 6-1. replaceAll() 메서드 사용 예시

```javascript
const regExp = /x/g;
const string = 'xoxoxoxoxoxo';

string.replaceAll(regExp, 'i'); // ioioioioioio
```

`x`가 `i`로 교체된다. 사실 위와 같은 경우는 `/x/g`로 하지 않고 `x`로만 해도 괜찮다.

---

## 7. split()

`split()` 메서드는 문자열을 지정한 구분자를 이용하여 여러 개의 문자열로 나눈다. 해당 메서드는 두개의 인자를 받는다.

- 첫번째 인자: `separator`로 문자열 또는 정규 표현식. 이를 기준으로 문자열리 분리된다.
- 두번째 인자: `limit`로 끊어진 문자열의 최대 개수를 나타내는 정수이다. 생략가능하다.

주어진 문자열을 `separator` 마다 끊은 부분 문자열을 담은 배열을 반환한다.

### 7-1. split() 메서드 사용 예시1

```javascript
const regExp = /x/;
const string = 'oxoxoxoxoxo';

string.split(regExp); //  ['o', 'o', 'o', 'o', 'o', 'o']
```

`x`를 기준으로 `string`를 나눈다. 나누어진 각각의 문자열이 요소가 되어 위와 같은 결과를 반환한다.

### 7-2. split() 메서드 사용 예시2

```javascript
const regExp = /[^a-zA-Z]/;
const string = 'Hello my name is hd';

string.split(regExp, 3); // ['Hello', 'my', 'name']
```

영어 대소문자가 아닌 문자를 기준으로 `string`를 나눈다. 하지만 `split()` 메서드의 두번재 인자인 `limit`가 3이므로 3개의 요소만 배열에 담겨서 반환된다.

---

## 8. Conclusion

> 평소에 `split()` 메서드는 자주 사용한다. 문자열을 자르고 이를 배열로 바꾸어야 하는 것이 코딩 테스트에서 자주 사용되기 때문이다. 하지만 정규 표현식을 기준으로 자르진 않았다. 나머지 메서드들은 사용해본적은 있지만 자주 사용하지 않아 매번 필요한 상황이 올 때 마다 검색하여 찾곤했다.  
> `matchAll()` 메서드는 이터러블한 객체를 반환하기 때문에 이전에 공부한 이터러블이 많이 도움이 되었다. 역시 배우면 배울수록 더 많은 것을 얻어가는 것 같다.

---

## 참고

[MDN - 정규 표현식](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Regular_Expressions#javascript%EC%97%90%EC%84%9C_%EC%A0%95%EA%B7%9C_%ED%91%9C%ED%98%84%EC%8B%9D_%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)  
[MDN - String.prototype.match()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/match)  
[MDN - String.prototype.matchAll()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll)  
[MDN - String.prototype.search()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/search)  
[MDN - String.prototype.replace()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/replace)  
[MDN - String.prototype.replaceAll()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll)  
[MDN - String.prototype.split()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/search)

---

📅 2023-01-29
