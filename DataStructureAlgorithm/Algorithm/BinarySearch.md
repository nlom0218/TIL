# 이진 탐색(Binary Search)

## 1. 이진 탐색 알고리즘이란?

이진 탐색 알고리즘(Binary Search Algorithm)은 탐색 범위를 두 부분 리스트로 나눠 절반씩 좁혀가 필요한
부분에서만 탐색하도록 제한하여 원하는 값을 찾는 알고리즘이다.

![Binary Search Algorithm](https://velog.velcdn.com/images%2Fdevjade%2Fpost%2F8f61bd71-f637-4641-9521-17dd99cd5223%2Fimage.png)

이진 탐색 알고리즘의 장점으론 시간 복잡도가 O(log n)의 시간이 걸린다는 것이다.
이런 이진 탐색 알고리즘은 배열과 이진 탐색 트리로 구현할 수 있다.

---

## 2. 배열로 이진 탐색 알고리즘 구현하기

배열로 이진 탐색 알고리즘을 구현하기 위해서는 배열은 기본적으로 정렬이 되어 있어야 한며 각 `left`, `right`,
`mid`의 변수가 필요하다.

정렬된 배열의 중간에 있는 임의의 값을 선택하여 찾고자 하는 값 X와 비교한다. X가 중간 값보다 작으면 중간 값을 기준으로
좌측의 데이터들을 대상으로, X가 중간값보다 크면 배열의 우측을 대상으로 다시 탐색한다. 이런 탐색을
원하는 값을 찾을 때까지 반복한다.

---

### 2-1. 반복문을 통한 구현

```javascript
const binarySerach = (arr, target) => {
  let start = 0;
  let end = arr.length - 1;
  let mid;

  while (start <= end) {
    mid = parseInt((start + end) / 2);
    if (target == arr[mid]) {
      return mid;
    } else {
      if (target < arr[mid]) {
        end = mid - 1;
      } else {
        start = mid + 1;
      }
    }
  }
  return -1;
};
```

반복문을 통해 `target`이 `arr[mid]`의 값과 같다면 그 때의 `mid`를 반환한다. 이때의 `mid`는 `target`이
위치한 `index`가 된다.

하지만 `target`이 `arr[mid]`보다 작다면 `target`은 `mid`보다 왼쪽에 위치함으로 `end`의 값을 `mid`의
바로 왼쪽으로 바꾼다.

반대로 `target`이 `arr[mid]`보다 크다면 `target`은 `mid`보다 오른쪽에 위치함으로 `start`의 값을 `mid`의
바로 오른쪽으로 바꾼다.

```javascript
let arr = [];
for (let i = 1; i <= 240000; i++) {
  arr.push(i);
}

binarySerach(arr, 54644);
```

길이가 240000인 배열이 있다. 배열의 요소가 `54644`인 값을 이진 탐색으로 찾아보자. 또한 이진 탐색 함수의
반복문에 콘솔을 찍어 `start, end, mid`가 어떻게 변하는지 호출 횟수와 함께 살펴보자.

그러기 위해선 `binarySearch` 함수를 아래와 같이 바꾼 후 실행하여 콘솔을 보자.

```javascript
const binarySerach = (arr, target) => {
  // ...

  while (start <= end) {
    mid = parseInt((start + end) / 2);
    console.log(`호출 횟수: ${i}, start: ${start}, mid: ${mid}, end: ${end}`);
    // ...
    i++;
  }
  return -1;
};
```

![BinarySearch1](/image/DataStructureAlgorithm/Algorithm/BinarySearch/binarySearch1.png)

총 18번의 반복문을 통해 원하는 데이터의 인덱스를 찾을 수 있다. 만약 이진 탐색이 아니라 배열의 첫 번째 요소 부터
탐색을 했다면 총 54644번의 반복문을 실행을 해야했다.

---

### 2-1. 재귀 함수를 통한 구현

```javascript
const binarySerach = (start, end, mid, target) => {
  if (arr[mid] === target) return mid;
  if (start > end) return -1;
  if (arr[mid] < target) {
    start = mid + 1;
  } else {
    end = mid - 1;
  }
  return binarySerach(start, end, parseInt((start + end) / 2), target);
};
```

함수 내에서 자기 자신을 호출하는 `binarySearch` 함수는 재귀 함수이다. 특정 조건에선 특정 값을 리턴하지 않지만
그렇지 않으면 `start` 또는 `end` 값을 바꾸어 또 다시 `binarySearch` 함수를 호출한다.

`binarySearch` 함수 내의 조건문에 대해 살펴보자.

1. `arr[mid]`가 `target`가 같을 때: 원하는 값이므로 그 때의 `mid`를 반환한다.
2. `start`가 `end`보다 클 때: 있어서는 안 될 경우이기 때문에 `-1`를 반환한다.
3. `arr[mid]`가 `target` 보다 작을 때: `target`이 `mid` 보다 오른쪽에 있으므로 `start`값을 `mid`의 바로 오른쪽으로 바꾼다.
4. `arr[mid]`가 `target` 보다 클 때: `targe`이 `mid` 보다 왼쪽에 있으므로 `end`값을 `mid`의 바로 왼쪽으로 바꾼다.

재귀 함수로 통한 구현도 직접 콘솔로 찍어 확인해보자. 테스트 과정 콘솔을 추가한 전체적인 코드는 아래와 같다.

```javascript
let i = 1;
const binarySerach = (start, end, mid, target) => {
  console.log(`호출 횟수: ${i}, start: ${start}, mid: ${mid}, end: ${end}`);
  if (arr[mid] === target) return mid;
  if (start > end) return -1;
  if (arr[mid] < target) {
    start = mid + 1;
  } else {
    end = mid - 1;
  }
  i++;
  return binarySerach(start, end, parseInt((start + end) / 2), target);
};

let arr = [];
for (let i = 1; i <= 240000; i++) {
  arr.push(i);
}
binarySerach(0, arr.length - 1, parseInt((arr.length - 1) / 2), 168798);
```

길이가 240000인 배열 중 요소가 168798인 인덱스를 찾는 과정에서 `binarySearch` 함수는 아래의 사진에서 보는 것과 같이
단 17번만 호출된다. 만약 0번 인덱스 부터 찾았다면 무려 168798번을 반복해야 한다.

![BinarySearch2](/image/DataStructureAlgorithm/Algorithm/BinarySearch/binarySearch2.png)

---

## 참고

[[JS] 이진 탐색(이분 탐색)](https://gurtn.tistory.com/94)  
[JavaScript로 이진탐색(Binary Search) 알고리즘 구현하기](https://velog.io/@devjade/%EC%9D%B4%EC%A7%84%ED%83%90%EC%83%89-binary-search)
