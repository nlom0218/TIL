# 빅오 표기법

## 1. 개요

알고리즘의 효율성에 따라 코드의 처리 속도가 달라진다. 이때 알고리즘 간에 효율성을 비교하기 위해
빅오 표기법(big-O notation)을 가장 많이 사용한다.

어린 빅오 표기법이 무엇인지 살펴보자.

---

## 2. 빅오 표기법란?

Big-O notation은 알고리즘의 시간 복잡도를 나타내는 표기법으로, O(f(n))으로 나타낸다.

시간 복잡도란? 어떤 알고리즘을 수행하는 데 걸리는 시간을 설명하는 계산 복잡도를 의미한다. 빅오로 시간 복잡도를
표현할때는 몇 가지 법칙에 대해 살펴보자.

---

### 2-1. 계수 법칙

입력 크기와 연관되지 않는 상수를 전부 무시한다.

> 상수 k>0인 경우 f(n)이 O(g(n))이면 kf(n)은 O(g(n))이다.

---

### 2-2. 합의 법칙

시간 복잡도를 더할 수 있다. 결괏값인 시간 복잡도가 두 개의 다른 시간 복잡도의 합이라면 결괏값인 빅오 표기법 역시
두 개의 다른 빅오 표기법의 합이다.

> f(n)이 O(h(n))이고 g(n)이 O(p(n))이라면 f(n)+g(n)은 O(h(n)+p(n))이다.

합의 법칙 이후 계수 법칙을 적용해야 한다.

---

### 2-3. 곱의 법칙

두 개의 다른 시간 복잡도를 곱할 때 빅오 표기법 역시 곱해진다.

> f(n)이 O(h(n))이고 g(n)이 O(p(n))이라면 f(n)g(n)은 O(h(n)p(n))이다.

곱의 법칙 이후 계수 법칙을 적용해야 한다.

---

### 2-4. 다항 법칙

> f(n)이 k차 다항식이면 f(n)은 O(n^k)이다.

---

## 3. 빅오 표기법의 종류

![Big-O](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FumDDr%2FbtqYhz5p1ZN%2FpULPOIs1zk2kA5QykgYEeK%2Fimg.png)

- O(1): 입력값에 상관없이 일정한 실행시간을 가진다. 최고의 알고리즘이지만 상수값이 상상 이상으로 클 경우 사실상
  일정한 시간의 의미가 없다.(상수 복잡도)
- O(log n): 로그는 매우 큰 입력값에서도 크게 영향을 받지 않는 편이다. 매우 견고한 알고리즘으로 이진 탐색의 경우가 이에 해당한다.(로그 복잡도))
- O(n): 알고리즘을 수행하는데 걸리는 시간은 입력값에 비례한다. 이러한 알고리즘을 선형 시간 알고리즘이라 불리며 모든 입력값을 적어도 한 번
  이상은 살펴봐야 한다.(선형 복잡도)
- O(n log n): 대부분 효율이 좋은 알고리즘이 이에 해당 한다.
- O(n^2): 버블 정렬 같은 비효율적인 정렬 알고리즘이 이에 해당 한다.
- O(2^n): 피보나치의 수를 재귀로 계산하는 알고르짐이 이에 해당 한다.
- O(n!): 가장 느린 알고리즘으로 입력값이 조금만 커져도 계산이 어렵다.

---

## 4. 예제

### 4-1. O(1)

```javascript
for (let i = 0; i < 1000; i++) {
  console.log(i);
}
```

반복문이 0부터 1000까지 실행된다.

> Array.push(), Array.pop()

---

### 4-2. O(log n)

```javascript
for (let i = 0; i < n; i * 2) {
  console.log(i);
}
```

주어진 n에 대해 log2n번만 실행된다. i가 증가할 i에 1을 더하는 것이 아니라 2를 곱하기 때문이다.

---

### 4-3. O(n)

```javascript
for (let i = 0; i < 10n; i++) {
  console.log(i);
}

for (let i = 0; i < 3n; i++) {
  console.log(i);
}
```

첫 번째 반복문은 0부터 10n까지 실행되므로 O(10n), 두 번째 반복은 0부터 3n까지 샐행되므로 O(3n)이다.
합의 법칙으로 O(13n), 계수 법칙으로 O(n)이 된다.

> Array.shift(), Array.unshift(), Array.concat(), Array.slice(), Array.splice()
> Array.forEach(), Array.map(), Array.filter(), Array.reduce()

---

### 4-4. O(n^2)

```javascript
for(let i = 0; i < 3n; i++) {
    for(let j = 0; < 2n; j++){
        console.log(i, j)
    }
}
```

두 개의 중첩 루프가 있고 곱의 법칙으로 O(6n^2), 계수 법칙으로 O(n^2)이 된다.

```javascript
for (let i = 0; i < n; i++) {
  for (let j = 0; j < n; j++) {
    for (let k = 0; k < n; k++) {
      for (let l = 0; l < 10; l++) {
        console.log(i, j, k, l);
      }
    }
  }
}
```

네 개의 중첩 루프가 있지만, 마지막 루프는 10까지 밖에 실행되지 않아 O(n^3)의 시간 복잡도를 가지게 된다.

---

## 5. Conclusion

> 1년 전 잠깐 c#를 공부할 때 들어봤던 빅오 표기법이었다. 그땐 단순하게 "아~ 이런것이 있구나"하고 넘어갔다. 사실 이해하기도 힘들어서
> 처리속도를 나타내는 지표정도로만 생각하고 있었다. 하지만 이번에 코딩 테스트 문제를 풀다가 시간 복잡도에 따라 결과가 달라지는 것을 보고
> 이것이 얼마나 중요한지를 깨닫게 되었다. `Array.shift()`, `Array.unshift()` 메서드가 O(n)의 시간복잡도를 가지고 있고
> O(n)이 어떤 특징이 있었는지 알았다면 실행 속도를 더 빨리 개선하지 않았을까 싶다. 해당 문제는 아래에 링크로 남긴다.  
> 이번 기회에 빅오 표기법, 시간 복잡도에 대해 정리하면서 공부도 했지만 아직 까진 해당 개념들이 뜬 구름처럼 잘 잡히지 않는다. 많은 코딩 테스트
> 문제를 풀어보면서 해당 알고리즘은 어떤 시간 복잡도를 가지고 있을지 생각을 많이 해봐야겠다.

[두 큐 합 같게 만들기](CodingTest/Programmers/Level2/programmers_make_the_sum_of_two_queues_equal.md)

---

## 참고

[[자료구조] 빅오 표기법(Big-O notation)이란?](https://holika.tistory.com/29)  
[빅오 표기법 (O, big-O)](https://codermun-log.tistory.com/235)  
[[Javacript] 빅오 표기법 정리 및 예제](https://itprogramming119.tistory.com/entry/Javascript-%EB%B9%85%EC%98%A4-%ED%91%9C%EA%B8%B0%EB%B2%95-%EC%A0%95%EB%A6%AC-%EB%B0%8F-%EC%97%B0%EC%8A%B5-%EB%AC%B8%EC%A0%9C)  
[[JS] 자바스크립트 배열 시간 복잡도](https://parkparkpark.tistory.com/m/101)
