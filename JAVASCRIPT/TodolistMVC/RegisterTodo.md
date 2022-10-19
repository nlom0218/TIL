# 투두 등록하기

## 1. 투두 등록 폼 만들기

```javascript
// index.js
const $app = document.getElementById("app");

const $form = document.createElement("form");
const $input = document.createElement("input");
const $button = document.createElement("button");
$input.placeholder = "투두를 입력하세요.";
$button.textContent = "등록하기";

$form.appendChild($input);
$form.appendChild($button);
$app.appendChild($form);
```

![RegisterTodo](/image/JS/TodolistMVC/RegisterTodo1.png)

`createElement()` 메서드와 `appendChild()` 메서드를 사용하여 요소 노드를 만들고 DOM에 추가한다.

---

## 2. 폼 전송 시 input 초기화

```javascript
function App() {
  const addTodo = () => {
    const todo = $input.value;
    $input.value = "";
  };
  $form.addEventListener("submit", (e) => {
    e.preventDefault();
    addTodo();
  });
}

new App();
```

`$form` 요소 노드에 `submit` 이벤트를 등록한다. `preventDefault()` 메서드를 사용하여 DOM 요소의 기본 동작을 중단하고 `addTodo()` 함수를 실행한다. 이곳에서는 `$input` 요소 노드의 `value`를 `todo` 변수에 할당하고 `value`를 빈 문자열로 바꾼다.

---

## 3. 투두 데이터 업데이트

```javascript
function App() {
  let todos = [];

  const addTodo = () => {
    const todo = $input.value;
    todos.push({ todo, done: false });
    $input.value = "";
  };
  $form.addEventListener("submit", (e) => {
    e.preventDefault();
    addTodo();
  });
}

new App();
```

빈 배열의 `todos`를 선언한다. `todos`는 앞으로 관리할 투두 리스트가 된다. `addTodo` 함수에서 `push()` 메서드를 사용하여 `todo`와 `done`(기본값으로 `false`)를 담은 객체를 추가한다.

---

## 4. 투두 데이터 업데이트에 따른 View(DOM) 업데이트

```javascript
function App() {
  let todos = [];

  const addTodoView = (todo) => {
    if (!document.querySelector(".todo-list")) {
      const $ul = document.createElement("ul");
      $ul.classList.add("todo-list");
      $app.appendChild($ul);
    }
    const $todoList = document.querySelector(".todo-list");
    const $li = document.createElement("li");
    $li.id = todos.length;
    $li.style.color = "black";
    $li.textContent = todo;
    $todoList.appendChild($li);
  };

  const addTodo = () => {
    const todo = $input.value;
    todos.push({ todo, done: false });
    $input.value = "";
    addTodoView(todo);
  };
  $form.addEventListener("submit", (e) => {
    e.preventDefault();
    addTodo();
  });
}

new App();
```

`addTodoView()` 함수를 통해 View(DOM)를 업데이트한다. 해당 함수는 `addTodo()` 함수가 실행된 후 호출되어 실행된다.

![RegisterTodo2](/image/JS/TodolistMVC/RegisterTodo2.png)

---

## 5. MVC 패턴으로 분리 및 리팩토링

티렉토리 구조

```bash
├── src
│   ├── controllers
│   │   ├── controller.js
│   ├── models
│   │   ├── todos.js
│   ├── utils
│   │   └── dom.js
│   ├── views
│   │   ├── view.js
├── index.html
└── index.js

```

### 5-1. Utils Function

파일 위치: src/utils/dom.js

DOM과 관련된 변수, 함수를 관리하기 위한 파일이다. 해당 파일엔 `<div id="app"></div>` 노드를 가져와 할당한 변수 `$app`와 요소 노드를 취득하기 위한 함수 `$`가 있다.

```javascript
export const $app = document.getElementById("app");
export const $ = (id) => document.getElementById(id);
```

---

### 5-2 View

파일 위치: src/views/view.js

```javascript
import { $, $app } from "../utils/dom.js";

export default class View {
  constructor() {
    this.render();
  }
  render() {
    const template = `
        <form id="todo-form">
            <input 
                placeholder="투두를 입력하세요." 
                id="todo-input"
                autocomplete="off"
            />
            <button>등록하기</button>
        </form>
        <ul id="todo-list">
        </ul>
    `;
    $app.innerHTML = template;
  }

  static todoRender = (todos) => {
    const template = todos
      .map(({ todo, done }, index) => {
        return `<li id=${index}>
            <span>${todo}</span>
            <button id="todo-done-btn">${done ? "완료함" : "완료"}</button>
            <button id="todo-delete-btn">삭제</button>
        </li>
        `;
      })
      .join("");
    $("todo-list").innerHTML = template;
  };
}
```

`View`는 크게 두 가지의 역할을 한다. 처음 생성될 때 `render()` 함수를 실행하여 뼈대가 되는 `html`를 그린다. 이때 todo를 등록할 수 있는 `form` 태그와 등록된 todo가 그려질 `ul` 태그가 생성된다.

`todoRender()` 함수는 `static`으로 사용한다. 해당 함수를 호출하면 생성된 모든 투두를 `todos`로 받아 목록을 그린 후 `innerHTML` 프로퍼티를 사용하여 `todo-list`의 자식 노드로 생성한다.
