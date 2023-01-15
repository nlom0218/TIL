# 이벤트 위임

## 1. 개요

이벤트 위임(Event Delegation)은 캡쳐링과 버블링을 이용한 것으로, 여러 엘리먼트마다 각각 이벤트 핸들러를 할당하지 않고, 공통되는 부모에 이벤트 핸들러를 할당하여 이벤트를 관리하는 방식이다. 여러 예제를 통해 이벤트 위임이 어떻게 사용되는지 알아보자.

---

## 2. 이벤트 위임을 사용하지 않은 예제

이벤트 위임을 하지 않는다면 어떤 불편한 점이 있는지 예제를 통해 알아보자.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="animals">
      <li id="cat">고양이</li>
      <li id="dog">강아지</li>
      <li id="tiger">호랑이</li>
    </ul>
    <script>
      const $cat = document.getElementById('cat');
      const $dog = document.getElementById('dog');
      const $tiger = document.getElementById('tiger');

      function handleClickAnimal({ target }) {
        const {
          style: { color },
        } = target;

        if (color === 'red') return (target.style.color = 'black');

        target.style.color = 'red';
      }

      $cat.addEventListener('click', handleClickAnimal);
      $dog.addEventListener('click', handleClickAnimal);
      $tiger.addEventListener('click', handleClickAnimal);
    </script>
  </body>
</html>
```

`ul` 태그에 3개의 `li` 자식 태그가 있고 각각의 고유한 아이디를 가지고 있다. 자바스크립트 코드를 보면 `li`를 클릭하게 되면 글씨의 색깔이 바뀌는 간단한 클릭 이벤트를 만들었다.

만약 `li` 태그가 3개가 아니라 100개, 1000개가 있다고 생각해보자. 그렇데 되면 위와 같이 하나하나 이벤트를 등록할 수 있는가? 할 수는 있지만 어느 누구도 시도를 하지 않을 것이다. 단순 노가다이면서 매우 귀찮은 일이기 때문이다. 이런 문제는 이벤트 위임을 통해 아주 간단히 해결할 수 있다.

---

## 3. 이벤트 위임을 사용한 예제

위의 예제를 이벤트 위임을 사용하여 리팩터링을 해보자.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="animals">
      <li>고양이</li>
      <li>강아지</li>
      <li>호랑이</li>
    </ul>
    <script>
      const $animals = document.getElementById('animals');

      function handleClickAnimal({ target }) {
        const {
          style: { color },
        } = target;

        if (color === 'red') return (target.style.color = 'black');

        target.style.color = 'red';
      }

      $animals.addEventListener('click', handleClickAnimal);
    </script>
  </body>
</html>
```

`li` 태그에는 더 이상 `id` 에트리뷰트가 필요없다. 때문에 제거를 하였다.

이어서 자바스크립트 코드를 보면 `ul` 태그(아이디가 animals인 노드)에 클릭 이벤트를 등록하였다. 그 이후의 코드는 이전과 같다.

정리하자면 `ul` 태그의 자식 노드에 하나하나 이벤트를 등록한 것이 아니라 이벤트가 필요한 노드들이 공통적으로 가지고 있는 부모 노드에 이벤트 등록을 한 것이다. 즉, 자식 노드들의 이벤트를 부모 노드가 위임을 하였다.

### 3-1. target vs currentTarget

`target`과 `currentTarget`에 대해서 구분하여 알아둬야 한다. 먼저 콘솔을 통해 무엇이 찍히는지 알아보자. 기존 색깔을 바꾸는 코드는 지우고 아래와 같이 작성해보자. 그리고 목록의 아이템을 클릭해보자.

```html
<script>
  const $animals = document.getElementById('animals');

  function handleClickAnimal({ target, currentTarget }) {
    console.log(`target: ${target}`); // target: [object HTMLLIElement]
    console.log(`currentTarget: ${currentTarget}`); // currentTarget: [object HTMLUListElement]
  }

  $animals.addEventListener('click', handleClickAnimal);
</script>
```

`target`은 `li` 요소가, `currentTarget`은 `ul` 요소가 찍히는 것을 확인할 수 있다. 정리하면 아래와 같다.

- target: 실제 이벤트가 발생하는 요소
- currentTarget: 이벤트 리스너가 달린 요소

이 두개를 구분하여 필요한 이벤트를 실행시키자.

### 3-2. Element.prototype.matches

`Element.prototype.matches` 메서드는 인수로 전달된 선택자에 의해 특정 노드를 탐색 가능한지 확인한다. 위의 예제에서 `ul` 태그 내 `li` 태그가 아닌 다른 태그들이 있다면 클릭 이벤트를 실행시키고 싶지 않다고 가정해보자. `Element.prototype.matches` 메서드는 이럴 때 유용하게 사용할 수 있다.

이를 활용한 예제를 살펴보자.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="animals">
      <li>고양이</li>
      <li>강아지</li>
      <li>호랑이</li>
      <span>햄스터</span>
    </ul>
    <script>
      const $animals = document.getElementById('animals');

      function handleClickAnimal({ target }) {
        if (!target.matches('#animals li')) return;

        const {
          style: { color },
        } = target;

        if (color === 'red') return (target.style.color = 'black');

        target.style.color = 'red';
      }

      $animals.addEventListener('click', handleClickAnimal);
    </script>
  </body>
</html>
```

`ul` 태그에 `span` 태그가 추가되었다. 하지만 `span` 태그를 클릭하게 되면 이벤트가 동작하지 않고 싶다. 그렇기 위해서 `handleClickAnimal` 함수가 실행되고 유효성 검사를 실시하는 데 이때 `Element.prototype.matches` 메서드를 사용하였다. 인수로 `#animals li`를 전달하여 `#animals` 아이디를 가지는 노드의 자식 중 `li` 노드가 아닌 경우 함수를 곧바로 종료하는 것이다.

```javascript
if (!target.matches('#animals li')) return;
```

---

## 4. 사용자에 의해 새롭게 추가된 요소에서 이벤트 다루기

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <form>
      <input type="text" />
    </form>
    <ul id="list"></ul>
    <script>
      const $form = document.querySelector('form');
      const $input = document.querySelector('form input');
      const $list = document.querySelector('ul');

      function paintItem(item) {
        $list.insertAdjacentHTML('beforeend', `<li>${item}</li>`);
      }

      function handleSubmitForm(event) {
        event.preventDefault();

        const item = $input.value;
        $input.value = '';
        paintItem(item);
      }

      function handleClickItem({ target }) {
        alert(target.innerText);
      }

      $form.addEventListener('submit', handleSubmitForm);
      $list.addEventListener('click', handleClickItem);
    </script>
  </body>
</html>
```

`submit` 이벤트에 이해 새로운 리스트의 아이템이 `ul` 태그에 추가되고 있다. 또한 추가된 아이템들을 클릭할 때 `alert` 함수가 실행된다. 아이템에 하나하나 이벤트를 등록하지 않고 부모 노드인 `ul`에 이벤트를 등록한 것이 이벤트 위임이다.

이렇듯 이벤트 위임은 새롭게 추가된 요소에 대해 이벤트를 등록하지 않아도 된다는 장점이 있다.

---

## 5. HTML data-\* 속성과 함께 사용하기

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div>
      <input data-action="save" type="button" value="Save" />
      <input data-action="reset" type="button" value="Reset" />
      <input data-action="upload" type="button" value="Upload" />
    </div>
    <ul id="list"></ul>
    <script>
      const $button = document.querySelector('div');

      const acitonHandler = {
        save() {
          alert('save');
        },

        reset() {
          alert('reset');
        },

        upload() {
          alert('upload');
        },
      };

      function handleClickButton({ target }) {
        const {
          dataset: { action },
        } = target;

        acitonHandler[action]();
      }

      $button.addEventListener('click', handleClickButton);
    </script>
  </body>
</html>
```

`data-*` 어트리뷰트에 특정 값을 지정함으로써 이를 통해 서로 다른 특정 동작을 연결시키는 예제이다.

---

## 6. Conclusion

> 이벤트 위임에 대해 살펴보았다. 처음 자바스크립트를 배울 때 이를 이해하는 것이 어려워 좌절했던 기억이 난다. 지금보면 크게 어렵지 않게 이해가 된다. 물론 이를 유용하게 사용하는 것은 또다른 문제이지만 말이다. 최근에 작업하고 있는 자바스크립트 모먼트 앱도 이벤트 위임을 사용하여 리팩터링을 진행하고 있다. 유용한 기능은 맞지만 정확하게 사용하도록 하자. 추가적으로 이벤트 위임은 잊고 살았던 개념이다. 그러던 중 스터디원 중 한 분이 이에 대해 이야기를 하는 것을 보고 정리를 하게 된 것이다. 스터디의 순기능 중 하나인 듯 하다.👍

---

## 참고

[[JavaScript] 이벤트 위임](https://velog.io/@moonheekim0118/JavaScript-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%B2%84%EB%B8%94%EB%A7%81)  
[이벤트에서 target과 currentTarget의 차이](https://kyounghwan01.github.io/blog/JS/JSbasic/target-currentTarget/)  
[[JS] Event Delegation(이벤트 위임)](https://hangeoreum.tistory.com/entry/JS-Event-Delegation%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EC%9C%84%EC%9E%84)  
도서 - 모던 자바스크립트 Deep Dive: 자바스크립트의 기본 개념과 동작 원리

---

📅 2022-01-15
