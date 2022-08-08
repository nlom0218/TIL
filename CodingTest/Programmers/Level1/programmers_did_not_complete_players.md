# 완주하지 못한 선수

## 1. 개요

- 프로그래머스
- Lv.1
- 해시
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/42576)

---

## 2. 문제 설명

수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

---

### 2-1. 문제 설명 - 제한사항

- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

---

### 2-2. 문제 설명 - 입출력 예 설명

| participant                                       | completion                               | return   |
| ------------------------------------------------- | ---------------------------------------- | -------- |
| ["leo", "kiki", "eden"]                           | ["eden", "kiki"]                         | "leo"    |
| ["marina", "josipa", "nikola", "vinko", "filipa"] | ["josipa", "filipa", "marina", "nikola"] | "vinko"  |
| ["mislav", "stanko", "mislav", "ana"]             | ["stanko", "ana", "mislav"]              | "mislav" |

---

### 2-3. 문제 설명 - 입출력 예 설명

예제 #1
"leo"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #2
"vinko"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #3
"mislav"는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.

---

## 3. 효율성 테스트를 실패한 문제 풀이

이번 코딩 테스트에서는 효율성 테스트가 있었다. 즉, 시간이 초과되면 실패된다. 먼저 살펴볼 문제 풀이는 효율성 테스트를 통과하지 못한 문제 풀이이다.

```js
function solution(participant, completion) {
    // 1) 완주하지 못한 선수를 담을 변수
    var failPlayer;

    participant.forEach(player => {
        // 2) 완주자 명단에 선수의 이름이 없으면 완주하지 못한 선수
        if(!completion.includes(player)) {
            failPlayer = player
        }

        // 3) 완주한 명단에서 완주한 선수의 인덱스 찾은 후 완주한 선수 제거하기
        const comPlayerIndex = completion.findIndex(comPlayer => comPlayer === player)
        const newCompletion = completion.filter((_, index) => index !== comPlayerIndex)
        completion = newCompletion
    })
    return failPlayer
```

---

### 1) 완주하지 못한 선수를 담을 변수

완주하지 못한 선수를 담을 변수를 만든다.

```js
var failPlayer;
```

---

### 2) 완주자 명단에 선수의 이름이 없으면 완주하지 못한 선수

`completion`배열에 참가한 선수의 이름이 없다면 완주하지 못한 선수이기 때문에 `Array.includes()`메서드를 이용하여 `player`가 `completion`배열에 포함되어 있는지 포함되어 있지 않는지 확인한다. 그래서 만약 포함되어 있지 않다면 `failPlayer`값을 `player`로 바꾼다.

```js
if (!completion.includes(player)) {
  failPlayer = player;
}
```

---

### 3) 완주한 명단에서 완주한 선수의 인덱스 찾은 후 완주한 선수 제거하기

`completion`배열에 `player`가 포함되어 있으면 `Array.findIndex()`메서드와 `Array.filter()`메서드를 사용하여 `completion`배열에서 `player`를 제거한다. 이러한 과정을 거치는 이유는 동명이인의 선수가 있을 수 있기 때문이다. 만약 동명이인 없다면 전체 풀이 과정이 훨씬 간단해 질 것이다.

`Array.findIndex()`는 인자로 들어오는 값에 해당하는 첫 번째 요소의 인덱스를 반환한다. 그리고 해당 인덱스를 가진 요소를 제외하고 새로운 완주자 명단을 만든다.

```js
const comPlayerIndex = completion.findIndex(
  (comPlayer) => comPlayer === player
);
const newCompletion = completion.filter((_, index) => index !== comPlayerIndex);
completion = newCompletion;
```

---

### 결과

![programmers_did_not_complete_players result1](/image/CodingTest/programmers_did_not_complete_players/programmers_did_not_complete_players_result1.png)

결과는 정확성 테스트에선 통과를 했지만 효율성 테스트에서 실패를 하였다. 그 이유는 `3) 완주한 명단에서 완주한 선수의 인덱스 찾은 후 완주한 선수 제거하기`에서 시간이 오래 걸렸기 때문이지 않을까 싶다...

---

## 4. 효율성 테스트를 통과한 문제 풀이

이번엔 효율성 테스트를 통과한 문제 풀이를 살펴보자. 위의 풀이에서 중복된 풀이는 생략한다.

```js
function solution(participant, completion) {
  var failPlayer;

  // 1) 참가자 명단과 완주자 명단을 알파벳 순으로 정렬한다.
  participant.sort();
  completion.sort();

  // 2) for...in문을 통해 반복문을 실행한다.
  for (index in participant) {
    // 3) 같은 인덱스에 위치한 선수의 이름이 다를 경우 해당 선수를 완주하지 못한 선수로 정하고 for문을 종료한다.
    if (participant[index] !== completion[index]) {
      failPlayer = participant[index];
      break;
    }
  }

  return failPlayer;
}
```

위의 코드는 `completion`배열은 `participant`배열의 길이보다 하나 작고 모든 요소를 포함하고 있다 특징을 활용하여 각각의 배열을 알파벳 순으로 정렬하고 같은 위치에 있는 값을 서로 비교하여 완주하지 못한 선수를 찾는 과정이다.

---

### 1) 참가자 명단과 완주자 명단을 알파벳 순으로 정렬한다.

```js
participant.sort();
completion.sort();
```

`Array.sort()` 메서드를 사용하여 알파벳 순으로 배열을 다시 정렬한다.

![programmers_did_not_complete_players_ sort](/image/CodingTest/programmers_did_not_complete_players/programmers_did_not_complete_players_sort.png)

---

### 2) for in문을 통해 반복문을 실행한다.

처음에는 `for...in`문을 통해 반복문을 실행하여 `participant`배열은 `completion`배열의 같은 위치의 값을 비교한다.

### 3) 같은 인덱스에 위치한 선수의 이름이 다를 경우 해당 선수를 완주하지 못한 선수로 정하고 for문을 종료한다.

```js
if (participant[index] !== completion[index]) {
    failPlayer = participant[index];
    break;
}
```

`participant`배열은 `completion`배열의 같은 위치의 값을 비교 했을 때 서로 다른 값이면 `participant[index]`의 값은 완주하지 못한 선수가 될 것이다. 그래서 `failPlayer`에 할당하고 `for...in문`을 종료한다.

---

### 결과

![programmers_did_not_complete_players result2](/image/CodingTest/programmers_did_not_complete_players/programmers_did_not_complete_players_result2.png)

이번엔 효율성 테스트까지 통과한 모습을 볼 수 있다.

---

## 5. Refactoring

다른 사람들의 문제 풀이의 댓글을 읽어보다가 `for...in` 구문 보다는 `for`를 사용하거나 ES6의 `forEach` 함수를 사용하는 것을 권장하다는 댓글이 있었다. 그 이유는 `for...in` 구문이 성능을 하락 시키는 주요 코드라는 설명이 있었고 성능을 하락시킨다는 의미보다는 객체 내의 프로퍼티 속성 중 enumerable:true인 모든 프로퍼티를 순회하여 검색하는 용도이기 때문에 적합하지 않는다는 댓글도 있었다. 그래서 `for...in` 구문 대신 `for`문, `Array.some()`메서드를 사용하여 문제를 다시 풀었다. 그랬더니 `Array.some()`으로 문제를 풀었을 때 효율성 테스트에서 실행 속도가 가장 빠르게 측정이 되었다.

리팩토링 한 코드를 작성하기 전 간단하게 배열 메서드 3개에 대해 살펴보자.

- `Array.forEach()`: 순회가 중단하지 않는다.
- `Array.some()`: 특정 조건을 만족할 경우 순회가 중단된다.
- `Array.every()`: 특정 조건을 만족하지 않을 경우 순회가 중단된다.

위의 배열 메서드는 모두 배열을 순회하는 메서드이다. 그 중 하는 `Array.some()` 메서드를 사용했는데 그 이유는 완주하지 못한 선수를 찾을 경우 순회를 중단하기 위해서이다.

그러면 위의 `for...in` 구문을 아래와 같이 바꾸어보자.

```js
participant.some((player, index) => {
  if (player !== completion[index]) {
    failPlayer = participant[index];
    return true;
  }
});
```

![programmers_did_not_complete_players result3](/image/CodingTest/programmers_did_not_complete_players/programmers_did_not_complete_players_result3.png)

효율성 테스의 실행속도 조금 더 빨라진 것을 확인할 수 있다.

---

## 6. 다른 사람의 풀이

다른 사람의 문제 풀이 중 해시 알고리즘을 활용하여 문제를 푼 코드를 가져왔다. 좋아요 수도 많았고 댓글에 해당 코드에 대한 칭찬도 많았다.

```js
function solution(participant, completion) {
  const map = new Map();

  for (let i = 0; i < participant.length; i++) {
    let a = participant[i],
      b = completion[i];

    map.set(a, (map.get(a) || 0) + 1);
    map.set(b, (map.get(b) || 0) - 1);
  }

  for (let [k, v] of map) {
    if (v > 0) return k;
  }

  return "nothing";
}
```

위의 코드를 딱 보고 처음의 반응은 "잉? 어떤 의미지"였다. 두 번째 줄에 등장한 코드부터 처음보는 코드였다.(아래 코드 참고)

```js
const map = new Map();
```

그리고 아직 해시에 대한 공부가 부족하기 때문에 내일 맵(Map)과 해시에 대한 공부를 한 뒤 다시 코드를 분석하고자 한다

---

## 7. Conclusion

> 처음에는 동명이인의 변수를 생각하지 않고 "이번 코딩 테스트의 문제는 쉬운데??"라고 생각을 했지만 큰 오산이었다. 동명이인의 변수를 생각하여 다시 코드를 고민하니 이번에는 효율성 테스트에서 막히였다. 그래서 `Array.sort()`를 사용해 단순히 인덱스의 값만 가지고 비교를 하였다. 다른 사람의 풀이를 보니 나와 같은 방법으로 푼 분들도 많이 있었다. 하지만 해시 알고리즘을 이용해서 풀어야 한다고 생각한다.해시와 맵(Map)에 대해 공부를 더 하고 `6. 다른 사람의 풀이`에 대한 분석을 이어나가보도록 하자. 아직 모르는 개념이 많다. 모르는 것이 있으면 미루지 말고 그때 그때 공부하자.🔥

---

[👆](#완주하지-못한-선수)

📅 2022-08-07
