# [1차] 프렌즈4블록

## 1. 개요

- 프로그래머스
- Lv.2
- 2018 KAKAO BLIND RECRUITMENT
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/17679)

---

## 2. 문제 설명

프렌즈4블록

블라인드 공채를 통과한 신입 사원 라이언은 신규 게임 개발 업무를 맡게 되었다. 이번에 출시할 게임 제목은 "프렌즈4블록".
같은 모양의 카카오프렌즈 블록이 2×2 형태로 4개가 붙어있을 경우 사라지면서 점수를 얻는 게임이다.

![프렌즈4블록 이미지](/image/CodingTest/programmers_friends_4_blocks/programmers_friends_4_blocks_image1.png)

만약 판이 위와 같이 주어질 경우, 라이언이 2×2로 배치된 7개 블록과 콘이 2×2로 배치된 4개 블록이 지워진다. 같은 블록은 여러 2×2에 포함될 수 있으며, 지워지는 조건에 만족하는 2×2 모양이 여러 개 있다면 한꺼번에 지워진다.

![프렌즈4블록 이미지](/image/CodingTest/programmers_friends_4_blocks/programmers_friends_4_blocks_image2.png)

블록이 지워진 후에 위에 있는 블록이 아래로 떨어져 빈 공간을 채우게 된다.

![프렌즈4블록 이미지](/image/CodingTest/programmers_friends_4_blocks/programmers_friends_4_blocks_image3.png)

만약 빈 공간을 채운 후에 다시 2×2 형태로 같은 모양의 블록이 모이면 다시 지워지고 떨어지고를 반복하게 된다.

![프렌즈4블록 이미지](/image/CodingTest/programmers_friends_4_blocks/programmers_friends_4_blocks_image4.png)

위 초기 배치를 문자로 표시하면 아래와 같다.

> TTTANT  
> RRFACC  
> RRRFCC  
> TRRRAA  
> TTMMMF  
> TMMTTJ

각 문자는 라이언(R), 무지(M), 어피치(A), 프로도(F), 네오(N), 튜브(T), 제이지(J), 콘(C)을 의미한다

입력으로 블록의 첫 배치가 주어졌을 때, 지워지는 블록은 모두 몇 개인지 판단하는 프로그램을 제작하라.

### 2-1. 문제 설명 - 입력 형식

- 입력으로 판의 높이 m, 폭 n과 판의 배치 정보 board가 들어온다.
- 2 ≦ n, m ≦ 30
- board는 길이 n인 문자열 m개의 배열로 주어진다. 블록을 나타내는 문자는 대문자 A에서 Z가 사용된다.

### 2-2. 문제 설명 - 출력 형식

입력으로 주어진 판 정보를 가지고 몇 개의 블록이 지워질지 출력하라.

### 2-3. 문제 설명 - 입출력 예제

| m   | n   | board                                                        | answer |
| --- | --- | ------------------------------------------------------------ | ------ |
| 4   | 5   | ["CCBDE", "AAADE", "AAABF", "CCBBF"]                         | 14     |
| 6   | 6   | ["TTTANT", "RRFACC", "RRRFCC", "TRRRAA", "TTMMMF", "TMMTTJ"] | 15     |

### 2-4. 문제 설명 - 예제에 대한 설명

- 입출력 예제 1의 경우, 첫 번째에는 A 블록 6개가 지워지고, 두 번째에는 B 블록 4개와 C 블록 4개가 지워져, 모두 14개의 블록이 지워진다.
- 입출력 예제 2는 본문 설명에 있는 그림을 옮긴 것이다. 11개와 4개의 블록이 차례로 지워지며, 모두 15개의 블록이 지워진다.

---

## 3. 문제 풀이

```javascript
function solution(m, n, board) {
  // 1) 2중 중첩 배열 만들기
  board = board.map((item) => [...item]);
  let count = 0;

  while (true) {
    // 2) 특정 위치에 같은 값이 존재하면 해당 요소의 인덱스를 delItem에 넣기
    let delItem = [];
    for (let i = 0; i < m - 1; i++) {
      for (let j = 0; j < n - 1; j++) {
        if (board[i][j] === ' ') continue;
        const item1 = board[i][j];
        const item2 = board[i + 1][j];
        const item3 = board[i + 1][j + 1];
        const item4 = board[i][j + 1];
        if (new Set([item1, item2, item3, item4]).size !== 1) continue;
        delItem.push(
          `${i},${j}`,
          `${i + 1},${j}`,
          `${i + 1},${j + 1}`,
          `${i},${j + 1}`
        );
      }
    }

    // 3) 같은 값을 가지는 요소가 없으면 반복문 종료하기
    if (new Set(delItem).size === 0) break;

    // 4) 중복된 요소를 제거 한 후 count에 요소의 개수를 더하기
    count += new Set(delItem).size;

    // 5) 제거된 위치에 " "넣기
    for (let i = 0; i < m; i++) {
      for (let j = 0; j < n; j++) {
        if (delItem.includes(`${i},${j}`)) board[i][j] = ' ';
      }
    }

    // 6) 중간에 " "이 존재할 때, 위에 있는 요소를 아래로 내리기
    for (let i = 0; i < n; i++) {
      let arr = [];
      for (let j = 0; j < m; j++) {
        arr.push(board[j][i]);
      }
      arr = arr.filter((item) => item !== ' ');
      arr = [...new Array(m - arr.length).fill(' '), ...arr];
      for (let j = 0; j < m; j++) {
        board[j][i] = arr[j];
      }
    }
  }

  return count;
}
```

### 1) 2중 중첩 배열 만들기

```javascript
board = board.map((item) => [...item]);
```

매개변수로 주어지는 `board` 배열은 요소가 문자열이다. 이런 요소를 구조분해 할당을 통해 배열로 만들어 2중 중첩 배열을 만든다.

### 2) 특정 위치에 같은 값이 존재하면 해당 요소의 인덱스를 delItem에 넣기

```javascript
let delItem = [];
for (let i = 0; i < m - 1; i++) {
  for (let j = 0; j < n - 1; j++) {
    if (board[i][j] === ' ') continue;
    const item1 = board[i][j];
    const item2 = board[i + 1][j];
    const item3 = board[i + 1][j + 1];
    const item4 = board[i][j + 1];
    if (new Set([item1, item2, item3, item4]).size !== 1) continue;
    delItem.push(
      `${i},${j}`,
      `${i + 1},${j}`,
      `${i + 1},${j + 1}`,
      `${i},${j + 1}`
    );
  }
}
```

반복문에서 가장 첫번재로 해야 할 것은 특정 위치에 같은 값이 존재하는지 확인하여 같은 값이 존재하면 해당 요소들의 인덱스를 `delItem`에 넣는 것이다. 문제 조건을 보면, 2 x 2 형태로 4개가 모두 같은 값이면 이는 삭제해야 하는 값들이다.

때문에 `item1 ~4`를 만들어 이를 `set` 객체를 통해 값들을 비교한다. 만약 `set` 객체의 `size`가 1이라면 이는 모두 같은 값이되는 것이고 그렇지 않으면 같은 값이 아니라는 것이다.

### 3) 같은 값을 가지는 요소가 없으면 반복문 종료하기

```javascript
if (new Set(delItem).size === 0) break;
```

`while` 문의 조건을 보면 항상 `true`이다. 때문에 반복문 내에서 `while` 문을 종료시켜야 한다. 이를 위한 코드가 바로 위에 있는 코드이다. `delItem`에 요소가 없으면 반복문을 종료하는 것이다. 즉, `delItem`에 요소가 없다는 것은 문제 조건에 부합되는 요소가 없다는 것이다. 이는 더 이상 삭제할 요소가 없다는 뜻이다.

### 4) 중복된 요소를 제거 한 후 count에 요소의 개수를 더하기

```javascript
count += new Set(delItem).size;
```

`count`는 제거된 요소의 개수를 카운트하는 변수이다. 때문에 매 반복문에서 `delItem`에 있는 요소 중 중복된 것을 제외한 개수를 추가해야 한다.

### 5) 제거된 위치에 " "넣기

```javascript
for (let i = 0; i < m; i++) {
  for (let j = 0; j < n; j++) {
    if (delItem.includes(`${i},${j}`)) board[i][j] = ' ';
  }
}
```

다음 반복문을 위해 제거된 위치에 `" "`을 넣는다.

### 6) 중간에 " "이 존재할 때, 위에 있는 요소를 아래로 내리기

```javascript
for (let i = 0; i < n; i++) {
  let arr = [];
  for (let j = 0; j < m; j++) {
    arr.push(board[j][i]);
  }
  arr = arr.filter((item) => item !== ' ');
  arr = [...new Array(m - arr.length).fill(' '), ...arr];
  for (let j = 0; j < m; j++) {
    board[j][i] = arr[j];
  }
}
```

위 코드는 문제에서 블록들이 아래로 내려가는 상황을 코드를 작성한 것이다. 먼저 행과 열을 바뀐 뒤, `" "`를 제거하고 제거한 개수 만큼 앞에 붙인다. 마지막로 다시 행과 열을 바꾸는 것이다.

### 결과

![programmers_friends_4_blocks_result](/image/CodingTest/programmers_friends_4_blocks/)

---

## 4. Conclusion

> 구현 문제이기 때문에 복잡한 알고리즘이나 자료구조는 필요가 없었다. 이 문제의 핵심은 이중 중첩 배열을 어떻게 다룰 수 있는냐를 확인하는 것이라고 생각한다. 행과 열을 바꾸고 요소를 제거하고 조건에 부합되는 요소를 찾는 과정이 복잡하지만 차근 차근 생각을 하면 쉽게 풀 수 있다고 생각한다. 물론 처음부터 해결법이 바로 생각나는 것은 아니지만 천천히 문제를 읽고 조건을 생각하면 그리 어려운 문제도 아니다. 단, 시간 복잡도가 `O(n^2)`이라는 점이 아쉽다. 물론, `n` 자체가 크지 않아서 이를 의도한 것인지는 몰라고 코드를 작성하면서 혹시나 시간 초과에 대한 두려움이 있었다.

---

📅 2023-02-05
