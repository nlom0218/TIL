# 배열의 순서를 반전시키고 문자로 바꾸기(reverse, join)

## 1. 개요

이번 챕터에서는 배열 요소의 순서를 반전시키고 배열의 요소를 바탕으로 문자열을 만드는 방법에
대해 알아보도록 한다.

---

## 2. Array.reverse()

- **특징**
  - 원본 배열의 순서를 반대로 뒤집는다.
  - 원본 배열이 변경
- **반환 값**: 순서가 반전된 배열
- **구문**
  ```javascript
  arr.reverse();
  ```

매우 간단한 메서드이다. 매개변수도 없어 배열의 요소를 반전시킬 때 사용하면 된다. 하나의 예시를 통해 `Array.reverse()` 메서드를
사용해보자.

```javascript
const array = [1, 2, 3, 4];
const result = array.reverse();
```

![reverse 1](/image/JS/ArrayMethod/ReverseJoin/reverse1.png)

원본 배열이 변경되고 `Array.reverse()`의 반환값은 순서가 반전된 배열인 것을 확인할 수 있다.

---

## 3. Array.join()

- ## **특징**
  - 배열의 모든 요소를 문자열로 변환한 후, 인수로 전달받은 문자열로 연결
- **반환 값**: 배열의 모든 요소들을 연결한 하나의 문자열, 만약 `arr.length`가 0이라면, 빈 문자열을 반환
- **구문**
  ```javascript
  arr.join([separator]);
  ```
- **매개변수**
  - separator
    - 생략 가능하며 기본 구분자는 콤바(",")
    - 배열의 각 요소를 구분할 문자열을 지정한
    - 빈 문자열이면 모든 요소들이 사이에 아무 문자도 없이 연결

매개변수를 다양하게 활용하여 여러가지 예시를 살펴보자.

```javascript
const user = ["HD", "KH", "BY", "GY", "IC"];
const string1 = user.join();
const string2 = user.join("");
const string3 = user.join(", ");
const string4 = user.join("입니다. ");
```

![join 1](/image/JS/ArrayMethod/ReverseJoin/join1.png)

1. string1: 모든 요소를 ","으로 연결하여 문자열로 만든다.
2. string2: 모든 요소를 공백으로 연결하여 문자열로 만든다.
3. string3: 모든 요소를 ", "으로 연결하여 문자열로 만든다. string1과 다른 점은 ","뒤에 공백이 있다는 것이다.
4. string4: 모든 요소를 "입니다. "으로 연결하여 문자열로 만든다. 마지막 요소는 뒤에 연결하는 부분이 없기 때문에 "입니다. "가 붙지 않는다.

---

## 4. Conclusion

> 매운 간단한 두 가지 메서드에 대해 정리하였다. `Array.reverse()`는 많이 사용하지 않았지만 `Array.join()`은
> 프로젝트를 하면서, 코딩 테스트 문제를 풀면서 많이 다루었다. 그 만큼 사용이 많은 것 같다. 구분자에 따라 다양한 문자열로
> 만들 수 있기 때문에 잘 활용하도록 하자.

---

## 참고

[MDN - Array.prototype.reverse()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)  
[MDN - Array.prototype.join()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/join)

---

📅 2022-08-24
