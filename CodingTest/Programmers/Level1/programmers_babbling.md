# 옹알이(2)

## 1. 개요

- 프로그래머스
- Lv.1
- 연습문제
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/133499#)

---

## 2. 문제 설명

머쓱이는 태어난 지 11개월 된 조카를 돌보고 있습니다. 조카는 아직 "aya", "ye", "woo", "ma" 네 가지 발음과 네 가지 발음을 조합해서 만들 수 있는 발음밖에 하지 못하고 연속해서 같은 발음을 하는 것을 어려워합니다. 문자열 배열 babbling이 매개변수로 주어질 때, 머쓱이의 조카가 발음할 수 있는 단어의 개수를 return하도록 solution 함수를 완성해주세요.

### 2-1. 문제 설명 - 제한사항

- 1 ≤ babbling의 길이 ≤ 100
- 1 ≤ babbling[i]의 길이 ≤ 30
- 문자열은 알파벳 소문자로만 이루어져 있습니다.

### 2-2. 문제 설명 - 입출력 예

| babbling                                       | result |
| ---------------------------------------------- | ------ |
| ["aya", "yee", "u", "maa"]                     | 1      |
| ["ayaye", "uuu", "yeye", "yemawoo", "ayaayaa"] | 2      |

### 2-3. 입출력 예 설명

입출력 예 #1

- ["aya", "yee", "u", "maa"]에서 발음할 수 있는 것은 "aya"뿐입니다. 따라서 1을 return합니다.

입출력 예 #2

- ["ayaye", "uuuma", "yeye", "yemawoo", "ayaayaa"]에서 발음할 수 있는 것은 "aya" + "ye" = "ayaye", "ye" + "ma" + "woo" = "yemawoo"로 2개입니다. "yeye"는 같은 발음이 연속되므로 발음할 수 없습니다. 따라서 2를 return합니다.

### 2-4. 유의사항

- 네 가지를 붙여 만들 수 있는 발음 이외에는 어떤 발음도 할 수 없는 것으로 규정합니다. 예를 들어 "woowo"는 "woo"는 발음할 수 있지만 "wo"를 발음할 수 없기 때문에 할 수 없는 발음입니다.

---

## 3. 문제 풀이

```javascript
function solution(babbling) {
  // 1) 머쓱이가 할 수 있는 발음
  const words = ['aya', 'ye', 'woo', 'ma'];
  let count = 0;
  // 2) babbling을 순회하며 머쓱이가 발음할 수 있는 문자열 구하기
  babbling.forEach((item) => {
    words.forEach((word) => {
      // 3) 연속된 발음이 있다면 다음 문자열로 넘어가기
      const regex = new RegExp(`${word.repeat(2)}`, 'g');
      if (item.match(regex)) return;
      // 4) 머쓱이가 할 수 있는 발음을 "/"으로 바꾸기
      item = item.split(word).join('/');
    });
    // 5) 문자열이 모두 제거되었으면 count 1올리기
    if (item.split('/').join('') === '') count += 1;
  });

  return count;
}
```

### 1) 머쓱이가 할 수 있는 발음

```javascript
const words = ['aya', 'ye', 'woo', 'ma'];
```

문제의 설명에 따르면 머쓱이가 할 수 있는 발음은 위의 4개 뿐이다. 단, 연속된 발음은 할 수 없다. 예를 들어 "ayaaya"는 하지 못한다. 하지만 "ayayeaya"처럼 중간에 다른 발음이 있다면 이는 가능하다. 이를 잘 생각하며 문제를 해결해야 한다.

### 2) babbling을 순회하며 머쓱이가 발음할 수 있는 문자열 구하기

```javascript
babbling.forEach((item) => {
  words.forEach((word) => {});
});
```

매개변수로 받은 `babbling`를 반복문을 돌린다. 각각의 문자열을 머쓱이가 발음할 수 있는 문자로만 이루어져 있는지 확인하기 위해 미리 만들어 둔 `word`를 반복문 돌려 확인하다.

이때 시간 복잡도는 `O(n^2)`이지만 `word`은 길이가 4인 배열이기 때문에 큰 시간이 소요되지 않는다고 생각한다.

### 3. 연속된 발음이 있다면 다음 문자열로 넘어가기

```javascript
const regex = new RegExp(`${word.repeat(2)}`, 'g');
if (item.match(regex)) return;
```

머쓱이가 연속된 같은 발음 못하기 때문에 이를 위해 변수 `regex`만든다. 그리고 만약 `babbling`의 어떤 요소(단어)가 `match()` 메서드를 통해 정규식을 통과한다면 해당 요소(단어)는 발음하지 못하는 것이기 때문에 다음 요소로 넘어간다.

### 4) 머쓱이가 할 수 있는 발음을 "/"으로 바꾸기

```javascript
item = item.split(word).join('/');
```

`babbling`의 요소(단어)를 머쓱이가 발음할 수 있는 단어를 기준으로 자른 후 "/"으로 다시 문자열로 만든다. 즉, 머쓱이가 발음할 수 있는 단어를 "/"으로 바꾸는 코드이다.

"/"으로 바꾸는 이유는 앞뒤의 문자열이 합쳐서 머쓱이가 발음할 수 있는 단어가 만들어 질 수 있기 때문이다. 아래의 예시처럼 말이다.

- "ayeeya": "yee"가 제거된다.
- "aya": "aya"가 제거된다.
- "": 빈 문자열인 것은 머쓱이가 발음할 수 있는 단어라는 것이다. 하지만 "ayeeay"는 머쓱이가 발음 할 수 없다.

### 5) 문자열이 모두 제거되었으면 count 1올리기

```javascript
if (item.split('/').join('') === '') count += 1;
```

마지막으로 "/"을 모두 제거한 후 결과가 빈 문자열이라면 머쓱이가 발음할 수 있는 단어이기 때문에 `count`를 1올린 후 반복문이 모두 끝나면 `count`를 반환한다.

### 결과

![programmers_babbling_result](/image/CodingTest/programmers_babbling/programmers_babbling_result.png)

이중 반복문을 사용하고 있어 `O(n^2)`이지만 빠른 실행속도로 통과된 것을 확인할 수 있다.

근데.. 따지고 생각해보면 `O(n)`인 거 같다. 왜냐 `n`은 변하지 않는 값이기 때문에 매개변수와 상관이 없기 때문이다.

---

## 4. 다른 사람 풀이

내가 부족한 정규식을 활용하여 깔끔하게 푼 풀이가 있어 이를 가져왔다.

```javascript
function solution(babbling) {
  const regexp1 = /(aya|ye|woo|ma)\1+/;
  const regexp2 = /^(aya|ye|woo|ma)+$/;

  return babbling.reduce(
    (ans, word) => (!regexp1.test(word) && regexp2.test(word) ? ++ans : ans),
    0
  );
}
```

나에게 부족한 점이 보이는 풀이이다.

1. `reudce`메서드의 사용: 보통 요소들의 사칙연산을 구할 때 많이 사용한다. 개인적으로는 요소가 숫자인 경우에 많이 사용하고 있다. 하지만 위의 풀이에서는 요소를 통해 참과 거짓을 구분하여 `acc`(여기서는 ans)를 1더하거나 그대로 유지하는 방법을 사용하고 있다.

2. 정규식 사용: 내가 많이 부족한 부분이다. 항상 정규식이 필요한 상황이 오면 구글링을 통해 가져다 쓰고 있다. 매번 정규식 관련 글을 읽으면 고개를 끄덕이지만 딱 거기까지이다. 아직 내것으로 만들지 않았다. 우테코 시작 전에 정규식 관련 내용을 정리하여 조금씩 데이터를 쌓아가자.

---

## 5. Conclusion

> 분명 과거에도 정규식을 공부하겠다고 TIL에 적었던 적이 있다. 하지만 지금 나는 정규식 관련한 어떠한 내용도 정리하지 않았다. 정말 필요할 때 시간을 허비하지 않도록 그리고 보다 유기적으로 생각하기 위해 더 이상 게으르게 행동하지 말고 바로 실천으로 옮기자.

---

📅 2023-01-17
