# 문자열 압축

## 1. 개요

- 프로그래머스
- Lv.2
- 2020 KAKAO BLIND RECRUITMENT
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/60057)

---

## 2. 문제 설명

데이터 처리 전문가가 되고 싶은 "어피치"는 문자열을 압축하는 방법에 대해 공부를 하고 있습니다. 최근에 대량의 데이터 처리를 위한 간단한 비손실 압축 방법에 대해 공부를 하고 있는데, 문자열에서 같은 값이 연속해서 나타나는 것을 그 문자의 개수와 반복되는 값으로 표현하여 더 짧은 문자열로 줄여서 표현하는 알고리즘을 공부하고 있습니다.

간단한 예로 "aabbaccc"의 경우 "2a2ba3c"(문자가 반복되지 않아 한번만 나타난 경우 1은 생략함)와 같이 표현할 수 있는데, 이러한 방식은 반복되는 문자가 적은 경우 압축률이 낮다는 단점이 있습니다. 예를 들면, "abcabcdede"와 같은 문자열은 전혀 압축되지 않습니다. "어피치"는 이러한 단점을 해결하기 위해 문자열을 1개 이상의 단위로 잘라서 압축하여 더 짧은 문자열로 표현할 수 있는지 방법을 찾아보려고 합니다.

예를 들어, "ababcdcdababcdcd"의 경우 문자를 1개 단위로 자르면 전혀 압축되지 않지만, 2개 단위로 잘라서 압축한다면 "2ab2cd2ab2cd"로 표현할 수 있습니다. 다른 방법으로 8개 단위로 잘라서 압축한다면 "2ababcdcd"로 표현할 수 있으며, 이때가 가장 짧게 압축하여 표현할 수 있는 방법입니다.

다른 예로, "abcabcdede"와 같은 경우, 문자를 2개 단위로 잘라서 압축하면 "abcabc2de"가 되지만, 3개 단위로 자른다면 "2abcdede"가 되어 3개 단위가 가장 짧은 압축 방법이 됩니다. 이때 3개 단위로 자르고 마지막에 남는 문자열은 그대로 붙여주면 됩니다.

압축할 문자열 s가 매개변수로 주어질 때, 위에 설명한 방법으로 1개 이상 단위로 문자열을 잘라 압축하여 표현한 문자열 중 가장 짧은 것의 길이를 return 하도록 solution 함수를 완성해주세요.

---

### 2-1. 문제 설명 - 제한사항

- s의 길이는 1 이상 1,000 이하입니다.
- s는 알파벳 소문자로만 이루어져 있습니다.

---

### 2-2. 문제 설명 - 입출력 예

| s                          | result |
| -------------------------- | ------ |
| "aabbaccc"                 | 7      |
| "ababcdcdababcdcd"         | 9      |
| "abcabcdede"               | 8      |
| "abcabcabcabcdededededede" | 14     |
| "xababcdcdababcdcd"        | 17     |

---

### 2-3. 문제 설명 - 입출력 예에 대한 설명

입출력 예 #1

문자열을 1개 단위로 잘라 압축했을 때 가장 짧습니다.

입출력 예 #2

문자열을 8개 단위로 잘라 압축했을 때 가장 짧습니다.

입출력 예 #3

문자열을 3개 단위로 잘라 압축했을 때 가장 짧습니다.

입출력 예 #4

문자열을 2개 단위로 자르면 "abcabcabcabc6de" 가 됩니다.
문자열을 3개 단위로 자르면 "4abcdededededede" 가 됩니다.
문자열을 4개 단위로 자르면 "abcabcabcabc3dede" 가 됩니다.
문자열을 6개 단위로 자를 경우 "2abcabc2dedede"가 되며, 이때의 길이가 14로 가장 짧습니다.

입출력 예 #5

문자열은 제일 앞부터 정해진 길이만큼 잘라야 합니다.
따라서 주어진 문자열을 x / ababcdcd / ababcdcd 로 자르는 것은 불가능 합니다.
이 경우 어떻게 문자열을 잘라도 압축되지 않으므로 가장 짧은 길이는 17이 됩니다.

---

## 3. 문제 풀이

```javascript
function solution(s) {
  // 1) 문자열을 자를 수 있는 최대 단위와 문자열의 처음 길이
  const max_unit = Math.floor(s.length / 2);
  let answer = s.length;

  // 2) 문자열을 한 문자씩 나누어 배열로 만들기
  const s_array = [...s];

  for (let i = 1; i <= max_unit; i++) {
    // 3) 문자열을 1~max_unit 단위로 나누기
    let array = [];
    let index = 0;
    s_array.forEach((item) => {
      if (!array[index]) {
        array[index] = item;
      } else if (array[index].length < i) {
        array[index] += item;
      } else {
        index += 1;
        array[index] = item;
      }
    });
    array.reverse();

    // 4) 압축된 새로운 문자열 만들기
    let new_answer = "";
    let pre_item = array.pop();
    let repeat = 1;
    while (array.length !== 0) {
      const item = array.pop();
      if (pre_item === item) {
        repeat += 1;
      } else {
        const add = repeat === 1 ? pre_item : repeat + pre_item;
        new_answer += add;
        pre_item = item;
        repeat = 1;
      }
      if (array.length === 0) {
        const add = repeat === 1 ? pre_item : repeat + pre_item;
        new_answer += add;
      }
    }

    // 5) 더 짧은 문자열의 길이로 바꾸기
    answer = answer > new_answer.length ? new_answer.length : answer;
  }

  return answer;
}
```

---

### 1) 문자열을 자를 수 있는 최대 단위와 문자열의 처음 길이

```javascript
const max_unit = Math.floor(s.length / 2);
let answer = s.length;
```

문자열을 자를 수 있는 최대 단위는 문자열의 길이를 2로 나눈 값이다. 하지만 만약 문자열의 길이가
홀수이면 나눈 값의 나머지를 버려야하기 때문에 `Math.floor()` 메서드를 사용하였다.

처음 받은 문자열의 길이를 변수에 담은 이유는 만약 문자열이 압축을 할 수 없는 경우 가장 짧은
길이의 문자열은 처음 받은 `s` 문자열의 길이이기 때문인다. 해당 변수의 값은 아래의 로직에서 특정
조건에 따라 달라질 수 있다.

---

### 2) 문자열을 한 문자씩 나누어 배열로 만들기

```javascript
const s_array = [...s];
```

배열를 사용하여 문자열를 단위별로 묶고 앞 뒤의 문자가 같은지 확인하려고 한다.
문자열도 이터러블이기 때문에 위와 같은 방법으로 문자열르 가지고 배열를 만들 수 있다.

---

### 3) 문자열을 1~max_unit 단위로 나누기

```javascript
let array = [];
let index = 0;
s_array.forEach((item) => {
  if (!array[index]) {
    array[index] = item;
  } else if (array[index].length < i) {
    array[index] += item;
  } else {
    index += 1;
    array[index] = item;
  }
});
array.reverse();
```

`max_unit`의 값 만큼 반복문을 실행한다. 반복문으로 들어오게 되면 첫 번째로 실행하는 것이 바로 `3) 문자열을 1~max_unit 단위로 나누기`이다.

`array` 배열에 단위의 크기만큼 나뉜 요소가 들어가게 된다. 예를들어 단위가 4이면 문자열을 앞에서부터 4개씩 잘라 배열에 들어간다.
이때 각 요소의 길이는 4이다. `array` 배열은 위에서 문자열로 만든 `s_array` 배열을 `Array.forEach()` 반복문을 실행하여 만든다.

아래는 `s_array.forEach()`의 과정이다.

1. `array` 배열이 비어있을 때

   ```javascript
   if (!array[index]) {
     array[index] = item;
   }
   ```

   해당 조건문은 `array` 배열이 비어있을 때 즉, 아무런 요소가 없을 때 실행된다. 여기서 `array[0]`이 만들어진다.

2. `array` 요소의 길이가 단위보다 작을 때

   ```javascript
    else if (array[index].length < i) {
    array[index] += item;
   }
   ```

   `array` 요소의 길이가 단위보다 작을 때에는 해당 요소에 계속 문자를 붙일 수 있다.

3. `array` 요소의 길이가 단위랑 같을 때

   ```javascript
   else {
    index += 1;
    array[index] = item;
   }
   ```

   마지막은 `array` 요소의 길이가 단위랑 같을 때이다. 이때 `index`의 값을 1올려 `array` 배열의 요소를 하나 더 추가한다.
   그리고 해당 요소에 들어온 문자를 넣는다.

4. array.reverse()

   `array` 배열이 생성되었으면 `Array.reverse()` 메서드를 사용하여 순서를 바꾼다. 이는 요소를 가져올 때 앞에서 하나씩 제거하여 가져오는 것이 아니라
   뒤에서 하나씩 제거하면서 가져오기 위함이다.

---

### 4) 압축된 새로운 문자열 만들기

```javascript
let new_answer = "";
let pre_item = array.pop();
let repeat = 1;
while (array.length !== 0) {
  const item = array.pop();
  if (pre_item === item) {
    repeat += 1;
  } else {
    const add = repeat === 1 ? pre_item : repeat + pre_item;
    new_answer += add;
    pre_item = item;
    repeat = 1;
  }
  if (array.length === 0) {
    const add = repeat === 1 ? pre_item : repeat + pre_item;
    new_answer += add;
  }
}
```

문자열을 1~max_unit 단위로 나누어 `array` 배열을 생성하였다. 그 후 해야 할 일은 앞 뒤로 같은 문자열이 있으면 압축을 해야
하는 것이다. 이때 얼마만큼 압축을 했는지를 문자열 앞에 숫자로 나타내야 한다.

- `new_answer`은 새롭게 압축된 문자열이다.
- `pre_item`은 반복문에서 현재 요소(문자열)와 비교 할 이전 요소(문자열)이다. 기본값은 가장 뒤에 위치한 요소이다.
- `repeat`은 요소(문자열)가 얼마만큼 압축되었는지 알려주는 역할을 한다. 기본값은 1이다.
- `while`문을 통해 `array` 배열의 길이가 0이 될 때 까지 반복문을 실행한다.
- 주의해야 할 점은 이전 단계에서 `array` 배열의 순서를 바꾸었으므로 `Array.pop()` 메서드는 앞에서 부터 요소를 가져오는
  역할을 한다.
- 요소 = 문자열

1. `array` 배열의 마지막 요소 가져오기

   ```javascript
   const item = array.pop();
   ```

   반복문은 시작에서는 `array` 배열의 마지막 요소를 가져온다. 해당 요소와 이전 요소를 가지고 있는 `pre_item`를 비교한다.

2. 이전 요소와 현재의 요소가 같을 경우

   ```javascript
   if (pre_item === item) {
     repeat += 1;
   }
   ```

   해당 경우에는 `repeat`을 1 증가시킨다.

3. 이전 요소와 현재의 요소가 다를 경우

   ```javascript
    else {
    const add = repeat === 1 ? pre_item : repeat + pre_item;
    new_answer += add;
    pre_item = item;
    repeat = 1;
   }
   ```

   해당 경우에는 `new_answer`에 새로운 문자열을 추가한다. 이때 주의해야 할 점은 `repeat`이 1이라면 1은 문자열에 포함시키지
   않는다. 즉, 두 번 이상 압축된 경우에만 `repeat`를 붙인다.

   추가한 후 `pre_item`를 현재의 요소로 바꾸고 `repeat`를 다시 1로 초기화 한다.

4. 반복문을 종료하기 직전인 경우

   ```javascript
   if (array.length === 0) {
     const add = repeat === 1 ? pre_item : repeat + pre_item;
     new_answer += add;
   }
   ```

   `array` 배열의 길이가 0이면 해당 반복문이 마지막 이라는 것이다. 그렇기 때문에 현재 남아있는 `new_answer`과 `repeat`를
   이용하여 `new_answer`에 새로운 문자열을 추가한다. 추가하는 방법은 3번의 과정과 같다.

---

### 5) 더 짧은 문자열의 길이로 바꾸기

```javascript
answer = answer > new_answer.length ? new_answer.length : answer;
```

새롭게 만들어진 문자열의 길이가 기존의 문자열의 길이보다 짧다면 `answer`의 값을 바꾸고 그렇지 않으면 기존의 `answer`를 유지한다.
문제에서 원하는 정답은 가장 짧은 문자열의 길이이기 때문이다.

---

### 결과

![programmers_string_compression_result](/image/CodingTest/programmers_string_compression/programmers_string_compression_result1.png)

---

## 4. Refactoring

```javascript
function solution(s) {
  let answer = s.length;

  for (let i = 1; i <= Math.floor(s.length / 2); i++) {
    let array = [s[0]];
    let index = 0;
    for (let j = 1; j < s.length; j++) {
      if (array[index].length < i) {
        array[index] += s[j];
      } else {
        index += 1;
        array[index] = s[j];
      }
    }

    let new_answer = "";
    let pre_item = array[0];
    let repeat = 1;
    for (let j = 1; j < array.length; j++) {
      if (pre_item === array[j]) {
        repeat += 1;
      } else {
        new_answer += repeat === 1 ? pre_item : repeat + pre_item;
        pre_item = array[j];
        repeat = 1;
      }
    }
    new_answer += repeat === 1 ? pre_item : repeat + pre_item;
    answer = answer > new_answer.length ? new_answer.length : answer;
  }

  return answer;
}
```

1. `max_unit` 변수 제거

   `max_unit` 변수를 제거하였다. 만약 해당 변수가 여러 곳에서 사용된다면 제거하지 않았겠지만 딱 한 곳에서만 사용하기 때문에 제거하였다.

2. `s_array` 변수 제거

   `s_array` 변수를 제거하면서 반복문에서 `array` 배열을 만드는 과정을 아래와 같이 수정하였다.

   ```javascript
   let array = [s[0]];
   let index = 0;
   for (let j = 1; j < s.length; j++) {
     if (array[index].length < i) {
       array[index] += s[j];
     } else {
       index += 1;
       array[index] = s[j];
     }
   }
   ```

3. `while` 문을 `for` 문으로 변경
   `while` 문을 사용하여 반복을 하지 않고 `for` 문을 사용하여 반복을 하였다. 이 과정에서 마지막 순서일 때의 과정을 제거하는 대신
   반복문이 끝날 때 남아있는 `pre_item` 값과 `repeat` 값을 사용하여 `new_answer` 에 새로운 문자열을 추가하였다.

   ```javascript
   let new_answer = "";
   let pre_item = array[0];
   let repeat = 1;
   for (let j = 1; j < array.length; j++) {
     if (pre_item === array[j]) {
       repeat += 1;
     } else {
       new_answer += repeat === 1 ? pre_item : repeat + pre_item;
       pre_item = array[j];
       repeat = 1;
     }
   }
   new_answer += repeat === 1 ? pre_item : repeat + pre_item;
   ```

---

### 결과

![programmers_string_compression_result2](/image/CodingTest/programmers_string_compression/programmers_string_compression_result2.png)

---

## 5. Refactoring2

다른 사람의 풀이를 보는 중 문자열의 메서드인 `String.slice()`를 사용하여 푼 풀이가 보였다. 그래서 굳이 배열을 사용하지 않고 문자열로만 사용하여
문제를 다시 풀어보기로 하였다.

```javascript
function solution(s) {
  let answer = s.length;

  for (let i = 1; i <= s.length / 2; i++) {
    let new_answer = "";
    let pre_string = s.slice(0, i);
    let repeat = 1;

    for (let j = 1; j < Math.ceil(s.length / i); j++) {
      const string = s.slice(j * i, (j + 1) * i);
      if (string === pre_string) {
        repeat++;
      } else {
        new_answer += repeat === 1 ? pre_string : repeat + pre_string;
        pre_string = string;
        repeat = 1;
      }
    }
    new_answer += repeat === 1 ? pre_string : repeat + pre_string;
    answer = answer > new_answer.length ? new_answer.length : answer;
  }
  return answer;
}
```

훨씬 간단해진 것을 볼 수 있다.

`3. 문제 풀이`의 `3) 문자열을 1~max_unit 단위로 나누기`과 `4) 압축된 새로운 문자열 만들기`를 한 번에 구현했기 때문이다.

중심이 되는 과정은 아래와 같다.

- `String.slice()`의 첫 번째 인자와 두 번째 인자에 적절한 값을 넣어 각각의 단위로 나눈다.
- 나눈 단위를 앞뒤로 비교하여 같은 문자열이면 압축을 한다.
- `pre_string`에 미리 값을 넣어둔 까닭은 중첩된 반복문에서 조건을 추가하지 않기 위함이다. 만약 `pre_string` 초기값이 없다면
  `j`가 0일 때라는 조건을 추가해야 한다.
- 전체적인 로직은 `3. 문제 풀이`랑 같다.

---

### 결과

![programmers_string_compression_result3](/image/CodingTest/programmers_string_compression/programmers_string_compression_result3.png)

배열로 바꾸는 과정, 즉 필요없는 과정을 없애서 그런지 확실히 빨라진 실행 속도를 확인할 수 있다.

---

## 6. Conclusion

> 문자열로만 풀 수 있는 문제는 문자열만 이용하여 풀자. 굳이 배열로 바꾸고 다시 문자열로 바꾸는 작업을 최소화 하자. 이번 문제는 스스로 두 가지의 방법을
> 찾은 것 같아 뿌듯한 마음이 든다. 물론 처음 풀었을 땐 무려 1시간 30분이라는 시간이 걸렸다... 확실히 아직 속도 측면에서 많은 부족함이 있다.
> 효율성은 공부하면서 더 고민해보도록 하고 일단은 시간을 축약하는 것에 집중하자. 효율성이 떨어진다 해도 통과는 통과니까. 그래도 꾸준히 공부하다 보면 가까운 미래에는 시간과 효율성을
> 모두 잡을 수 있는 날이 올 것이다.

---

📅 2022-08-30
