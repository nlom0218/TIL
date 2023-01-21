# n^2 배열 자르기

## 1. 개요

- 프로그래머스
- Lv.2
- 월간 코드 챌린지 시즌3
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/87390)

---

## 2. 문제 설명

정수 n, left, right가 주어집니다. 다음 과정을 거쳐서 1차원 배열을 만들고자 합니다.

1. n행 n열 크기의 비어있는 2차원 배열을 만듭니다.
2. i = 1, 2, 3, ..., n에 대해서, 다음 과정을 반복합니다.
   - 1행 1열부터 i행 i열까지의 영역 내의 모든 빈 칸을 숫자 i로 채웁니다.
3. 1행, 2행, ..., n행을 잘라내어 모두 이어붙인 새로운 1차원 배열을 만듭니다.
4. 새로운 1차원 배열을 arr이라 할 때, arr[left], arr[left+1], ..., arr[right]만 남기고 나머지는 지웁니다.

정수 n, left, right가 매개변수로 주어집니다. 주어진 과정대로 만들어진 1차원 배열을 return 하도록 solution 함수를 완성해주세요.

### 2-1. 문제 설명 - 제한사항

- 1 ≤ n ≤ 107
- 0 ≤ left ≤ right < n2
- right - left < 105

### 2-3. 문제 설명 - 입출력 예

| n   | left | right | result            |
| --- | ---- | ----- | ----------------- |
| 3   | 2    | 5     | [3,2,2,3]         |
| 4   | 7    | 14    | [4,3,3,3,4,4,4,4] |

### 2-4. 문제 설명 - 입출력 예 설명

입출력 예 #1

- 다음 애니메이션은 주어진 과정대로 1차원 배열을 만드는 과정을 나타낸 것입니다.

![입출력 예 #1](https://grepp-programmers.s3.amazonaws.com/production/file_resource/103/FlattenedFills_ex1.gif)

입출력 예 #2

- 다음 애니메이션은 주어진 과정대로 1차원 배열을 만드는 과정을 나타낸 것입니다.

![입출력 예 #2](https://grepp-programmers.s3.amazonaws.com/production/file_resource/104/FlattenedFills_ex2.gif)

---

## 3. 문제 풀이

```javascript
function solution(n, left, right) {
  let result = [];
  // 1) left부터 시작하여 right까지 이동하며 반복문 수행하기
  for (let idx = left; idx <= right; idx++) {
    // 2) 인덱스가 idx에 해당하는 요소의 행과 열 구하기
    const [i, j] = [Math.floor(idx / n), idx % n];
    // 3) 조건에 따라 인덱스가 idx에 해당하는 요소 구하기
    if (i >= j) result.push(i + 1);
    else result.push(j + 1);
  }

  return result;
}
```

### 1) left부터 시작하여 right까지 이동하며 반복문 수행하기

```javascript
for (let idx = left; idx <= right; idx++) {}
```

어떠한 규칙이 있는 배열에서 `left`에서 부터 `right`까지의 요소를 담은 새로운 배열을 반환해야 한다. 이때 `left`와 `right`은 인덱스이다.

### 2) 인덱스가 idx에 해당하는 요소의 행과 열 구하기

```javascript
const [i, j] = [Math.floor(idx / n), idx % n];
```

문제의 규칙에 의해 만들어지는 배열은 이중 중쳡 배열이다. 이러한 이중 중첩 배열에서 중첩을 제거한 배열을 바탕으로 요소를 찾아야 한다. 예를 들어, `[[1,2],[2,2]]`는 `[1,2,2,2]`가 된다.

내가 찾고자 하는 요소의 인덱스가 `idx`일 때 `idx`을 `n`으로 나눈 값을 통해 이중 중첩 배열에서 어디에 존재했다는 것을 알 수 있다. 이때 `n`은 행과 열의 수이다. 즉, `n`이 5일 때 5x5인 이중 중쳡 배열이 만들어 진다.

- 행: `idx`을 `n`으로 나눈 값의 몫
- 열: `idx`을 `n`으로 나눈 값의 나머지

그림을 통해 보면 훨씬 이해하기 쉽다. 예를 들어, `n`이 4인 4x4 이중 중첩 배열을 중첩을 제거한 배열이 있다.

![programmers_crop_array_analyze1](/image/CodingTest/programmers_crop_array/programmers_crop_array_%20analyze1.jpg)

배열을 `n`만큼 자르게 되면 행의 위치를 알 수 있다. 열의 위치는 각각의 행에서 해당 요소의 순서를 통해 알 수 있다. 1행 부터 시작해야 하지만 배열의 인덱스 순서와 같게 하기 위해 0행 부터 시작한다.

![programmers_crop_array_analyze2](/image/CodingTest/programmers_crop_array/programmers_crop_array_%20analyze2.jpg)

- `idx`가 7일 때
  - 몫은 1이므로 1행에 위치한다.
  - 나머지는 3이므로 행의 3번째에 위치한다.
  - 즉 `idx`가 7일 때 해당 요소는 이중 중첩 배열에서 1행 3열에 위치한 요소이다.(실제론 2행 4열)
- `idx`가 12일 때
  - 몫은 3이므로 3행에 위치한다.
  - 나머지는 0이므로 행의 0번째에 위치한다.
  - 즉 `idx`가 12일 때 해당 요소는 이중 중첩 배열에서 3행 0열에 위치한 요소이다.(실제론 3행 0열)

즉, `idx`를 통해 해당 요소가 이중 중첩 배열에서 어디에 위치해 있는지 알 수 있다.

### 3) 조건에 따라 인덱스가 idx에 해당하는 요소 구하기

```javascript
if (i >= j) result.push(i + 1);
else result.push(j + 1);
```

이중 중첩 배열이 만들어지는 조건을 정리하자면 각 행은 아래와 같은 규칙이있다. 편하게 행은 `i` 열은 `j`로 한다. 또한 `i`와 `j`가 0부터 시작하다는 것을 잊지 말아야 한다.

- `i`가 `j`보다 같거나 크다면 해당 요소의 값은 `i + 1`이다.
- `j`가 `i`보다 크다면 해당 요소의 값은 `j + 1`이다.

위의 규칙과 `idx`를 통해 구한 `i`와 `j`를 통해 `idx`에 해당하는 요소의 값을 구할 수 있다.

### 결과

![programmers_crop_array_result](/image/CodingTest/programmers_crop_array/programmers_crop_array_result.png)

---

## 4. 리펙터링

`i`와 `j`를 비교한 조건문을 아래와 같이 수정하였다.

```javascript
result.push(Math.max(i + 1, j + 1));
```

`Math.max()` 메서드를 사용하여 인자로 받은 값들 중 큰 값을 `result` 배열에 넣는다.

---

## 5. Conclusion

> 만약 이중 중첩 배열을 모두 만들고 `left`, `right` 사이에 존재하는 요소를 가져오는 풀이를 작성했다면 엄청난 시간이 걸려 시간 초과가 나타날 것이다.  
> 위의 풀이가 바로 내가 처음에 생각했던 풀이이다. 하지만 바로 실행 오류가 나왔고 어떻게 풀어야 하는지 고민을 하다 인덱스에 따른 규칙을 찾아보기로 하였다. 머릿속으로만 생각해서는 규칙이 잘 떠오르지 않아 직접 노트에 배열을 만들어 보며 규칙을 찾았다.  
> 규칙이 있는 배열은 처음부터 만들기 보다 규칙을 찾아보는 습관을 가지도록 하자.  
> 오랜만에 프로그래머스 Level2 문제였다. Level2 문제인 만큼 생각할 것들이 더 많아졌고 바로 한 번에 통과를 하지 못했다. 술술 잘 풀리는 문제보다 오히려 이런 문제가 더욱 재밌게 느껴진다. Level2도 파이팅하고 Level3으로 나아가자.

---

📅 2023-01-21
