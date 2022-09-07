# 해시 테이블(Hash Table)

## 1. 개요

학교의 사물함을 보면 이름표가 붙어있다. 이런 이름표는 사물함의 주인을 뜻하는 키가 된다. 사물함의 주인의 이름만 알고있다면
사물함을 처음부터 열어보지 않더라도 원하는 사물함을 쉽게 찾을 수 가 있다. 이러한 원리가 프로그래밍에서도 적용이 된다.
그것이 바로 해시 테이블이다. 이번 해시 테이블에 대해 살펴본다.

---

## 2. 해시 테이블이란?

### 2-1. 먼저 알아둬야 할 용어

해시 테이블에 대해 살펴보기 전 미리 알아둬야 할 용어부터 정리한다.

- 해시: 단방향 암호화 기법으로 해시함수를 이용하여 고정된 길이의 암호화된 문자열로 바꿔버리는 것을 의미
- 해시함수: 임의의 길이의 데이터를 고정된 길이의 데이터로 매핑하는 함수
- 키: 매핑 전 원래 데이터의 값
- 해시값: 매핑 후 데이터의 값
- 해싱: 매핑하는 과정
- 해시 알고리즘: 특정 입력에 대해 항상 같은 해시 값을 리턴하는 알고리즘, SHA-256, SHA-512 등을 사용하기를 권고
- 버킷(bucket)
  - 각 테이블에 해당하는 인덱스
  - 테이블의 크기
  - 키를 입력받는 해시 함수의 결과 범위
- 슬롯(slot)
  - 한 개의 버킷에 저장될 키 값의 개수

---

### 2-2. 해시 테이블

**키와 값을 받아 키를 해싱하여 인덱스를 배정하고, 인덱스에 값을 넣는 자료 구조**

즉, 해시 테이블은 `key`와 `value`이 한 쌍이 되어 데이터를 저장하는 자료구조이다.

해시 테이블의 특징은 아래와 같다.

- 순차적으로 데이터를 저장하지 않음
- `key`를 통해 `value`를 얻을 수 있음
- 커다란 데이터를 해시해서 짧은 길이로 축약할 수 있기 때문에 데이터를 비교할 때 효율적
- `value`는 중복 가능하지만 `key`는 유니크해야 함
- 수정 가능

![Hash Table](https://blog.kakaocdn.net/dn/b1zOw1/btqL6HAW7jy/jpBA5pPkQFnfiZcPLakg00/img.png)

---

### 2-3. 해시 테이블의 충돌

![Hash Table collision](https://velog.velcdn.com/images%2Fjangwonyoon%2Fpost%2F0f7b6f52-5149-4b77-b9d2-0e0c11ec5439%2F123.png)

위의 그림에서 `John Smith`와 `Sandra Dee`는 모두 해시 함수를 통해 값은 키를 가지고 있다. 해시 테이블의 특징인
`key`는 유니크 해야하는 점에 위반이 된다. 이러한 경우가 해시 테이블에서는 종종 발견된다. 위의 그림에서는 해시 테이블의 충돌을
`분리 연결법`을 사용하여 해결하고 있다.

그렇다면 해시 테이블의 충돌을 해결할 수 있는 방벙은 또 무엇이 더 있을까? 간단히 살펴보도록 한다.

1. 개방 주소법(open address)

   - 선형 탐사법(Linear probing): 이미 해시값이 존재한다면 해당 해시값에서 고정 폭을 옮겨 다음 해시값에 해당하는 버킷에 데이터를 할당한다.  
     ![선형 탑사법](https://miro.medium.com/max/614/1*xN0omiiMDelgCQmmg7zv9Q.png)
   - 제곱 탐사법(Quadratic Probing): 선형 탐사법과 동일하지만 고정폭이 아니라 제곱으로 옮긴 해시값에 해당하는 버킷에 데이터를 할당한다.  
     ![제곱 탐사법](https://upload.wikimedia.org/wikipedia/commons/e/ee/Quadratic_probing_png.png)
   - 이중 해싱(Double Hashing): 해시 함수를 이중으로 사용하는 벙법으로, 하나는 최초의 해시값을 얻을 때, 다른 하나는 해시 충돌이 일어났을 때 탐사 이동폭을 얻기 위해 사용한다.

     ***

2. 분리 연결법(seperate chaining)

   - 분리 연결법은 개방 주소법과는 달리 한 버킷당 들어갈 수 있는 슬롯의 수에 제한을 두지 않는다.
   - 슬롯에는 연결 리스트(linked list)나 트리(tree)를 사용한다.

---

## 3. Conclusion

> 내가 이해를 잘 못하는 건지 블로그의 내용들이 중구난방인 건지... 개념에 대한 이해가 많이 부족하다. 그래서 좀더 시간을 두고
> 다음에 다시 정리하고자 한다. 특히 슬롯과 버켓의 개념에 어떤 블로그에선 같은 개념으로 보고 어떤 블로그에선 다른 개념으로 본다.
> 일단 자바스크립트에선 해시 테이블과 비슷한 객체와 Map이 있다고 생각하고 해시 테이블이 필요한 즉, 여러가지의 값을
> 하나의 테이터로 만들어 문제를 풀어야 하는 문제가 있다면 Map를 적극적으로 사용하여 문제를 풀어야겠다.
> 아직 갈 길이 멀다.😭

---

## 참고

[[자료구조] 해시테이블 with JavaScript](https://overcome-the-limits.tistory.com/9)  
[자료구조 3. Hash Table](https://study-ihl.tistory.com/71)  
[자료구조 | 해시 테이블 hash table](https://velog.io/@edie_ko/hashtable-with-js#%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-object%EB%8A%94-%ED%95%B4%EC%8B%9C-%ED%85%8C%EC%9D%B4%EB%B8%94%EC%9D%BC%EA%B9%8C)