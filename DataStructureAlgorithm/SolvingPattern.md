# 문제 해결 패턴

## 1. 패턴이란?

패턴은 일종의 프로그래밍 메커니즘이나 여러 요소를 넣을 수 있는 청사진이다. 이러한 패턴은 코딩 테스트 문제를 푸는데 많은 도움을 준다.

문제를 풀기 위한 많은 패턴들이 존재한다.

- Frequency Counter
- Multiple Pointers
- Sliding Window
- Divide and Conquer
- Dynamic Programming
- Greedy Algorithms
- Backtracking
- More...

물론 위의 패터만 달달 외운다 해도 모든 문제를 잘 풀 수 있진 않을 것이다. 하지만 패턴의 종류를 통해 문제 해결 접근에 조금이라도 도달 할 수 있다면 이는 유용한 지식이 될 것이다.

몇 가지 패턴을 살펴보자.

---

## 2. Frequency Counter(빈도수 세기 패턴)

빈도수 세기 패턴은 자바스크립트의 객체를 사용해서 다양한 값과 빈도를 수집하는 패턴이다.

### 2-1. 빈도수 세기 패턴 예제

예제를 통해 빈도수 세기 패턴을 파악해보자. 아래와 같은 문제가 있다.

> Wirte a function called `same`, which accepts two arrays. The function sholud return true if every value in the array has it's corresponding value squared in the second array. The frequency of values must be the same

문제를 살펴보자면 우리는 `same`이라는 함수를 만들어야 한다. 두 개의 배열을 인자로 받고 첫 번째 배열의 요소들의 제곱이 두 번째 배열의 요소로 존재를 하면 `ture`를 반환하고 그렇지 않으면 `false`를 반환하는 문제이다. 단, 등장하는 빈도수는 같아야 한다.

```javascript
same([1, 2, 3], [4, 1, 9]); // true
same([1, 2, 3], [1, 9]); // false
same([1, 2, 1], [4, 4, 1]); // false (빈도수가 반드시 같아야 한다.)
```

첫 번째 풀이는 빈도수 세기 패턴이 아닌 중첩 반복문을 통한 풀이이다.

```javascript
function same(arr1, arr2) {
  if (arr1.length !== arr2.length) {
    return false;
  }
  for (let i = 0; i < arr1.length; i++) {
    let correctIndex = arr2.indexOf(arr1[i] ** 2);
    if (correctIndex === -1) {
      return false;
    }
    arr2.splice(correctIndex, 1);
  }
  return true;
}
```

`for 문`을 사용하여 반복문을 실행하였고 내부에 `indexOf()` 메서드와 `splice()` 메서드를 사용하여 반복문을 다시 실행하였다. 그러므로 해당 방법은 `O(n^2)`의 시간 복잡도를 가진다.

이번에는 빈도수 세기 패턴을 통한 풀이를 살펴보자.

```javascript
function same(arr1, arr2) {
  // 배열의 길이가 다를 경우 false 반환하기
  if (arr1.length !== arr2.length) {
    return false;
  }

  // 각각의 배열을 순회하여 얻은 결과를 담을 객체 만들기
  let frequencyCounter1 = {};
  let frequencyCounter2 = {};

  // 각각의 배열을 순회하며 데이터 처리하기
  for (let val of arr1) {
    frequencyCounter1[val] = (frequencyCounter1[val] || 0) + 1;
  }
  for (let val of arr2) {
    frequencyCounter2[val] = (frequencyCounter2[val] || 0) + 1;
  }

  // 객체를 비교하여 조건에 부합되면 false 반환하기
  for (let key in frequencyCounter1) {
    if (!(key ** 2 in frequencyCounter2)) {
      return false;
    }
    if (frequencyCounter2[key ** 2] !== frequencyCounter1[key]) {
      return false;
    }
  }
  return true;
}
```

코드의 길이가 더 길어졌지만 시간 복잡도 측면에서 본다면 위의 코드는 `O(n)`의 시간 복잡도를 가진다. 즉 실행 속도는 훨씬 빨라졌다.

또한 각각의 배열을 서로 비교하는 것이 아니라 일단 객체를 만들어 필요한 데이터를 추가한다. 그 후 생성한 객체를 통해 값을 비교하는 모습이 보인다.

빈도수 세기 패턴은 객체를 활용하기 때문에 `for...of 문`이나 `in 연산`와 같은 객체를 잘 활용할 수 있는 여러 개념을 공부를 해야겠다.

### 2-2. 빈도수 세기 패턴 연습

> Given two strings, write a function to determine if the second string is an anagram of the first. An anagram is a word, phrase, or name formed by rearranging the letters of another, such as cinema, formed from iceman.

```javascript
// Examples:
validAnagram('', ''); // true
validAnagram('aaz', 'zza'); // false
validAnagram('anagram', 'nagaram'); // true
validAnagram('rat', 'car'); // false) // false
validAnagram('awesome', 'awesom'); // false
validAnagram('amanaplanacanalpanama', 'acanalmanplanpamana'); // false
validAnagram('qwerty', 'qeywrt'); // true
validAnagram('texttwisttime', 'timetwisttext'); // true
```

> Note: You may assume the string contains only lowercase alphabets.  
> Time Complexity - O(n)

내가 작성한 풀이는 아래와 같다.

```javascript
function validAnagram(string1, string2) {
  if (string1.length !== string2.length) return false;

  const string1Obj = {};
  const string2Obj = {};

  string1.split('').forEach((alphabet) => {
    string1Obj[alphabet] = string1Obj[alphabet] ? string1Obj[alphabet] + 1 : 1;
  });
  string2.split('').forEach((alphabet) => {
    string2Obj[alphabet] = string2Obj[alphabet] ? string2Obj[alphabet] + 1 : 1;
  });

  for (let key in string1Obj) {
    if (!(key in string2Obj)) return false;
    if (string1Obj[key] !== string2Obj[key]) return false;
  }

  return true;
}
```

위에서 살펴본 예시 문제와 풀이 과정이 거의 같다고 봐도 무방하다. 때문에 `O(n)`의 시간 복잡도를 가지고 있다.

아래는 강사님의 풀이이다. 비교하여 내가 공부할 점을 찾아보자.

```javascript
function validAnagram(first, second) {
  if (first.length !== second.length) {
    return false;
  }

  const lookup = {};

  for (let i = 0; i < first.length; i++) {
    let letter = first[i];
    lookup[letter] ? (lookup[letter] += 1) : (lookup[letter] = 1);
  }

  for (let i = 0; i < second.length; i++) {
    let letter = second[i];
    if (!lookup[letter]) {
      return false;
    } else {
      lookup[letter] -= 1;
    }
  }

  return true;
}
```

### 2-3. 배울 점

1. 첫 번째는 논리 연산자의 사용이다. 아래의 코드를 살펴보자.

   ```javascript
   frequencyCounter1[val] = (frequencyCounter1[val] || 0) + 1;

   string1Obj[alphabet] = string1Obj[alphabet] ? string1Obj[alphabet] + 1 : 1;
   ```

   모두 같은 역할을 하는 코드이다. 객체의 특정 값의 유무를 파악한 후 값이 있으면 해당 값에 1를 더하는 것이고 그렇지 않으면 1을 값으로 선언하는 코드이다. 하지만 어떤 코드가 더 가독성이 좋아 보이는가? 바로 위에 있는 강사님의 코드가 더욱 가독성이 좋아 보인다.

   ```javascript
   const string = null || 'string';
   const string2 = undefined || 'string';
   ```

   위의 두 변수 모두 `string`으로 할당되어 있다. 길고 복잡하게 쓰지 말고 짧지만 가독성까지 챙기는 코드를 작성하자.

2. `0`이 자바스크립트에서의 `Boolean` 타입일 때 `false`가 된다는 것이다.

   ```javascript
   if (!lookup[letter]) {
     return false;
   }
   ```

   해당 조건문은 `lookup` 객체에 `letter` 키의 값이 존재하지 않거나 `0`일 때 실행된다. 즉, 값이 존재하지 않을 때만 생각을 하는 것이 아니라 더 나가아 `0`일 때도 생각하면 더 유연한 코드가 완성된다.

3. 불필요한 객체는 만들지 말자. 내 코드와 강사님의 코드를 비교하면 누가봐도 강사님의 코드가 더욱 깔끔하다는 것을 알 수 있다. 그 이유는 무엇일까? 굳이 필요없는 객체를 만들었기 때문이다. 최소한의 데이터 정리를 생각해보자.

---

## 3. Multiple Pointers(다중 포인터 패턴)

다중 포인터는 배열이나 문자열 같은 선형 구조에서 각자 다른 원소를 가리키는 2개의 포인터를 조작해가면서 원하는 것을 얻어내는 개념이다. 즉, 2개의 포인터가 값이나 조건을 충족시키는 한 쌍을 찾아간다고 생각하면 된다.

### 3-1. 다중 포인터 패턴 예제

예제를 통해 다중 포인터 패턴을 알아보자. 아래와 같은 문제가 있다.

> Write a function called sumZero which accepts a sorted array of integers. The function should find the first pair where the sum is 0. Return an array that includes both values that sum to zero or undefined if a pair does not exist.

오름차순으로 정렬된 배열에서 서로의 합이 0이되는 짝을 찾는 문제이다.

```javascript
sumZero([-3, -2, -1, 0, 1, 2, 3]); // [3, -3]
sumZero([-2, 0, 1, 3]); // undefined
sumZero([1, 2, 3]); // undefined
```

먼저 다중 포인터 패턴을 적용하지 않고 이중 for문을 사용한 풀이이다.

```javascript
function sumZero(arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] + arr[j] === 0) {
        return [arr[i], arr[j]];
      }
    }
  }
}
```

이와 같은 풀이는 배열의 길이가 짧다면 큰 문제가 되지 않겠지만 조금만 길어져도 실행속도가 눈에 띄게 늘어나는 단점을 가지고 있다. 즉, `O(n^2)`의 시간 복잡도를 가지고 있다.

그 다음으로 다중 포인터 패턴을 적용한 풀이를 살펴보자.

```javascript
function sumZero(arr) {
  let left = 0;
  let right = arr.length - 1;
  while (left < right) {
    let sum = arr[left] + arr[right];
    if (sum === 0) return [arr[left], arr[right]];
    if (sum > 0) right -= 1;
    if (sum < 0) left += 1;
  }
}
```

`left`와 `right` 변수를 선언하여 각각 배열의 가장 왼쪽, 오른쪽을 가리키고 있다. 이를 이용하여 배열의 요소에 접근을 하고 두 요소의 합에 따라 `left` 또는 `right`의 값이 변하면서 합이 0이 되는 쌍을 찾고 있다. 이는 `O(n)` 시간 복잡도를 가진다.

주의해야 할 점은 만약 `while`의 조건이 `left <= right`이었다면 `[0, 0]`이라는 답이 나올 수 있다는 것이다. 그렇기 때문에 `left`와 `right`은 같은 요소를 가르킬 수 없도록 잘 설정을 해둬야 한다.

### 3-2. 다중 포인터 패턴 연습

> Implement a function called countUniqueValues, which accepts a sorted array, and counts the unique values in the array. There can be negative numbers in the array, but it will always be sorted.

```javascript
countUniqueValues([1, 1, 1, 1, 1, 2]); // 2
countUniqueValues([1, 2, 3, 4, 4, 4, 7, 7, 12, 12, 13]); // 7
countUniqueValues([]); // 0
countUniqueValues([-2, -1, -1, 0, 1]); // 4
```

> Time Complexity - O(n)  
> Space Complexity - O(n)  
> Bonus  
> You must do this with constant or O(1) space and O(n) time.

일단 가장 먼저 생각나는 것은 `Set` 객체를 활용한 풀이이다.

```javascript
function countUniqueValues(array) {
  return new Set(array).size;
}
```

매우... 간단하다! 하지만 다중 포인터 패턴을 연습하고 있으니 다중 포인터 패턴을 활용하여 다시 문제를 풀어보자.

```javascript
function countUniqueValues(array) {
  if (array.length === 0) return 0;

  let i = 0;
  array.forEach((item) => {
    if (array[i] === item) return;
    i += 1;
    array[i] = item;
  });

  return i + 1;
}
```

먼저 `array`의 길이가 0일 경우엔 `0`를 반환을 한다.

이후 `i`라는 포인터를 하나 가지고 `forEach()` 메서드를 사용하여 `array` 배열을 순회한다. 이때 또 다른 포인터에 대한 선언은 하지 않았지만 순회를 하면서 `array`의 요소를 하나하나 확인하기 때문에 포인터가 있다고 생각하면 좋을 것이다. 순회를 하면서 `array[i]`의 값과 현재 순회 중인 값이 다르다면 `i`의 값을 하나 올리고 이어서 `array[i]`의 값은 현재 순회 중인 값으로 바꾼다.

아래의 배열이 있다고 생각해보자.

- [1, 1, 2, 3, 3, 4, 5, 5]

첫 번째 순회부터 살펴보자.

| 순서 | `i`의 값 | 배열                     |
| ---- | -------- | ------------------------ |
| 1    | 0        | [1, 1, 2, 3, 3, 4, 5, 5] |
| 2    | 0        | [1, 1, 2, 3, 3, 4, 5, 5] |
| 3    | 1        | [1, 2, 2, 3, 3, 4, 5, 5] |
| 4    | 2        | [1, 2, 3, 3, 3, 4, 5, 5] |
| 5    | 2        | [1, 2, 3, 3, 3, 4, 5, 5] |
| 6    | 3        | [1, 2, 3, 4, 3, 4, 5, 5] |
| 7    | 4        | [1, 2, 3, 4, 5, 4, 5, 5] |
| 8    | 4        | [1, 2, 3, 4, 5, 4, 5, 5] |

위와 같은 과정을 거쳐 `i`의 값과 배열이 바뀐다. 최종적으로 구하고자 하는 값은 고유한 숫자의 개수 이므로 `i`에 1을 더한 값을 반환하면 된다.

이번엔 강사님의 코드를 살펴보자.

```javascript
function countUniqueValues(arr) {
  if (arr.length === 0) return 0;
  var i = 0;
  for (var j = 1; j < arr.length; j++) {
    if (arr[i] !== arr[j]) {
      i++;
      arr[i] = arr[j];
    }
  }
  return i + 1;
}
```

전체적인 흐름은 같다. 하지만 디테일한 부분에 차이가 있다. 이를 위주로 살피고 넘어가자.

1. `var` 변수 사용 - 난 하지 않는다. 변수는 `let`, 상수는 `const`를 사용하여 변수를 선언한다.
2. `for문`을 통한 반복문 - 난 `for문`도 사용하지만 보통 배열 메서드를 자주 사용한다.

---

## 4. Sliding Window(기준점 간 이동 배열 패턴)

This pattern involves creating a window which can either be an array of number form one position tto another. Depending on a certain condition, the window either increases or closes(and a new window is created)

쉽게 생각하자면 문자열이나 배열에서 연속된 요소들을 비교하고자 할 때 유용한 패턴이다. 가장 간단한 예시를 생각해보자. 길이가 100인 배열이 있다. 첫 번째 요소부터 순서대로 n개의 요소를 비교할텐데(더하든 빼든 다른 특정한 작업을 하든 상관없다.) 비교하는 순서는 아래와 같다.

- 0번 ~ 4번
- 1번 ~ 5번 -> 0번의 창문을 닫고 5번의 창문을 연다.
- 2번 ~ 6번 -> 1번의 창문을 닫고 6번의 창문을 연다.
- ...

위와 같이 데이터를 비교하게 되는 것이 기준점 간 이동 배열 패턴의 기본이라고 할 수 있다.

규모가 큰 데이터셋에서 데이터의 하위 집합을 추적하는 문제에 있어 유용한 패턴이다.

### 3-1. 기준점 간 이동 배열 패턴 예제

예제를 통해 기준점 간 이동 배열 패턴을 알아보자. 아래와 같은 문제가 있다.

> Write a function called maxSubarraySum which accepts an array of integers and a number called n. The function sholud calculate the maximun sum of n consecutive(연속적인) elements in the array.

```javascript
maxSubarraySum([1, 2, 5, 2, 8, 1, 5], 2); // 10
maxSubarraySum([1, 2, 5, 2, 8, 1, 5], 4); // 17
maxSubarraySum([4, 2, 1, 6], 1); // 6
maxSubarraySum([4, 2, 1, 6, 2], 4); // 13
maxSubarraySum([], 4); // null
```

먼저 이중 for문을 이용한 풀이이다. 당연히 시간 복잡도는 `O(n^2)`이므로 효율적인 풀이는 아니다.

```javascript
function maxSubarraySum(arr, num) {
  // 배열의 길이가 num보다 작으면 null를 반환한다.
  if (num > arr.length) {
    return null;
  }
  // 더한 값이 음수가 될 수 있으므로 -Infinity로 시작한다.
  var max = -Infinity;
  for (let i = 0; i < arr.length - num + 1; i++) {
    temp = 0;
    // i번째 요소부터 num개의 요소를 더한다.
    for (let j = 0; j < num; j++) {
      temp += arr[i + j];
    }
    // 더한 값이 기존 max보다 크면 max에 할당한다.
    if (temp > max) {
      max = temp;
    }
  }
  return max;
}
```

다음으론 기준점 간 이동 배열 패턴을 적용한 풀이이다.

```javascript
function maxSubarraySum(arr, num) {
  let maxSum = 0;
  let tempSum = 0;
  if (arr.length < num) return null;
  for (let i = 0; i < num; i++) {
    maxSum += arr[i];
  }
  tempSum = maxSum;
  for (let i = num; i < arr.length; i++) {
    tempSum = tempSum - arr[i - num] + arr[i];
    maxSum = Math.max(maxSum, tempSum);
  }
  return maxSum;
}
```

`maxSum`과 `tempSum`를 미리 선언을 한 뒤 `maxSum`은 처음 값으로 0번째 부터 n번째의 요소의 합을 할당한다. 이후 반복문이 등장한다. 여기가 바로 기준점 간 이동 배열 패턴을 적용한 핵심 포인트가 된다. 아래의 코드를 보자.

```javascript
tempSum = tempSum - arr[i - num] + arr[i];
```

`- arr[i - num]`으로 기존에 열려있던 window를 하나 닫고 `+ arr[i]`으로 새로운 window를 하나 연다. 이렇게 차곡차곡 하나씩 열고 닫으면서 `tempSum`값이 변하게 되고 이는 바로 아래에서 `maxSum`과 비교하여 `maxSum`를 업데이트 할지 말지를 정한다. 바로 이것이 기준점 간 이동 배열 패턴의 기본 개념이라고 할 수 있다.

---

## 참고

Udemy - JavaScript 알고리즘 & 자료구조 마스터클래스 / 섹션 5: 문제 해결 패턴

---

📅 2023-01-12
