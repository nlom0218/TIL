# 이벤트 전파

## 1. 개요

버튼을 클릭하거나 폼 데이터를 전송하거나 스클롤 등 다양한 이벤트가 발생하게 되면 해당 이벤트는 DOM 트리를 통해 전파된다. 이를 이벤트 전파라고 한다. 이에 대해 살펴보자.

---

## 2. 이벤트 전파 과정

생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소인 이벤트 타깃을 중심으로 DOM 트리를 통해 전파된다.

![event propagation](/image/JS/EventPropagation/event-propagation.png)

위 사진에서 보이는 것과 같이 이벤트 전파 과정은 3단계로 구분할 수 있다.

1. 캡쳐링 단계(capturing phase): 이벤트가 상위 요소에서 하위 요소 방향으로 전파
2. 타싯 단계(target phase): 이벤트가 이벤트 타깃에 도달
3. 버블링 단계(bubbling phase): 이벤트가 하위 요소에서 상위 요소 방향으로 전파

---

## 3. 예제로 살펴보는 버블링 단계와 캡쳐링 단계

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
      <li>Orange</li>
    </ul>
    <script>
      const $fruits = document.getElementById('fruits');
      function handleClick(event) {
        const { eventPhase } = event;

        console.log(`이벤트 단계: ${eventPhase}`); // 3: 버블링 단계
      }
      $fruits.addEventListener('click', handleClick);
    </script>
  </body>
</html>
```

위의 예제에서 `Apple`를 클릭하면 어떤 이벤트 단계가 캐치 될까? 바로 3단계인 `버블링 단계`가 캐치된다.

차근차근 살펴보자. `Apple`를 클릭할 때 이벤트의 전파 과정이다.

1. 클릭 이벤트 객체가 생성되고 `Apple`이 이벤트 타깃이 된다.
2. 클릭 이벤트 객체는 window에서 이벤트 타깃 방향으로 전파된다. -> `1: 캡처링 단계`
3. 클릭 이벤트 객체는 이벤트 타깃에 도달한다. -> `2: 타싯 단계`
4. 클릭 이벤트 객체는 이벤트 타깃에서 window 방향으로 전파된다. -> `3: 버블링 단계`

`ul` 태그 입장에서 볼 땐 자신에게 이벤트가 달려있고 이벤트 타겟은 자신의 자식 요소인 `Apple`이다.
그러므로 위의 예제에서 `버블링 단계`가 캐치된 이유는 이벤트 객체가 `Apple`를 찍고 window 방향으로 전파될 때 `ul` 태그가 `버블링 단계`에 포함되어 있기 때문이다.

그렇다면 `캡쳐링 단계`는 어떻게 캐치될 수 있을까? 바로 `addEventListener` 메서드의 3번째 인수로 `true`를 전달해야 한다. 아래와 같이 수정해보자.

```html
<script>
  const $fruits = document.getElementById('fruits');
  function handleClick(event) {
    const { eventPhase } = event;

    console.log(`이벤트 단계: ${eventPhase}`); // 1: 캡쳐링 단계
  }
  $fruits.addEventListener('click', handleClick, true);
</script>
```

`타깃 단계`를 캐치하기 위해선 추가적인 작업이 필요하다. 왜냐? 위의 예제에서는 `li` 태그에 어떠한 이벤트도 추가하지 않았기 때문이다.

---

## 3. 예제로 살펴보는 이벤트 전파 과정

전체 과정을 살펴보며 마무리 해보자.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li id="apple">Apple</li>
      <li>Banana</li>
      <li>Orange</li>
    </ul>
    <script>
      const $fruits = document.getElementById('fruits');
      const $apple = document.getElementById('apple');

      $fruits.addEventListener(
        'click',
        ({ eventPhase }) => {
          console.log(`이벤트 단계: ${eventPhase}`); // 1: 캡쳐링 단계
        },
        true
      );

      $apple.addEventListener('click', ({ eventPhase }) => {
        console.log(`이벤트 단계: ${eventPhase}`); // 2: 타깃 단계
      });

      $fruits.addEventListener('click', ({ eventPhase }) => {
        console.log(`이벤트 단계: ${eventPhase}`); // 3: 버블링 단계
      });
    </script>
  </body>
</html>
```

---

## 4. 깜짝 문제

아래의 코드를 보고 버튼을 클릭했을 때 콘솔이 찍히는 순서를 맞춰보세요:)

```html
<!DOCTYPE html>
<html>
  <body>
    <div>Hello <button>버튼</button></div>
    <script>
      document.body.addEventListener(
        'click',
        () => {
          console.log('body의 클릭 이벤트 실행');
        },
        true
      );

      document.querySelector('div').addEventListener('click', () => {
        console.log('Hello의 클릭 이벤트 실행');
      });

      document.querySelector('button').addEventListener('click', () => {
        console.log('버튼의 클릭 이벤트 실행');
      });
    </script>
  </body>
</html>
```

순서는 아래와 같다.

1. body의 클릭 이벤트 실행
2. 버튼의 클릭 이벤트 실행
3. Hello의 클릭 이벤트 실행

버블링 단계와 캡쳐링 단계에서 이벤트를 캐치하는 과정을 그려보면 쉽게 알 수 있다.

---

## 5. Conclusion

> 이벤트의 전파 과정에 대해 살펴보았다. 단순히 내가 클릭한 요소에서만 이벤트가 발생하는 것이 아니라 이벤트 객체가 이벤트 타깃까지 전파되는 과정 속에 있는 모든 요소에서 이벤트가 발생한다고 하니 앞으로 이벤트를 다룰 때 겹치지 않게 조심히 해야 겠다. 그리고 클릭 이벤트 객체가 전파된다면 클릭 이벤트를 단 요소들만 동작하는 것 같다. 이어서 이벤트 위임과 DOM 요소의 기본 동작의 조작에 대해 공부하자.

---

## 참고

도서 - 모던 자바스크립트 Deep Dive: 자바스크립트의 기본 개념과 동작 원리

---

📅 2022-01-14
