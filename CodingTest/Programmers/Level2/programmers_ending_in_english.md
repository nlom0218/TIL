# 영어 끝말잇기

## 1. 개요

- 프로그래머스
- Lv.2
- Summer/Winter Coding(~2018)
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/12981)

---

## 2. 문제 설명

1부터 n까지 번호가 붙어있는 n명의 사람이 영어 끝말잇기를 하고 있습니다. 영어 끝말잇기는 다음과 같은 규칙으로 진행됩니다.

1. 1번부터 번호 순서대로 한 사람씩 차례대로 단어를 말합니다.
2. 마지막 사람이 단어를 말한 다음에는 다시 1번부터 시작합니다.
3. 앞사람이 말한 단어의 마지막 문자로 시작하는 단어를 말해야 합니다.
4. 이전에 등장했던 단어는 사용할 수 없습니다.
5. 한 글자인 단어는 인정되지 않습니다.

다음은 3명이 끝말잇기를 하는 상황을 나타냅니다.

tank → kick → know → wheel → land → dream → mother → robot → tank

위 끝말잇기는 다음과 같이 진행됩니다.

- 1번 사람이 자신의 첫 번째 차례에 tank를 말합니다.
- 2번 사람이 자신의 첫 번째 차례에 kick을 말합니다.
- 3번 사람이 자신의 첫 번째 차례에 know를 말합니다.
- 1번 사람이 자신의 두 번째 차례에 wheel을 말합니다.
- (계속 진행)

끝말잇기를 계속 진행해 나가다 보면, 3번 사람이 자신의 세 번째 차례에 말한 tank 라는 단어는 이전에 등장했던 단어이므로 탈락하게 됩니다.

사람의 수 n과 사람들이 순서대로 말한 단어 words 가 매개변수로 주어질 때, 가장 먼저 탈락하는 사람의 번호와 그 사람이 자신의 몇 번째 차례에 탈락하는지를 구해서 return 하도록 solution 함수를 완성해주세요.

---

### 2-1. 문제 설명 - 제한 사항

- 끝말잇기에 참여하는 사람의 수 n은 2 이상 10 이하의 자연수입니다.
- words는 끝말잇기에 사용한 단어들이 순서대로 들어있는 배열이며, 길이는 n 이상 100 이하입니다.
- 단어의 길이는 2 이상 50 이하입니다.
- 모든 단어는 알파벳 소문자로만 이루어져 있습니다.
- 끝말잇기에 사용되는 단어의 뜻(의미)은 신경 쓰지 않으셔도 됩니다.
- 정답은 [ 번호, 차례 ] 형태로 return 해주세요.
- 만약 주어진 단어들로 탈락자가 생기지 않는다면, [0, 0]을 return 해주세요.

---

### 2-2. 문제 설명 - 입출력 예

| n   | words                                                                                                                                                              | result |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------ |
| 3   | ["tank", "kick", "know", "wheel", "land", "dream", "mother", "robot", "tank"]                                                                                      | [3,3]  |
| 5   | ["hello", "observe", "effect", "take", "either", "recognize", "encourage", "ensure", "establish", "hang", "gather", "refer", "reference", "estimate", "executive"] | [0,0]  |
| 2   | ["hello", "one", "even", "never", "now", "world", "draw"]                                                                                                          | [1,3]  |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1

3명의 사람이 끝말잇기에 참여하고 있습니다.

- 1번 사람 : tank, wheel, mother
- 2번 사람 : kick, land, robot
- 3번 사람 : know, dream, tank

와 같은 순서로 말을 하게 되며, 3번 사람이 자신의 세 번째 차례에 말한 tank라는 단어가 1번 사람이 자신의 첫 번째 차례에 말한 tank와 같으므로 3번 사람이 자신의 세 번째 차례로 말을 할 때 처음 탈락자가 나오게 됩니다.

입출력 예 #2

5명의 사람이 끝말잇기에 참여하고 있습니다.

- 1번 사람 : hello, recognize, gather
- 2번 사람 : observe, encourage, refer
- 3번 사람 : effect, ensure, reference
- 4번 사람 : take, establish, estimate
- 5번 사람 : either, hang, executive

와 같은 순서로 말을 하게 되며, 이 경우는 주어진 단어로만으로는 탈락자가 발생하지 않습니다. 따라서 [0, 0]을 return하면 됩니다.

입출력 예 #3

2명의 사람이 끝말잇기에 참여하고 있습니다.

- 1번 사람 : hello, even, now, draw
- 2번 사람 : one, never, world

와 같은 순서로 말을 하게 되며, 1번 사람이 자신의 세 번째 차례에 'r'로 시작하는 단어 대신, n으로 시작하는 now를 말했기 때문에 이때 처음 탈락자가 나오게 됩니다.

---

## 3. 문제 풀이

```javascript
function solution(n, words) {
  // 1) 진행된 단어를 요소로 가지는 배열과 탈락 라운드를 카운트하는 변수 만들기
  let round = [words[0]];
  let fail = 2;

  for (let i = 1; i < words.length; i++) {
    // 2) 이미 등장한 단어가 다시 등장되면 반복문 종료하기
    const is_includes = round.includes(words[i]);
    if (is_includes) break;

    // 3) 이전 단어의 마지막 문자와 현재 단어의 첫 문자가 다르면 반복문 종료하기
    const last_round = round[round.length - 1];
    const last_word = [...last_round].pop();
    const first_word = [...words[i]][0];
    if (last_word !== first_word) break;

    // 4) round 배열에 단어 추가한 후 fail 1증가시키기
    round.push(words[i]);
    fail++;
  }

  // 5) 아무도 실패하지 않았따면 [0, 0]를 반환하기
  if (round.length === words.length) return [0, 0];

  // 6) 탈락한 사람과 탈락한 순서 구한 후 반환하기
  const person = fail % n === 0 ? n : fail % n;
  const order = Math.ceil(fail / n);
  return [person, order];
}
```

---

### 1) 진행된 단어를 요소로 가지는 배열과 탈락 라운드를 카운트하는 변수 만들기

```javascript
let round = [words[0]];
let fail = 2;
```

첫 번째 라운드인 `words[0]`를 0번째 요소로 가지는 `round` 배열을 만든다. 그리고 탈락된 순서를
계산하는 변수 `fail`를 변수로 만든다.

`round` 배열은 아래의 반복문을 통해 새로운 단어들이 추가되고 `fail` 변수는 아래의 반복문에서 특정
조건이 만족하지 않는다면 1씩 증가된다.

---

### 2) 이미 등장한 단어가 다시 등장되면 반복문 종료하기

```javascript
const is_includes = round.includes(words[i]);
if (is_includes) break;
```

만약 제시한 단어가 이전에 등장했다면 해당 단어를 말한 사람은 탈락한다.

즉, `round` 배열에 요소로 해당 단어가 있다면 탈락을 하게 된다. 탈락이 확정되면
더 이상의 라운드 진행은 필요없기 때문에 `break`를 실행하여 반복문을 종료한다.

---

### 3) 이전 단어의 마지막 문자와 현재 단어의 첫 문자가 다르면 반복문 종료하기

```javascript
const last_round = round[round.length - 1];
const last_word = [...last_round].pop();
const first_word = [...words[i]][0];
if (last_word !== first_word) break;
```

만약 제시한 단어의 첫 문자가 이전 단어의 마지막 문자와 다르면 해당 단어를
말한 사람은 탈락한다.

즉, `round` 배열의 마지막 요소의 마지막 문자가 `word[i]`의 첫 번재 문자가 다르면
탈락을 한다. 이와 같은 경우엔 더 이상의 라운드 진행이 필요없기 때문에
`break`를 실행하여 반복문을 진행한다.

---

### 4) round 배열에 단어 추가한 후 fail 1증가시키기

```javascript
round.push(words[i]);
fail++;
```

`round` 배열에 없는 단어, 이전 단어의 마지막 문자가 첫 번재 문자로 온 경우
즉, 위의 조건문에서 걸러지지 않았다면 해당 단어를 `round`에 추가한다. 또한
탈락한 순서인 `fail`를 1증가시킨다.

지금 생각해보면 `fail`변수는 그닥 필요 없어 보인다. 그 이유는 `round`의 배열의
길이로 탈락한 순서를 알 수 있기 때문이다.

---

### 5) 아무도 실패하지 않았따면 [0, 0]를 반환하기

```javascript
if (round.length === words.length) return [0, 0];
```

마지막 라운드까지 완료를 하였다면 문제의 조건에 맞게 `[0, 0]`를 반환한다.

---

### 6) 탈락한 사람과 탈락한 순서 구한 후 반환하기

```javascript
const person = fail % n === 0 ? n : fail % n;
const order = Math.ceil(fail / n);
return [person, order];
```

중간에 탈락한 사람이 생기면 그 사람이 누구인지와 그 사람이 해당 단어를
몇 번째에 말했는지 알아야 한다.

`person`을 구하기 위해선 먼저 탈락한 순서를 전체 사람의 수로 나누어야 한다.
나눈 나머지가 `0`이라면 탈락한 사람의 순서는 `n`이고 그러지 않으면 나눈 수의
나머지가 된다.

`order`는 탈락한 사람이 탈락된 단어를 몇 번째에 말했는지를 나타낸다. 이를 위해
탈락한 순서에 사람을 수로 나눈 몫을 올림하여 구한다.

그 후 `person`, `order` 순으로 배열를 만들어 반환한다.

---

### 결과

![programmers_ending_in_english_result](/image/CodingTest/programmers_ending_in_english/programmers_ending_in_english_result1.png)

---

## 4. Refactoring

```javascript
function solution(n, words) {
  let fail = -1;

  for (let i = 0; i < words.length; i++) {
    // 1) 첫 번째 단어는 뒤도 돌아보지 않고 건너뛰기
    if (i === 0) continue;

    // 2) 이전 단어의 마지막 문자와 현재 단어의 첫 번째 문자를 비교하기
    if (words[i - 1][words[i - 1].length - 1] !== words[i][0]) {
      fail = i + 1;
      break;
    }

    // 3) 현재 단어가 이전에 존재 했는지 확인하기
    if (words.indexOf(words[i]) !== i) {
      fail = i + 1;
      break;
    }
  }

  if (fail === -1) return [0, 0];
  const person = fail % n === 0 ? n : fail % n;
  const order = Math.ceil(fail / n);
  return [person, order];
}
```

배열에서 `array[index]`와 같은 방법으로 특정 인덱스의 요소에 접근한다.
이는 문자열에도 같다. 즉 `문자열[index]`와 같은 방법으로 접근이 가능한다.
이를 사용하여 단어의 마지막 문자와 첫 번째 문자를 접근하여 비교하였다.

또한 `round` 배열을 없애고 `words` 배열을 `i`로 접근하였다.

전체적인 로직은 비슷하나 반복문 내에서 조건을 비교하는 방법은 달라졌다. 이를
위주로 설명을 작성한다.

---

### 1) 첫 번째 단어는 뒤도 돌아보지 않고 건너뛰기

```javascript
if (i === 0) continue;
```

`i`가 `0`이라는 것은 첫 번재 단어라는 것이다. 첫 번째 단어는 당연히
탈락을 할 수 없기 때문에 다음 반복문으로 넘어간다.

---

### 2) 이전 단어의 마지막 문자와 현재 단어의 첫 번째 문자를 비교하기

```javascript
if (words[i - 1][words[i - 1].length - 1] !== words[i][0]) {
    fail = i + 1;
    break;
}
```

`i` 번째 단어의 이전 단어인 `i - 1` 번째 단어의 마지막 문자와
`i` 번째 단어의 첫 번째 문자를 비교하였을 때 그 둘이 다르다면 `fail`를
`i + 1`로 바꾸고 반복문을 종룔한다.

`i`는 인덱스이기 때문에 실제 실패한 순서는 `i`에 1을 더한 값이다.
왜냐! `i`는 0부터 시작하기 때문이다.

---

### 3) 현재 단어가 이전에 존재 했는지 확인하기

```javascript
if (words.indexOf(words[i]) !== i) {
    fail = i + 1;
    break;
}
```

`i` 번째 단어를 `indexOf`를 검색하였을 때 `i`와 같지 않다면
이전에 `i` 번째 단어와 같은 단어가 존재한다는 것이다. `array.indexOf()`
메서드는 매개변수로 주어진 값과 일치하는 요소의 인덱스를 반환한다. 만약
3번 인덱스와 8번 인덱스가 같은 값이고 해당 값이 매개변수로 주어진다면
`array.indexOf()` 메서드는 `3`를 반환한다. 즉, 순서가 빠른
인덱스를 반환한다.

---

### 결과

뭔가.. 더 가독성이 좋아졌다고 생각해서 실행속도도 크게 차이나지 않을 것이라
생각했는데 리팩토링한 풀이가 더 오래 걸린다.

![programmers_ending_in_english_result2](/image/CodingTest/programmers_ending_in_english/programmers_ending_in_english_result2.png)

---

## 5. Conclusion

> 엄청 어렵지 않은 문제였다. 조건만 잘 파악하면 특정 알고리즘과 자료구조
> 없이 쉽게 해결할 수 있는 문제라고 생각한다. 어렵지 않았지만 무엇이 더
> 효율적인 풀이인지 항상 고민을 해야 겠다고 생각했다. 내가 처음으로 풀었던
> 풀이에서는 굳이 필요없는 배열을 만들어 풀었다. 딱 처음 생각하는 풀이를 그대로
> 적었기 때문에 수정하는 과정은 반드시 필요하겠지만 실제 코딩테스트에선 여유롭게 풀 수
> 없기 때문에 한 번 생각할 때 최적의 최선의 최고의 풀이를 떠오를 수 있도록
> 공부하고 노력하자.

---

📅 2022-09-26
