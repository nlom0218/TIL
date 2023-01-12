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

> Examples:  
> validAnagram('', '') // true  
> validAnagram('aaz', 'zza') // false  
> validAnagram('anagram', 'nagaram') // true  
> validAnagram("rat","car") // false) // false  
> validAnagram('awesome', 'awesom') // false  
> validAnagram('amanaplanacanalpanama', 'acanalmanplanpamana') // false  
> validAnagram('qwerty', 'qeywrt') // true  
> validAnagram('texttwisttime', 'timetwisttext') // true

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

위에서 살펴본 예시 문제와 풀이 과정이 거의 같다고 봐도 무방하다. 때문에 `O(n^2)`의 시간 복잡도를 가지고 있다.

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

## 참고

Udemy - JavaScript 알고리즘 & 자료구조 마스터클래스 / 섹션 5: 문제 해결 패턴

---

📅 2023-01-12
