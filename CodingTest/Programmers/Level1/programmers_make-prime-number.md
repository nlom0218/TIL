# 소수 만들기

## 1. 개요

- 프로그래머스
- Lv.1
- Summer/Winter Coding(~2018)
- [문제 바로가기](https://school.programmers.co.kr/learn/courses/30/lessons/12977)

---

## 2. 문제 설명

주어진 숫자 중 3개의 수를 더했을 때 소수가 되는 경우의 개수를 구하려고 합니다. 숫자들이 들어있는 배열 nums가 매개변수로 주어질 때, nums에 있는 숫자들 중 서로 다른 3개를 골라 더했을 때 소수가 되는 경우의 개수를 return 하도록 solution 함수를 완성해주세요.

---

### 2-1. 문제 설명 - 제한사항

- nums에 들어있는 숫자의 개수는 3개 이상 50개 이하입니다.
- nums의 각 원소는 1 이상 1,000 이하의 자연수이며, 중복된 숫자가 들어있지 않습니다.

---

### 2-2. 문제 설명 - 입출력 예

| nums        | result |
| ----------- | ------ |
| [1,2,3,4]   | 1      |
| [1,2,7,6,4] | 4      |

---

### 2-3. 문제설명 - 입출력 예 설명

입출력 예 #1  
[1,2,4]를 이용해서 7을 만들 수 있습니다.

입출력 예 #2  
[1,2,4]를 이용해서 7을 만들 수 있습니다.  
[1,4,6]을 이용해서 11을 만들 수 있습니다.  
[2,4,7]을 이용해서 13을 만들 수 있습니다.  
[4,6,7]을 이용해서 17을 만들 수 있습니다.

---

## 3. 문제 풀이

```js
// 1) 서로 다른 n개의 원소를 가지는 어떤 배열에서 r개의 원소를 선택하기(조합)
function getCombinations(array, size) {
  function p(t, i) {
    if (t.length === size) {
      result.push(t);
      return;
    }
    if (i + 1 > array.length) {
      return;
    }
    p(t.concat(array[i]), i + 1);
    p(t, i + 1);
  }

  var result = [];
  p([], 0);
  return result;
}

// 2) 소수인지 판별하기
function isPrime(num) {
  for (i = 2; i * i <= num; i++) {
    if (num % i === 0) return false;
  }
  return true;
}

function solution(nums) {
  // 3) 조합으로 만들어진 각각의 배열의 원소들의 합 구하기
  const sumNums = getCombinations(nums, 3).map(
    (num) => num[0] + num[1] + num[2]
  );

  // 4) 구한 원소들의 합이 소수인지 판별하기
  const isPrimeNums = sumNums.map((num) => isPrime(num)).filter((item) => item);

  // 5) 소수의 개수 구하기
  return isPrimeNums.length;
}
```

---

### 1) 서로 다른 n개의 원소를 가지는 어떤 배열에서 r개의 원소를 선택하기(조합)

이번 코딩테스트에서 가장 기본이 되고 중심이 되는 알고리즘이지 않을까 생각한다.
3개 이상의 숫자로 이루어진 `nums`배열에서 중복되지 않는 숫자 3개를 선택을 해야하며 모든 경우의 수를 생각을 해야했다. 처음에는 혼자서 알고리즘을 작성을 했지만 쉽게 해결되지 않았다. 그래서 이후에 스터디원들의 도움을 받아 해당 개념이 `Combinations`이라는 것을 알게 되었고 스택오버플로우에 좋은 코드가 있어 참고하였다.  
[Itertools.combinations in Javascript](https://stackoverflow.com/questions/45813439/itertools-combinations-in-javascript)

아래의 코드를 분석하여 어떤 과정을 거쳐 조합을 만드는지 살펴보자.

```js
function getCombinations(array, size) {
  function p(t, i) {
    if (t.length === size) {
      result.push(t);
      return;
    }
    if (i + 1 > array.length) {
      return;
    }
    p(t.concat(array[i]), i + 1);
    p(t, i + 1);
  }

  var result = [];
  p([], 0);
  return result;
}
```

`getCombinations()`함수는 두 가지 파라미터를 받는데 하나는 combination을 하게 될 `array`과 몇개의 원소를 선택할지 정하는 `size`이다.

내부에는 차례대로 `p()`함수가 정의되어 있고 `result`변수가 선언, `p()`함수가 호출 그리고 마지막으로 `result`를 반환하고 있다.

`p()`함수가 실행되는 과정을 알아보자.

```js
p([], 0);
```

위와 같이 함수가 호출이 되었고 빈 배열과 0을 파라미터로 전달하고 있다. 이제 `p()`함수가 실행되는 과정을 살펴보자.

```js
if (t.length === size) {
  result.push(t);
  return;
}
```

가장 먼저 보이는 `if문`이다. 배열`t`의 원소의 개수가 `size`(하나의 조합에 들어갈 원소의 개수)와 같다면 배열`t`를 `result`에 `Array.push()`메서드를 통해 추가한다. 그 후 `p()`함수를 종료한다.

아래는 다음으로 보이는 `if문`이다.

```js
if (i + 1 > array.length) {
  return;
}
```

위의 `if문`에서의 변수 `i`는 `p()`함수의 두 번째 파라미터이고 `array`는 combination을 하게 될 배열이다. 단순히 `i + 1`이 `array.length`보다 커지게 되면 `p()`함수를 종료한다는 조건문이다.

그 다음 코드를 살펴보자.

```js
p(t.concat(array[i]), i + 1);
```

`p()`함수 내부에서 또 다시 `p()`함수를 호출하고 있다. 다만 파라미터는 변했다. 기존에 받은 배열과 combination을 하게 될 배열의 i번째 원소를 합한 배열(`Array.concat(array)`)을 첫 번재 파라미터, 그리고 i + 1를 두 번째 파라미터로 전달하고 있다.

마지막 코드도 `p()`함수를 내부에서 다시 호출하고 있는데 이때는 기존의 배열을 첫 번재 파라미터, 그리고 i + 1를 두 번째 파라미터로 전달하고 있다. (아래 코드 참고)

```js
p(t, i + 1);
```

`getCombinations()`함수의 예시를 그림으로 나타내봤다.

![make-prime-number1](/image/CodingTest/programmers_make-prime-number/make-preime-number1.jpg)

---

### 2) 소수인지 판별하기

소수는 나눌 수 있는 수가 1과 자신인 수이다. 예를들어 4인 경우 1, 2, 4로 나눌 수 있기 때문에 소수가 아니다. 13은 1, 13으로만 나눌 수 있으므로 소수이다. 즉, 소인수분해를 했을 때 1과 자기자신인 수가 소수이다.

```js
function isPrime(num) {
  for (i = 2; i * i <= num; i++) {
    if (num % i === 0) return false;
  }
  return true;
}
```

위는 특정 수를 파라티터로 받고 해당 수가 소수인지 아닌지 판별하는 `isPrime()`함수를 나타낸 코드이다.

---

### 3) 조합으로 만들어진 각각의 배열의 원소들의 합 구하기

`getCombinations()`함수 덕분에 3개의 이상의 숫자로를 원소로 가지고 있는 배열을 조합의 형태로 만들었다. 그 후 각각의 배열을 돌면서(`Array.map()`) 3개의 숫자를 더하는 코드이다.

```js
const sumNums = getCombinations(nums, 3).map((num) => num[0] + num[1] + num[2]);
```

---

### 4) 구한 원소들의 합이 소수인지 판별하기

위의 과정에서 `sumNums`배열에는 각각의 조합의 숫자들을 더한 값이 저장되어 있다. 그 다음으로 해야 할 일들은 숫자들이 소수인지 아닌지 판별하는 과정이다. 그리고 판별이 끝나면 소수는 `true`, 소수가 아닌 수는 `false`로 리턴된다. 내가 필요한 것은 소수이므로 마지막으로 `Array.filter()`메서드를 사용하여 `true`값만 가져왔다.

```js
const isPrimeNums = sumNums.map((num) => isPrime(num)).filter((item) => item);
```

---

### 5) 소수의 개수 구하기

이제 `isPrimeNums`배열에는 소수의 개수 만큼 `ture`가 원소로 있고 `isPrimeNums`배열의 길이를 리턴하면 문제는 마무리된다!

```js
return isPrimeNums.length;
```

---

### 6) 결과

![make-prime-number 2](/image/CodingTest/programmers_make-prime-number/make-prime-unmber2.png)

---

## 4. Refactoring

나는 `isPrimeNums`배열의 원소를 `boolean`값으로 가지고 있다. 하지만 변수의 이름과 통일성을 가지기 위해 `boolean`값이 아닌 `number`값으로 가지려 한다. 또한 변수명을 `primeNums`로 바꾸려 한다.

이를 위해 수정해야 할 부분은 `2) 소수인지 판별하기`에서의 `isPrime()`함수이다. 소수가 아닌 수는 `false`로 리턴을 하지만 소수인 값은 받은 `num`을 그대로 리턴을 하도록 바꾸었다. (아래 코드 참고)

```js
// 2) 소수인지 판별하기
function isPrime(num) {
  for (i = 2; i * i <= num; i++) {
    if (num % i === 0) return false;
  }
  return num;
}
```

```js
// 4) 구한 원소들의 합이 소수인지 판별하기
const primeNums = sumNums.map((num) => isPrime(num)).filter((item) => item);

return primeNums.length;
```

---

## 5. 다른 사람의 풀이

### Seulki Lee님의 풀이

```js
function primecheck(n) {
  for (var i = 2; i <= Math.sqrt(n); i++) {
    if (n % i == 0) {
      return false;
    }
  }
  return true;
}
function solution(nums) {
  var cnt = 0;
  for (var i = 0; i < nums.length - 2; i++) {
    for (var j = i + 1; j < nums.length - 1; j++) {
      for (var w = j + 1; w < nums.length; w++) {
        //console.log(nums[i]+"/"+nums[j]+"/"+nums[w]);

        if (primecheck(nums[i] + nums[j] + nums[w])) {
          //console.log(nums[i]+nums[j]+nums[w]);
          cnt++;
        }
      }
    }
  }
  return cnt;
}
```

내가 처음으로 시도했던 방법이다. 하지만 나는 해당 방법으로 끝까지 답을 구하지 못하였다. 어떤 문제가 있었는지 생각해보자.

다른 내용은 같은데 각각의 `for문`에서 변수가 언제까지 반복문을 실행하지를 정하는 조건에서 나에게는 실수가 있었던 거 같다. 처음에 나도 첫 번째로 뽑힌 숫자는 배열의 길이의 - 2 만큼만 반복문을 돌려야 겠다고 생각을 했지만 두 번째, 세 번째 숫자에 대한 논리적인 사고가 부족했다. 그림으로 표현하면 아래와 같다.

![make-preime-number3](/image/CodingTest/programmers_make-prime-number/make-preime-number3.jpg)

---

## 6. Conclusion

> 처음 시도했던 방법을 다시 한 번더 체크할 수 있어서 좋은 공부였다. 머리속으로 생각하는 것보다 실제로 그림을 그리면서 조건을 파악하는 것을 적극 사용해보자.  
> 시간 내에 풀지 못했서 조금은 우울한 기분이 들었지만 이후 공부하는 과정에서 `Combinations`에 대한 개념과 함수에 대해 분석할 수 있었고 내가 테스트에 실패한 코드를 다시 확인할 수 있어 좋은 기회였다고 생각한다.

---

[👆](#소수-만들기)
