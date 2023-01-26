# 검색 알고리즘

## 1. 선형 검색(Linear Search)

선형 검색이란 처음부터 또는 그 반대인 마지막 부터 순서대로 내가 원하는 값이 맞는지 확인하며 진행하는 검색이다. 이런 선형 검색은 데이터가 정렬되지 않았을 때 사용할 수 있는 가장 좋은 방법이다.

자바스크립트에는 선형 검색을 도와주는 다양한 내장 메서드들이 있다.

- indexOf
- lastIndexOf
- includes
- find
- findIndex

위 메서드와 관련한 내용은 아래의 TIL를 참고

- [원하는 인덱스 찾기(indexOf, lastIndexOf, findIndex)](/JAVASCRIPT/ArrayMethod/FindIndex.md)
- [배열에 특정 요소가 있는지 확인하기(includes)](/JAVASCRIPT/ArrayMethod/Includes.md)

그렇다면 선형 검색의 시간 복잡도는 어떻게 될까? 운이 좋으면 `O(1)`의 시간이 걸린다. 하지만 이건 굉장히 운이 좋을 경우이고 우리는 항상 최악의 시간을 고려해야 한다. 100만개의 데이터가 있다고 가정하자. 만약 우리가 찾는 데이터가 100만번 째의 데이터거나 내가 찾고자 하는 데이터가 없다면 모든 데이터를 찾아야 한다. 이때 걸리는 시간은 `O(n)`이다. 그러므로 선형 검색은 `O(n)`의 시간 복잡도를 가지고 있다.

---

## 2. 이진 검색(Binary Search)

선형 검색은 처음 부터 하나하나 씩 값을 비교한다면 이진 검색은 값을 비교할 때 마다 남은 항목의 절반을 제거할 수 있다. 단, 이진 검색을 사용하기 위해선 데이터가 정렬이 되어 있어야 한다.

정렬이 된 데이터를 검색하는 중요한 포인트는 데이터의 중간점을 비교하는 것이다. 중간점을 내가 찾고자 하는 데이터와 비교하여 큰지 작은지를 비교하면서 값을 찾아나간다. 그러므로 비교할 때 마다 데이터의 절반이 제거된다.

즉, 이진 검색의 기본적인 개념은 `분할 정복(Divide and Conquer)`이다. `분할 정복`에 대한 개념은 아래 TIL의 #5에서 확인할 수 있다.

- [문제 해결 패턴](/DataStructureAlgorithm/SolvingPattern.md)

이진 검색의 시간복잡도는 최악의 경우 O(log n)이다. 절반씩 감소하기 때문이다.

---

### 2-1. 이진 검색 의사 코드

```javascript
function binarySearch(array, val) {
  let left = 0;
  let right = array.length - 1;

  while (left <= right) {
    let mid = Math.floor((left + right) / 2);
    if (array[mid] < val) left = mid + 1;
    else right = mid - 1;

    if (array[mid] === val) return mid;
  }

  return -1;
}
```

`left`와 `right` 그기로 `mid`라는 포인터를 두고 값을 찾아가는 과정이다. 반복문은 `left`가 `right`보다 커지게 되면 종료한다.

---

## 3. 나이브 문자열 검색(Naive String Search)

나이브 문자열 검색은 긴 문자열 내에 짧은 문자열을 찾을 때 사용되는 방법이다. 개인적으로 정규식을 사용하면 어떨까? 하는 생각이 든다.

### 3-1. 나이브 문자열 검색 의사 코드

```javascript
function naiveSearch(long, short) {
  let count = 0;
  for (let i = 0; i < long.length; i++) {
    for (let j = 0; j < short.length; j++) {
      if (short[j] !== long[i + j]) break;
      if (j === short.length - 1) count += 1;
    }
  }

  return count;
}
```

정규식을 이용한 의사 코드는 아래와 같다.

```javascript
function naiveSearch(long, short) {
  const regexp = new RegExp(short, 'g');
  return long.match(regexp).length;
}
```

---

## 4. Conclusion

> 강의를 들으며 검색 알고리즘에 대해 정리하였다. 해당 알고리즘은 어떤 원리로 동작하는지에 대해 알게 되었고 직접 의사 코드를 작성해본 것이 큰 도움이 되었다. 물론 선형 검색, 나이브 문자열 검색은 자바스크립트의 내장 메서드를 통해 구현을 할 수 있다. 하지만 너무 편리하고 익숙한 것만 취하지 말자. 원리를 알고 사용하는 것이 중요하다. 그렇기 때문에 강사님도 의사 코드를 모두 작성하면서 원리를 알려주시는 것 같다.

---

## 참고

Udemy - JavaScript 알고리즘 & 자료구조 마스터클래스 / 섹션 10: 검색 알고리즘

---

📅 2023-01-26
