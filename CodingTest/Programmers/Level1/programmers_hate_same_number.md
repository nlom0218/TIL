# 같은 숫자는 싫어

## 1. 개요

- 프로그래머스
- Lv.1
- 스택/큐
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/12906)

---

## 2. 문제 설명

배열 arr가 주어집니다. 배열 arr의 각 원소는 숫자 0부터 9까지로 이루어져 있습니다. 이때, 배열 arr에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려고 합니다. 단, 제거된 후 남은 수들을 반환할 때는 배열 arr의 원소들의 순서를 유지해야 합니다. 예를 들면,

- arr = [1, 1, 3, 3, 0, 1, 1] 이면 [1, 3, 0, 1] 을 return 합니다.
- arr = [4, 4, 4, 3, 3] 이면 [4, 3] 을 return 합니다.

배열 arr에서 연속적으로 나타나는 숫자는 제거하고 남은 수들을 return 하는 solution 함수를 완성해 주세요.

---

### 2-1. 문제 설명 - 제한사항

- 배열 arr의 크기 : 1,000,000 이하의 자연수
- 배열 arr의 원소의 크기 : 0보다 크거나 같고 9보다 작거나 같은 정수

---

### 2-2. 문제 설명 - 입출력 예

| arr             | answer    |
| --------------- | --------- |
| [1,1,3,3,0,1,1] | [1,3,0,1] |
| [4,4,4,3,3]     | [4,3]     |

---

### 2-3. 문제 설명 - 입출력 예 설명

입출력 예 #1,2  
문제의 예시와 같습니다.

---

## 3. 문제 풀이

```javascript
function solution(arr) {
  // 1) 연속적으로 나타나는 숫자를 제거하고 남은 수들이 요소로 있는 배열
  const answer = [];
  // 2) 반복문을 돌며 item이 answer 배열의 마지막 값과 다르다면 배열에 추가하기
  arr.forEach((item) => {
    if (answer[answer.length - 1] !== item) answer.push(item);
  });
  return answer;
}
```

---

### 1) 연속적으로 나타나는 숫자를 제거하고 남은 수들이 요소로 있는 배열

구하고자 하는 것은 배열 `arr`에 연속적으로 중복되는 숫자를 하나로 만드는 것이다. `arr` 배열에서 연속적으로 중복되는 숫자를 제거한 새로운 배열을 만들기 위해 `answer`을 선언한다.

```javascript
const answer = [];
```

---

### 2) 반복문을 돌며 item이 answer 배열의 마지막 값과 다르다면 배열에 추가하기

```javascript
arr.forEach((item) => {
  if (answer[answer.length - 1] !== item) answer.push(item);
});
```

반복문을 돌며 `arr` 배열의 요소를 `answer` 배열의 마지막 요소와 비교하여 `answer` 배열에 새롭게 추가하거나 추가하지 않고 다음 반복문으로 넘어간다.

이때 추가되는 조건은 `answer` 배열의 마지막 요소와 같지 않을 경우이다. 같은 경우엔 연속으로 같은 숫자이기 때문이다.

---

### 결과

![programmers_hate_same_number_result](/image/CodingTest/programmers_hate_same_number/programmers_hate_same_number_result1.png)

---

## 4. 다른 사람 풀이

간단하면서도 가독성이 좋은 코드를 가져왔다. 물론 댓글과 좋아요가 가장 많았다.

```javascript
function solution(arr) {
  return arr.filter((val, index) => val != arr[index + 1]);
}
```

`Array.filter()` 메서드를 사용하고 있는데 특정 값이 그 다음 값과 같지 않을
경우에만 리턴하는 배열의 요소로 저장한다.

---

## 5. Conclusion

> 처음에는 중복되는 모든 값들을 제거하는 문제인 줄 알아서 셋(Set)을 활용했지만 다시 문제를 읽어보니 중복되는
> 모든 값이 아니라 연속적으로 중복되는 값이여서 다시 문제를 풀었다. 그리고 나도 처음에는 다른 사람 풀이처럼
> `Array.filter()` 메서드를 생각했지만 풀이 방법이 바로 떠오르지 않아 반복문을 돌며 새로운 배열에 요소를
> 추가하는 방법으로 문제를 풀었다. 조금만 더 생각을 했더라면 어땠을까? 라는 아쉬움이 든다.

---

📅 2022-08-18
