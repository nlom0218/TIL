# 문자열 다루기

## 1. 개요

토스와 Airbnb에서 제안하는 문자열 컨벤션에 대해 알아본다.

---

## 2. 문자열에는 작은 따옴표('')를 사용하자.

```javascript
const name = 'Kim Hong Dong';
```

---

### 2-1. vscode에서 저장할 때 자동으로 큰 따옴표를 작은 따옴표로 바꾸는 방법

1. setting으로 이동
   - Mac 기준
   - 좌측 상단의 Code -> preference -> setting  
     ![setting으로 이동1](/image/JS/CodingConvention/String/setting1.png)
   - 또는 좌측 하단 Manage(톱니바퀴) -> setting  
     ![setting으로 이동2](/image/JS/CodingConvention/String/setting2.png)
2. quote 검색
3. Extensions의 하위 항목인 Prettier 선택
4. Prettier: Single Quote을 아래의 사진과 같이 체크하기  
   ![Single Quote 체크](/image/JS/CodingConvention/String/setting3.png)

---

## 3. 여러 줄에 걸쳐 쓰지 않는다.

문자열이 중간에 끊어지면 작업하기가 어려워지고 코드를 찾기 어렵게 한다.

```javascript
// bad
const message =
  'Lorem ipsum dolor sit amet consectetur adipisicing elit. \
  Ab dolor rem temporibus ratione ducimus dicta, ea voluptas id, \
  quae illo repudiandae minus enim nesciunt soluta expedita incidunt \
  non pariatur labore.';

// bad
const message =
  'Lorem ipsum dolor sit amet consectetur adipisicing elit.' +
  'Ab dolor rem temporibus ratione ducimus dicta, ea voluptas id,' +
  'quae illo repudiandae minus enim nesciunt soluta expedita incidunt' +
  'non pariatur labore.';

// good
const message =
  'Lorem ipsum dolor sit amet consectetur adipisicing elit. Ab dolor rem temporibus ratione ducimus dicta, ea voluptas id quae illo repudiandae minus enim nesciunt soluta expedita incidunt non pariatur labore.';
```

---

## 4. 템플릿 문자열을 사용하여 문자열을 생성하자.

변수를 함께 사용하여 문자열을 만들 경우 템플릿 문자열을 사용한다면 더욱 간결하게 생성할 수 있고 가독성 또한 높아진다.

변수를 함께 사용하지 않느다면 굳이 템플릿 문자열을 사용할 필요는 없다.

```javascript
// bad
function sayMySelf(name, age) {
  return '안녕하세요! ' + '전 ' + name + '입니다. 나이는 ' + age + '살입니다.';
}

// good
function sayMySelf(name, age) {
  return `안녕하세요! 전 ${name}입니다. 나이는 ${age}살입니다.`;
}
```

---

## 5. Conclusion

> 프로그래밍을 하면서 문자열은 정말로 많이 다룬다. 위의 내용을 정리하면서 나름대로 문자열에 대한 코딩 컨벤션은 잘 지키고 있었다는 생각이 들었다. 물론 문자열을 나타낼 때 지금까지 큰 따옴표를 사용했지만 이는 에디터 환경설정을 변경하는 것으로 쉽게 수정할 수 있으니 큰 문제를 느끼지 않았고 편안한 마음으로 학습을 진행하였다.

---

## 참고

[토스 코딩컨벤션 - 선언과 할당/템플릿 문자열](https://ui.toast.com/fe-guide/ko_CODING-CONVENTION#%ED%85%9C%ED%94%8C%EB%A6%BF-%EB%AC%B8%EC%9E%90%EC%97%B4)  
[Airbnb JavaScript 스타일 가이드 - 문자열 (Strings)](https://github.com/parksb/javascript-style-guide#%EB%AC%B8%EC%9E%90%EC%97%B4-strings)

---

📅 2022-11-11
