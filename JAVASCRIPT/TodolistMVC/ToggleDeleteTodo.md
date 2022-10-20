# TODO 완료 및 삭제하기

## 1. 개요

이전 파트인 `TODO 등록하기`에서 구성한 파일 구조는 바뀌지 않았고 새로운 파일 또한 생성되지 않았다. Controller, View, Model에서의 코드만 추가되었다.

이전 파트에서 추가된 내용을 위주로 설명을 진행한다.

---

## 2. Controller

전체 코드

```javascript
import Todos from "../models/todos.js";
import { $ } from "../utils/dom.js";
import View from "../views/view.js";

export default class Controller {
  constructor() {
    this.todosModel = new Todos();
    this.view = new View();
    this.eventHandler();
  }

  addTodo = () => {
    this.todosModel.addTodo();
    View.todoRender(this.todosModel.todos);
  };

  toggleTodo = (todoId) => {
    this.todosModel.toggleTodo(todoId);
    View.todoRender(this.todosModel.todos);
  };

  deleteTodo = (todoId) => {
    this.todosModel.deleteTodo(todoId);
    View.todoRender(this.todosModel.todos);
  };

  eventHandler = () => {
    $("todo-form").addEventListener("submit", (e) => {
      e.preventDefault();
      this.addTodo();
    });

    $("todo-list").addEventListener("click", ({ target }) => {
      const { id } = target;
      const { todoId } = target.closest("li").dataset;
      if (id === "todo-done-btn") {
        this.toggleTodo(todoId);
        return;
      }
      if (id === "todo-delete-btn") {
        this.deleteTodo(todoId);
        return;
      }
    });
  };
}
```

### 2-1. Controller - 이벤트 핸들러 추가

```javascript
    $("todo-list").addEventListener("click", ({ target }) => {
      const { id } = target;
      const { todoId } = target.closest("li").dataset;
      if (id === "todo-done-btn") {
        this.toggleTodo(todoId);
        return;
      }
      if (id === "todo-delete-btn") {
        this.deleteTodo(todoId);
        return;
      }
    });
  };
```

위 코드는 `todo`를 완료, 삭제하기 위한 이벤트 핸들러이다.

이 부분에 어떻게 하면 코드를 깔끔하게 작성할 수 있을지 고민을 많이 하였다. 이밴트 위임을 이용하여 버튼을 눌렀을 때 부모 노드 중 `li` 태그를 찾아 해당 태그의 `data 어트리뷰트`의 값을 가져오기 위한 고민을 하였다. 처음엔 아래와 같이 값을 구했다.

```javascript
const {
  parentNode: {
    dataset: { todoId },
  },
} = target;
```

위와 같은 방법으로도 `data 어트리뷰트` 값을 가져올 수 있지만 `parentNode`가 `li` 태그인 것을 확신해야 가능한 코드이다. 그렇기 때문에 `closest()` 메서드를 사용하여 코드를 리팩토링하였다.

또한 `taget`의 `id`에 따라 완료 버튼인지 삭제 버튼인지를 구분하여 각각 서로 다른 함수를 실행하도록 하였다.
즉, 완료 버튼을 클릭하였을 땐 `toggleTodo()` 함수가 실행되고 삭제 버튼을 클릭하였을 땐 `deleteBtn()`이 실행된다.

---

### 2-2. Controller - TODO 완료하기

```javascript
toggleTodo = (todoId) => {
  this.todosModel.toggleTodo(todoId);
  View.todoRender(this.todosModel.todos);
};
```

완료 버튼을 누르게 되면 이밴트 핸들러에 의해 `toggleTodo()` 함수가 실행된다. `toggleTodo()` 함수를 통해 `todosMdoel`의 `toggleTodo()` 함수가 실행되고 `todos` 목록이 다시 랜더링 된다. 두 가지는 각각 `Model`, `View`에 전달되어 각각의 기능을 실행한다.

---

### 2-3. Controller - TODO 삭제하기

```javascript
deleteTodo = (todoId) => {
  this.todosModel.deleteTodo(todoId);
  View.todoRender(this.todosModel.todos);
};
```

삭제 버튼을 누르게 되면 이벤트 핸들러에 의해 `deleteTodo()` 함수가 실행된다. 해당 함수도 `Model`과 `View`를 각각 수정하는 함수가 실행된다.

---

## 3. Model

전체 코드

```javascript
import { $ } from "../utils/dom.js";

export default class Todos {
  constructor() {
    this.todos = [];
  }

  addTodo = () => {
    const todo = $("todo-input").value;
    $("todo-input").value = "";
    this.todos.push({ todo, done: false });
  };

  toggleTodo = (todoId) => {
    const isDone = this.todos[todoId].done;
    this.todos[todoId].done = isDone ? false : true;
  };

  deleteTodo = (todoId) => {
    this.todos.splice(todoId, 1);
  };
}
```

## 3-1. Model - TODO 완료하기

```javascript
toggleTodo = (todoId) => {
  const isDone = this.todos[todoId].done;
  this.todos[todoId].done = isDone ? false : true;
};
```

완료하기 버튼을 누르게 되면 `Model`의 `todos.js`에서는 `toggleTodo()` 함수가 실행된다. `todoId`를 매개변수로 받고 먼저 해당 `todo`가 완료가 되었는지 안 되었는지 확인을 한다. 확인된 결과는 `Blooean` 값으로 `isDone`에 할당된다.

이후 `this.todos`의 배열 중 `todoId`에 해당하는 요소의 `done` 값을 바꾸는데 이때 `isDone`에 따라 값을 수정한다. 이렇게 `완료`, `미완료`에 대한 값을 서로 바꿀 수 있다.

---

### 3-2. Model - TODO 삭제하기

```javascript
deleteTodo = (todoId) => {
  this.todos.splice(todoId, 1);
};
```

삭제하기 버튼을 누르게 되면 `Model`의 `todos.js`에서는 `deleteTodo()` 함수가 실행이 되고 해당 함수는 `todoId`를 매개변수를 받고 `splice()` 메서드를 사용하여 `todoId`에 해당하는 요소를 제거한다.

---

## 4. View

`View`에서는 `todoRender()` 함수를 수정하였다.

```javascript
  static todoRender = (todos) => {
    const template = todos
      .map(({ todo, done }, index) => {
        return `<li data-todo-id=${index}>
            <span style="text-decoration: ${
              done && "line-through"
            }">${todo}</span>
            <button id="todo-done-btn">${done ? "완료함" : "완료"}</button>
            <button id="todo-delete-btn">삭제</button>
        </li>
        `;
      })
      .join("");
    $("todo-list").innerHTML = template;
  };
```

먼저 `dataset 어트리뷰트`를 사용하기 위해 `li` 태그에 `data-todo-id` 어트리뷰트를 추가하였고 해당 값으로 `index`를 할당하였다.

또한 `done`에 값에 따라 `todo`의 중간에 선을 긋거나 긋지 않게 하였다. 마찬가지로 완료 버튼을 눌러 완료가 되었으면 완료함으로 바꾸게 하였다. 다시 한 번 더 누르면 완료로 바꾼다. 즉 toggle이 된다.

---

## 5. Conclusion

> 한 번 구조를 짜고 추가 기능을 구현을 하니 확실히 코드 구현이 간단하게 느껴졌다. View, Model, Controller에서 하는 역할이 각각 있으니 역할 분담이 눈에 띄었고 연결도 매끈해 보였다. 하지만 이것이 맞는 방법인지는 여전히 의문이다. 고민하고 공부하는 것은 좋으나 남을 것을 무작정 따라하여 이해도 되지 않는 구조를 만들지는 말자. 이유가 있을 때 코드를 작성하자.

---

📅 2022-10\*21
