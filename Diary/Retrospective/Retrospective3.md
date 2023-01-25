# #3 23년 1월 16일 ~ 23년 1월 22일

## 1. 주간 요약

![#3 주간 요약](/image/Diary/Retrospective/retrospective3.png)

---

## 2. 개발 공부

### 2-1. 코딩 테스트

드디어 프로그래머스 Level 1 문제를 모두 풀었다. 가장 정답률이 낮았던 카카오 기출의 `개인정보 수집 유효기간
` 문제는 그다지 어렵지 않아 정리를 하지 않았다. 아마 작년 실제 시험에서 한 번 풀어봤던 문제여서 그런 듯 하다.
Level 2로 넘어가서 푼 문제는 `n^2 배열 자르기` 문제였다. 해당 문제은 엄청 어렵진 않았지만 시간 복잡도를 생각을 하며 풀어야 했기 때문에 까다로운 점이 있었다.

개인적인 느낌으로 Level 1은 단순 구현 문제가 많이 있다면 Level 2 부터는 자료구조, 알고리즘, 시간 복잡도를 조금씩 고려를 해야만 해결할 수 있는 문제가 주로 있는 듯 하다.

아직 자료구조, 알고리즘, 시간 복잡도에 어려움을 가지고 있다. 특히 어떤 자료구조, 알고리즘을 적용을 하여 풀어야 하는지 파악하는 힘이 부족하여 보다 많은 공부가 필요하다고 생각한다. 현재 듣고 있는 유데미 자료구조&알고리즘 강의를 꾸준히 들으며 많은 문제를 풀어보도록 하자.

아래는 내가 이번주에 푼 문제와 이를 정리한 TIL이다.

- [햄버거 만들기](CodingTest/Programmers/Level1/programmers_makeing_hamburger.md)
- [n^2 배열 자르기](CodingTest/Programmers/Level2/programmers_crop_array.md)

### 2-2. 알고리즘 강의

이번주 알고리즘 강의의 주제는 `재귀`였다. 코딩 테스트에 관심을 가지고 여러 문제를 풀기 전 까지 재귀라는 것을 구현해 본 적이 없었다. 때문에 처음 재귀를 사용하여 문제를 풀었을 땐 많은 어려움이 있었다. 어떻게 이런 풀이가 가능한건지, 어떻게 이렇게 생각을 할 수 있는지 감탄을 하며 다른 사람들의 풀이를 봤던 기억이 난다.

하지만 여러 문제를 통해 직접 재귀를 구현해보고 이번 알고리즘 강의를 통해 재귀에 대해 다시 공부하니 옛날 보다는 그래도 조금은 편안한 느낌이 들었다.

재귀를 공부하면 좋았던 점은 재귀 함수가 가져야 할 조건을 정리를 할 수 있었던 것이다. 조건은 바로 아래와 같다.

1. 종료 조건: 인자(num)가 1보다 작을 때
2. 다른 입력값: 함수가 호출 될 때 마다 인자(num)가 1씩 감소한다.

재귀의 조건을 잘 기억을 하면서 재귀가 필요한 상황에 잘 사용하도록 하자.

또한 여러 재귀 문제를 풀어봤는데, 간단한 문제로만 되어있어 재귀가 아니라 단순 반복문을 통해 구할 수 있었다. 하지만 재귀를 연습하고자 일부러 재귀만 사용하였다. 추가적으로 `순수 재귀 방법`, `helper method 재귀 방법`을 모두 사용하고자 고민을 하였다.

강의에서 제공하는 문제는 쉬운 문제였다고 생각한다. 그래서 쉽게 풀었던 것이었을 뿐 내 실력이 뛰어나다고 생각하지 않는다. 복잡한 재귀가 등장하면 과연 어떻게 대응할까 궁금하다.

아래는 강의를 들으며 정리한 재귀에 대한 내용을 정리한 TIL이다.

- [재귀(Recursion)](DataStructureAlgorithm/Recursion.md)

### 2-3. 노마드 코더 - 바닐라 JS로 크롬 웹 만들기

옛날의 들었던 강의이기 때문에 강의 내용은 어렵지 않게 마무리를 하였다. 이제 앞으로 해야할 일은 내 스타일이 잔뜩 들어간 모멘토 앱을 만드는 것이다. 그 중 가낭 큰 영역을 차지하는 것은 `투두 리스트`이다. 내가 사용하고 있는 TickTick 어플을 레퍼런스 삼아 디자인을 하고 있고 추가, 삭제, 수정, 완료 등 기능을 만들고 있다.

해당 과정은 스터디에서 공유하고 있기 때문에 함께 사용하고 있는 레포에 코드를 올려야 한다. 해당 과정에서 일주일의 커밋이 모두 사라질 뻔 했지만 다행히 복구를 할 수 있어 위험한 상황은 피했다. 나의 잔디도 소중하기 때문에 다시는 위험한 상황을 만들지 말자ㅠㅠ 아직도 생각하면 심장이 벌렁벌렁...

23일(월)부터는 해당 과정 챌린지가 진행된다. 나는 3년 전에 완료했기 때문에 새로운 계정을 만들고 새롭게 신청을 했다. 챌린지는 2주간 진행되기 때문에 나만의 모멘토 앱도 해당 기간 동안 완성을 해야 한다. 부지런하게 완성해보자.

![모멘토 앱 - 만들고 있는 중..](/image/Diary/Retrospective/nomadMoment.png)

### 2-4. 자바스크립트 공부

1. 이터러블
   이터러블 개념은 진작에 알고 있었다. 하지만 순회 가능하다라는 것만 알고 있었고 정확히 어떤 개념인지는 몰랐다. 이번 기회에 정리를 하였고 이터레이터라는 개념도 학습하였다. 이터레이터는 평소에 자주 사용할지는 의문이지만 일단 정리를 했으니 이후에 필요할 때 떠올리도록 하자. 이터러블 객체에 여러 문법을 사용할 수 있는데 그 중 `for..of 문`을 정리하였다. `스프레드 문법`, `배열 디스트럭처링 할당`은 각각 파트로 나누어 정리할 예정이다.
2. 객체 관련 구문
   배열은 자주 사용하고 메서드도 다양하게 있어 익숙하다. 하지만 일반 객체의 순회에 대해서는 부족한 점이 있어 이를 정리하였다. 코딩 테스트 문제를 풀 때 유용하게 사용할 수 있을 듯 하다.
3. 정규 표현식
   정규 표현식은 엄밀히 따지자면 자바스크립트 공부라고는 할 수 없다. 여러 언어에서도 사용하기 때문이다. 하지만 나의 주 언어가 자바스크립트이고 정규식 관련 자바스크립트 메서드도 정리할 예정이기 때문에 자바스크립트 공부 파트에 넣었다. 정규 표현식은 설날 연휴에 틈틈히 시간을 내서 정리하고자 하였다. 하지만 생각보다 시간이 여유롭지 않아 25일(수)까지 정리하기로 다시 계획을 세웠다. 당장은 익숙하지 않고 완벽하게 사용할 순 없지만 꼼꼼히 정리하여 조금씩 내 것으로 만들어 나가자.

아래는 이번에 공부한 내용을 정리한 TIL이다.

- [이터러블](JAVASCRIPT/Iterable.md)
- [객체의 프로퍼티 존재 확인과 열거](JAVASCRIPT/ObjectProperty.md)
- [정규 표현식이란?](RegExp/WhatisRegExp.md)
- [정규식 플래그](RegExp/RegExpFlag.md)

---

## 3. 독서

객체지향의 사실과 오해를 1월달에 2회독을 마무리하려고 한다. 벌써 일주일 밖에 남지 않았으니 부지런히 읽고 다음 책을 정해서 읽어보자.

개발 공부도 중요하지만 독서도 많이 하도록 하자. 개발 서적 뿐 아니라 자기 관리서, 소설 등 다양한 분야를 골고루 읽자!

---

## 4. 영어 공부

이번주 7일 동안 스픽 공부를 모두 하였다. 물론 자정을 넘어서 공부를 한 것도 있어 스픽 어플의 불꽃이 다시 초기화가 되었지만... 그래도 꾸준히 했다는 것이 뿌듯하다.

다음주에도 밀리지 말고 꾸준히 하자!

---

## 5. 그 외

1. 운동: 눈썹 문신을 하게 되어서 땀이 많이 나는 운동을 하지 못해 아쉬웠다. 오랜만에 시작하는 운동이라 몸이 많이 굳었지만 이제 슬슬 몸이 풀린 듯 하다. 다음 주차 땐 땀도 많이 흘리면서 체력을 키우는 것에 집중하자.
2. 우테코 크루 저녁 모임: 16일(월요일)에 함께 스터디를 했던 우테코 예비 크루들과 선릉에서 모임을 가지게 되었다. 나 포함 4명의 크루들이 모여서 저녁과 함께 술도 하였다. 처음 만난 자리여서 어색한 점도 있었지만 여러 이야기를 통해 조금은 가까워진 듯 하여 즐거운 시간이었다. 앞으로 좋은 교류를 통해 함께 성장을 했으면 한다.

---

📅 2023-01-23