# 숫자 문자열과 영단어

## 1. 개요

- 프로그래머스
- Lv.1
- 2021 카카오 채용연계형 인턴십
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/81301)

---

## 2. 문제 설명

![문제 설명](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d31cb063-4025-4412-8cbc-6ac6909cf93e/img1.png)

네오와 프로도가 숫자놀이를 하고 있습니다. 네오가 프로도에게 숫자를 건넬 때 일부 자릿수를 영단어로 바꾼 카드를 건네주면 프로도는 원래 숫자를 찾는 게임입니다.

다음은 숫자의 일부 자릿수를 영단어로 바꾸는 예시입니다.

- 1478 → "one4seveneight"
- 234567 → "23four5six7"
- 10203 → "1zerotwozero3"

이렇게 숫자의 일부 자릿수가 영단어로 바뀌어졌거나, 혹은 바뀌지 않고 그대로인 문자열 s가 매개변수로 주어집니다. s가 의미하는 원래 숫자를 return 하도록 solution 함수를 완성해주세요.

참고로 각 숫자에 대응되는 영단어는 다음 표와 같습니다.

| 숫자 | 영단어 |
| ---- | ------ |
| 0    | zero   |
| 1    | one    |
| 2    | two    |
| 3    | three  |
| 4    | four   |
| 5    | five   |
| 6    | six    |
| 7    | seven  |
| 8    | eight  |
| 9    | nine   |

---

### 2-1. 문제 설명 - 제한사항

- 1 ≤ s의 길이 ≤ 50
- s가 "zero" 또는 "0"으로 시작하는 경우는 주어지지 않습니다.
- return 값이 1 이상 2,000,000,000 이하의 정수가 되는 올바른 입력만 s로 주어집니다.

---

### 2-2. 문제 설명 - 입출력 예

| s                  | result |
| ------------------ | ------ |
| "one4seveneight"   | 1478   |
| "23four5six7"      | 234567 |
| "2three45sixseven" | 234567 |
| "123"              | 123    |

---

### 2-3. 입출력 예 설명

입출력 예 #1  
문제 예시와 같습니다.

입출력 예 #2  
문제 예시와 같습니다.

입출력 예 #3

- "three"는 3, "six"는 6, "seven"은 7에 대응되기 때문에 정답은 입출력 예 #2와 같은 234567이 됩니다.
- 입출력 예 #2와 #3과 같이 같은 정답을 가리키는 문자열이 여러 가지가 나올 수 있습니다.

입출력 예 #4  
s에는 영단어로 바뀐 부분이 없습니다.

---

## 3. 문제 풀이

```javascript
function solution(s) {
  return parseInt(
    s
      .replace(/zero/g, 0)
      .replace(/one/g, 1)
      .replace(/two/g, 2)
      .replace(/three/g, 3)
      .replace(/four/g, 4)
      .replace(/five/g, 5)
      .replace(/six/g, 6)
      .replace(/seven/g, 7)
      .replace(/eight/g, 8)
      .replace(/nine/g, 9)
  );
}
```

`String.replace()` 메서드는 첫 번째 인수로 주어진 패턴에 일치하는 일부 또는 모든 부분이
두 번째 인수로 교체되어 새로운 문자를 반환한다.

첫 번째 인수인 정규식 표현을 살펴보면 숫자의 영문자가 적혀있고 `g` Flag를 사용하고 있다. `g` Flag는
대상 문자열내에 모든 패턴들을 검색하는 것을 의미한다. 즉, 정규식에 해당하는 표현을 전부 찾아 두 번째 인수 교체
하는 것이다.

마지막으로 최종 반환값은 숫자이므로 `parseInt()`를 사용한다.

---

### 결과

![programmers_numeric_strings_en_words_result](/image/CodingTest/programmers_numeric_strings_en_words/programmers_numeric_strings_en_words_result1.png)

---

## 4. 다른 사람 풀이

가장 많은 좋아요와 유사한 풀이 그리고 댓글이 달린 풀이이다.

```javascript
function solution(s) {
  let numbers = [
    "zero",
    "one",
    "two",
    "three",
    "four",
    "five",
    "six",
    "seven",
    "eight",
    "nine",
  ];
  var answer = s;

  for (let i = 0; i < numbers.length; i++) {
    let arr = answer.split(numbers[i]);
    answer = arr.join(i);
  }

  return Number(answer);
}
```

위의 풀이에서 사용된 메서드를 살펴보자.

- `String.split()`
  - string 객체를 지정한 구분하를 이용하여 여러 개의 문자열로 나눈다.
  - 첫 번째 매개변수는 원본 문자열을 끊어야 할 부분을 나타내는 문자열을 나타낸다.(실제 문자열 또는 정규표현식)
- `Array.join()`
  - 배열의 모든 요소를 연결해 하나의 문자열로 만든다.
  - 첫 번째 매개변수는 배열의 각 요소를 구분할 문자열을 지정한다. 생략하면 배열의 요소들이 쉼표로 구분된다.

하나의 예시를 통해 위의 코드를 분석하자.

"5zero4two12onefive9twosixseven"을 원래 숫자로 만들어보자.

| 순서 | arr                                  | answer                        |
| ---- | ------------------------------------ | ----------------------------- |
| 1    | [ '5', '4two12onefive9twosixseven' ] | "504two12onefive9twosixseven" |
| 2    | [ '504two12', 'five9twosixseven' ]   | 504two121five9twosixseven     |
| 3    | [ '504', '121five9', 'sixseven' ]    | 5042121five92sixseven         |
| 4    | [ '5042121five92sixseven' ]          | 5042121five92sixseven         |
| 5    | [ '5042121five92sixseven' ]          | 5042121five92sixseven         |
| 6    | [ '5042121', '92sixseven' ]          | 5042121592sixseven            |
| 7    | [ '5042121592', 'seven' ]            | 50421215926seven              |
| 8    | [ '50421215926', '' ]                | 504212159267                  |
| 9    | [ '504212159267' ]                   | 504212159267                  |
| 10   | [ '504212159267' ]                   | 504212159267                  |

이런식으로 문자열이 숫자로 바뀐다. 다만 아쉬운 점은 4,5,9,10 번째 반복은 필요가 없는데 실행을
했다는 것이다.

그리고 마지막으로 문자열을 숫자로 바꿔주기 위해 `Number()`를 사용한다.
`Number()`는 `parseInt()`보다 실행 속도가 더 빠르다고 한다.

---

### 결과

![programmers_numeric_strings_en_words_result2](/image/CodingTest/programmers_numeric_strings_en_words/programmers_numeric_strings_en_words_result2.png)

실행속도는 나의 풀이가 더 빠르다.

---

## 5. Conclusion

> `String.replace()` 메서드를 사용하지 않고 문자열을 바꾸는 방법에 대해 배웠던 문제였다. 실행속도를 보면
> 더 효율적인 풀이라고는 생각되지 않지만 이러한 방법이 있다는 사실만 알고 있어도 나중에 다른 문제를 풀었을 때
> 큰 도움이 될 것 같다.

---

---

[👆](#숫자-문자열과-영단어)

📅 2022-08-19
