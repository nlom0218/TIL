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

---

## 참고

[MDN - 정규 표현식](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Regular_Expressions#javascript%EC%97%90%EC%84%9C_%EC%A0%95%EA%B7%9C_%ED%91%9C%ED%98%84%EC%8B%9D_%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)

---

📅 2023-01-28
