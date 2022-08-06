# [카카오 인터] 키패드 누르기

## 1. 개요

- 프로그래머스
- Lv.1
- 2020 카카로 개발자 겨울 인턴십
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/67256)

## 2. 문제 설명

스마트폰 전화 키패드의 각 칸에 다음과 같이 숫자들이 적혀 있습니다.

![문제 설명 키패드 이미지](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/4b69a271-5f4a-4bf4-9ebf-6ebed5a02d8d/kakao_phone1.png)

이 전화 키패드에서 왼손과 오른손의 엄지손가락만을 이용해서 숫자만을 입력하려고 합니다.
맨 처음 왼손 엄지손가락은 \* 키패드에 오른손 엄지손가락은 # 키패드 위치에서 시작하며, 엄지손가락을 사용하는 규칙은 다음과 같습니다.

1. 엄지손가락은 상하좌우 4가지 방향으로만 이동할 수 있으며 키패드 이동 한 칸은 거리로 1에 해당합니다.
2. 왼쪽 열의 3개의 숫자 1, 4, 7을 입력할 때는 왼손 엄지손가락을 사용합니다.
3. 오른쪽 열의 3개의 숫자 3, 6, 9를 입력할 때는 오른손 엄지손가락을 사용합니다.
4. 가운데 열의 4개의 숫자 2, 5, 8, 0을 입력할 때는 두 엄지손가락의 현재 키패드의 위치에서 더 가까운 엄지손가락을 사용합니다.

   - 만약 두 엄지손가락의 거리가 같다면, 오른손잡이는 오른손 엄지손가락, 왼손잡이는 왼손 엄지손가락을 사용합니다.

순서대로 누를 번호가 담긴 배열 numbers, 왼손잡이인지 오른손잡이인 지를 나타내는 문자열 hand가 매개변수로 주어질 때, 각 번호를 누른 엄지손가락이 왼손인 지 오른손인 지를 나타내는 연속된 문자열 형태로 return 하도록 solution 함수를 완성해주세요.

---

### 2-1. 문제 설명 - 제한사항

- numbers 배열의 크기는 1 이상 1,000 이하입니다.
- numbers 배열 원소의 값은 0 이상 9 이하인 정수입니다.
- hand는 "left" 또는 "right" 입니다.
  - "left"는 왼손잡이, "right"는 오른손잡이를 의미합니다.
- 왼손 엄지손가락을 사용한 경우는 L, 오른손 엄지손가락을 사용한 경우는 R을 순서대로 이어붙여 문자열 형태로 return 해주세요.

---

### 2-2. 문제 설명 - 입출력 예

| numbers                           | hand    | result        |
| --------------------------------- | ------- | ------------- |
| [1, 3, 4, 5, 8, 2, 1, 4, 5, 9, 5] | "right" | "LRLLLRLLRRL" |
| [7, 0, 8, 2, 8, 3, 1, 5, 7, 6, 2] | "left"  | "LRLLRRLLLRR" |
| [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]    | "right" | "LLRLLRLLRL"  |

---

### 2-3. 문제 설명 - 입출력 예에 대한 설명

**입출력 예 #1**

순서대로 눌러야 할 번호가 [1, 3, 4, 5, 8, 2, 1, 4, 5, 9, 5]이고, 오른손잡이입니다.

| 왼손 위치 | 오른손 위치 | 눌러야 할 숫자 | 사용한 손 | 설명                                                             |
| --------- | ----------- | -------------- | --------- | ---------------------------------------------------------------- |
| \*        | #           | 1              | L         | 1은 왼손으로 누릅니다.                                           |
| 1         | #           | 3              | R         | 3은 오른손으로 누릅니다.                                         |
| 1         | 3           | 4              | L         | 4는 왼손으로 누릅니다.                                           |
| 4         | 3           | 5              | L         | 왼손 거리는 1, 오른손 거리는 2이므로 왼손으로 5를 누릅니다.      |
| 5         | 3           | 8              | L         | 왼손 거리는 1, 오른손 거리는 3이므로 왼손으로 8을 누릅니다.      |
| 8         | 3           | 2              | R         | 왼손 거리는 2, 오른손 거리는 1이므로 오른손으로 2를 누릅니다.    |
| 8         | 2           | 1              | L         | 1은 왼손으로 누릅니다.                                           |
| 1         | 2           | 4              | L         | 4는 왼손으로 누릅니다.                                           |
| 4         | 2           | 5              | R         | 왼손 거리와 오른손 거리가 1로 같으므로, 오른손으로 5를 누릅니다. |
| 4         | 5           | 9              | R         | 9는 오른손으로 누릅니다.                                         |
| 4         | 9           | 5              | L         | 왼손 거리는 1, 오른손 거리는 2이므로 왼손으로 5를 누릅니다.      |
| 5         | 9           | -              | -         |                                                                  |

따라서 "LRLLLRLLRRL"를 return 합니다.

**입출력 예 #2**

왼손잡이가 [7, 0, 8, 2, 8, 3, 1, 5, 7, 6, 2]를 순서대로 누르면 사용한 손은 "LRLLRRLLLRR"이 됩니다.

**입출력 예 #3**

오른손잡이가 [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]를 순서대로 누르면 사용한 손은 "LLRLLRLLRL"이 됩니다.

---

## 3. 문제 풀이

```js
function solution(numbers, hand) {
  // 1) 사용한 손가락을 의미하는 문자열 "R" 또는 "L"을 담을 배열
  var answer = [];

  // 2) 왼속 엄지손가락과 오른손 엄지손가락의 처음 위치
  var lefthands = 10;
  var righthands = 12;

  for (let number of numbers) {
    // 3) 눌러야 키패드가 0이라면 number의 값을 11로 바꾸기
    if (number === 0) {
      number = 11;
    }

    // 4) number의 값이 1또는 4또는 7인 경우
    if (number === 1 || number === 4 || number === 7) {
      answer.push("L");
      lefthands = number;

      // 5) number의 값이 3또는 6또는 9일 경우
    } else if (number === 3 || number === 6 || number === 9) {
      answer.push("R");
      righthands = number;

      // 5) number의 값이 2또는 5또는 8또는 0(11)일 경우
    } else {
      // 6) 양손의 엄지손가락 부터 눌러야 할 키패드까지의 거리 구하기
      const leftDistance =
        lefthands === 2 ||
        lefthands === 5 ||
        lefthands === 8 ||
        lefthands === 11
          ? Math.abs(lefthands - number) / 3
          : lefthands - number > 0
          ? Math.floor((lefthands - number) / 3) + 2
          : Math.floor(Math.abs(lefthands - number) / 3) + 1;
      const rightDistance =
        righthands === 2 ||
        righthands === 5 ||
        righthands === 8 ||
        righthands === 11
          ? Math.abs(righthands - number) / 3
          : righthands - number > 0
          ? Math.floor((righthands - number) / 3) + 1
          : Math.floor(Math.abs(righthands - number) / 3) + 2;

      // 7) 거리를 비교하여 눌러야 할 엄지 손가락 정하기
      if (leftDistance > rightDistance) {
        answer.push("R");
        righthands = number;
      } else if (leftDistance < rightDistance) {
        answer.push("L");
        lefthands = number;
      } else {
        if (hand === "right") {
          answer.push("R");
          righthands = number;
        } else {
          answer.push("L");
          lefthands = number;
        }
      }
    }
  }

  // 8) Array.join()를 사용하여 배열을 문자열로 반환하기
  return answer.join("");
}
```

---

### 1) 사용한 손가락을 의미하는 문자열 "R" 또는 "L"을 담을 배열

```js
var answer = [];
```

아래의 반복문을 통해 사용한 손가락을 의미하는 문자열 "R"또는 "L"이 `answer`배열에 순서대로 위치하게 한다.

---

### 2) 왼속 엄지손가락과 오른손 엄지손가락의 처음 위치

```js
var lefthands = 10;
var righthands = 12;
```

왼손 엄지손가락의 처음 위치는 \*이고 오른손 엄지손가락의 처음 위치는 #이다. 이를 각각 숫자 10과 12의 값으로 바꾸어 변수에 할당을 하였다. 그 이유는 눌러야 할 키패드와 손가락의 거리를 계산을 구하기 위함이다.

---

### 3) 눌러야 키패드가 0이라면 number의 값을 11로 바꾸기

```js
if (number === 0) {
  number = 11;
}
```

`numbers`의 배열을 반복문을 돌면서 가장 먼저 해야 할 일은 눌러야 할 키패드가 0이라면 이를 11로 바꾸어 주는 것이다. 이러한 이유는 `2) 왼속 엄지손가락과 오른손 엄지손가락의 처음 위치`에서 \*을 10, #을 11로 바꾸어 주는 것과 같다.(거리 계산을 위해)

---

### 4) number의 값이 1또는 4또는 7인 경우

```js
if (number === 1 || number === 4 || number === 7) {
  answer.push("L");
  lefthands = number;
}
```

반복문에 들어와서 세 가지의 경우를 생각해야 한다. 그 중 첫번째로 `numbers`의 원소 중 값이 1, 4, 7인 경우이다. 이 경우에는 왼쪽 엄지손가락으로 눌러야 한다. 그렇기 때문에 `answer`배열에 문자열 "L"을 넣고 `lefthands`의 위치도 1또는 4또는 7로 바꾼다.

---

### 5) number의 값이 3또는 6또는 9일 경우

```js
else if (number === 3 || number === 6 || number === 9) {
      answer.push("R");
      righthands = number;
}
```

두 번째 경우 `numbers`의 원소가 3, 6, 9인 경우이다. 이 경우엔 오른쪽 엄지손가락이므로 `answer`배열에 문자열 "R"을 널고 `righthands`을 3또는 6또는 9로 수정한다.

---

### 6) 양손의 엄지손가락 부터 눌러야 할 키패드까지의 거리 구하기

마지막 경우는 `numbers`의 원소가 2, 5, 8, 0(11)인 경우이다. 해당 경우에는 양손의 엄지손가락과 눌러야 할 키패드의 거리를 계산해야 한다. 계산한 거리를 바탕으로 가까운 거리의 엄지 손가락으로 누른다. 만약 거리가 같다면 어느 손 잡이인지에 따라 손가락이 정해진다.

> 엄지손가락은 상하좌우 4가지 방향으로만 이동할 수 있으며 키패드 이동 한 칸은 거리로 1에 해당합니다.

```js
const leftDistance =
  lefthands === 2 || lefthands === 5 || lefthands === 8 || lefthands === 11
    ? Math.abs(lefthands - number) / 3
    : lefthands - number > 0
    ? Math.floor((lefthands - number) / 3) + 2
    : Math.floor(Math.abs(lefthands - number) / 3) + 1;
const rightDistance =
  righthands === 2 || righthands === 5 || righthands === 8 || righthands === 11
    ? Math.abs(righthands - number) / 3
    : righthands - number > 0
    ? Math.floor((righthands - number) / 3) + 1
    : Math.floor(Math.abs(righthands - number) / 3) + 2;
```

일단 위의 코드에서 사용하고 있는 `Math`와 과련된 메서드를 살펴보자.

1. Math.floor(): 주어진 수 이하의 가장 큰 정수를 반환한다. 쉽게 말해 소수점 아래의 수를 버림한다.
2. Math.abs(): 주어진 수의 절대값을 반환한다.

왼손 엄지손가락, 오른손 엄지손가락의 위치가 2, 5, 8, 0(11)일 때 거리를 구하는 방법은 같다. 현재의 위치의 숫자에서 다음에 눌러야 할 숫자를 뺀 값의 절대값에 3을 나누면 된다.

```js
Math.abs(lefthands - number) / 3;
```

왼손 엄지손가락, 오른속 엄지손가락의 위치가 2, 5, 8, 0(11)이 아니라면 거리를 구하는 방법이 살짝 달라져 몇 가지의 조건을 나누어 계산해야 한다. 아래의 사진 왼쪽 엄지손가락의 위치가 7일때와 오른쪽 엄지손가락의 6일 때 각각의 거리를 계산한 과정을 그림으로 나타낸 것이다.

![programmers_keypad_press_note](/image/CodingTest/programmers_keypad_press/programmers_keypad_press_note.jpg)

위의 사진에서 보이는 2번까지의 과정은 모두 동일하다. 그 후 달라지는 조건을 살펴보자.

- 왼손 엄지손가락
  - 왼손 엄지손가락의 숫자 - 다음 눌러야 할 숫자 > 0: 마지막 값 + 2
  - 왼손 엄지손가락의 숫자 - 다음 눌러야 할 숫자 < 0: 마지막 값 + 1
- 오른손 엄지손가락
  - 오른속 엄지손가락의 숫자 - 다음 눌러야 할 숫자 > 0: 마지막 값 + 1
  - 오른속 엄지손가락의 숫자 - 다음 눌러야 할 숫자 < 0: 마지막 값 + 2

위의 4가지의 조건을 바탕으로 그 다음으로 눌러야 할 숫자와의 거리를 계산하였다.

그리고 구한 거리는 각각의 `leftDistance`와 `rightDistance`에 할당하였다.

---

### 7) 거리를 비교하여 눌러야 할 엄지 손가락 정하기

거리를 계산하였으니 어느 엄지손가락이 더 까운지 계산하여 `anwser`배열에 적절한 값을 넣어야 한다.

```js
if (leftDistance > rightDistance) {
  answer.push("R");
  righthands = number;
} else if (leftDistance < rightDistance) {
  answer.push("L");
  lefthands = number;
} else {
  if (hand === "right") {
    answer.push("R");
    righthands = number;
  } else {
    answer.push("L");
    lefthands = number;
  }
}
```

rightDistance가 더 짧을 때 문자열 "R"을 `answer`배열에 넣고 leftDistance가 더 짧을 때에는 문자열 "L"을 `answer`배열에 넣는다. 하지만 두 거리가 모두 같을 때에는 `hand`의 값을 보고 오른손잡일 경우 "R", 왼손잡일 경우 "L"을 `answer`배열에 넣는다.

그리고 움직인 엄지손가락의 위치를 바꾼다.

---

### 8) Array.join()를 사용하여 배열을 문자열로 반환하기

`answer`은 배열이고 그 안에 "R", "L"이 들어있다. 이를 문자열로 바꿔주기 위해 `Array.join()`메서드를 이용한다.

```js
return answer.join("");
```

---

### 결과

![programmers_keypad_press_result](/image/CodingTest/programmers_keypad_press/programmers_keypad_press_result.png)

---

## 4. Refactoring

각각의 엄지손가락의 위치부터 이동해야 할 위치까지의 거리를 구하는 코드를 수정하고 싶다. 거리를 구하는 함수를 만들어보고 중복되는 코드도 함수로 작성하여 함수를 호출할 수 있도록 리팩토링을 하였다.

기본적인 로직을 같지만 이전보다 가독성은 조금 더 괜찮아지지 않았을까? 싶다.

```js
function getDistance(lefthands, righthands, number) {
  var leftDistance;
  var rightDistance;

  const leftDif = Math.floor(Math.abs(lefthands - number) / 3);
  const rigthDif = Math.floor(Math.abs(righthands - number) / 3);

  if (
    lefthands === 2 ||
    lefthands === 5 ||
    lefthands === 8 ||
    lefthands === 11
  ) {
    leftDistance = leftDif;
  } else if (lefthands > number) {
    leftDistance = leftDif + 2;
  } else {
    leftDistance = leftDif + 1;
  }

  if (
    righthands === 2 ||
    righthands === 5 ||
    righthands === 8 ||
    righthands === 11
  ) {
    rightDistance = rigthDif;
  } else if (righthands > number) {
    rightDistance = rigthDif + 1;
  } else {
    rightDistance = rigthDif + 2;
  }
  return [leftDistance, rightDistance];
}

function solution(numbers, hand) {
  var answer = [];

  var lefthands = 10;
  var righthands = 12;

  function pushLefthands(number) {
    answer.push("L");
    lefthands = number;
  }

  function pushRighthands(number) {
    answer.push("R");
    righthands = number;
  }

  for (let number of numbers) {
    if (number === 0) {
      number = 11;
    }
    if (number === 1 || number === 4 || number === 7) {
      pushLefthands(number);
      continue;
    } else if (number === 3 || number === 6 || number === 9) {
      pushRighthands(number);
      continue;
    }

    const [leftDistance, rightDistance] = getDistance(
      lefthands,
      righthands,
      number
    );

    if (leftDistance > rightDistance) {
      pushRighthands(number);
    } else if (leftDistance < rightDistance) {
      pushLefthands(number);
    } else {
      if (hand === "right") {
        pushRighthands(number);
      } else {
        pushLefthands(number);
      }
    }
  }

  return answer.join("");
}
```

![programmers_keypad_press refactoring result](/image/CodingTest/programmers_keypad_press/programmers_keypad_press_result2.png)

실행속도가 빨라진 것을 볼 수 있다!

---

## 5. 다른 사람의 풀이

다른 사람의 풀이 중 2차원 배열을 통해 현재의 위치와 다음의 위치까지의 거리를 구하는 방법을 사용한 풀이를 가져왔다.

```js
const solution = (numbers, hand) => {
  const answer = [];

  let leftHandPosition = "*";
  let rightHandPosition = "#";

  numbers.forEach((number) => {
    if (number === 1 || number === 4 || number === 7) {
      answer.push("L");
      leftHandPosition = number;
      return;
    }

    if (number === 3 || number === 6 || number === 9) {
      answer.push("R");
      rightHandPosition = number;
      return;
    }

    const leftHandDistance = getDistance(leftHandPosition, number);
    const rightHandDistance = getDistance(rightHandPosition, number);

    if (leftHandDistance === rightHandDistance) {
      if (hand === "right") {
        answer.push("R");
        rightHandPosition = number;
        return;
      }

      if (hand === "left") {
        answer.push("L");
        leftHandPosition = number;
        return;
      }
    }

    if (leftHandDistance > rightHandDistance) {
      answer.push("R");
      rightHandPosition = number;
    }

    if (leftHandDistance < rightHandDistance) {
      answer.push("L");
      leftHandPosition = number;
    }
  });

  return answer.join("");
};

const getDistance = (locatedNumber, target) => {
  const keyPad = {
    1: [0, 0],
    2: [0, 1],
    3: [0, 2],
    4: [1, 0],
    5: [1, 1],
    6: [1, 2],
    7: [2, 0],
    8: [2, 1],
    9: [2, 2],
    "*": [3, 0],
    0: [3, 1],
    "#": [3, 2],
  };

  const nowPosition = keyPad[locatedNumber];
  const targetPosition = keyPad[target];

  return (
    Math.abs(targetPosition[0] - nowPosition[0]) +
    Math.abs(targetPosition[1] - nowPosition[1])
  );
};
```

위의 코드에서 `getDistance()`함수를 살펴보면 내가 작성한 코드보다 훨씬 간결하고 가독성도 뛰어난 것을 볼 수 있다. x, y좌표의 차이의 절대값을 더하면 간단하게 거리를 구할 수 있다. 옛날 중학생 때 배웠던 2차 함수에서의 거리 계산이 떠오른다. 이렇게 2차원 배열로 생각을 하면 훨씬 간단하게 거리를 구할 수 있으니 참고하자.

## 6. Conclusion

> 코딩 테스트 문제는 막상 풀고 나면 쉬운 문제라고 생각이 드는데 푸는 순간에는 지금까지 겪었던 문제 중 가장 어렵고 난해한 문제라고 생각한다. 그래서 조급함이 찾아오는 것 같고 그로인해 시간이 오바되거나 오류 투성이인 결과를 보는 거 같다. 실력을 키우는 것도 좋지만 코딩 테스트를 볼 때의 마음가짐을 잘 정돈하자.

---

[👆](#카카오-인터-키패드-누르기)
