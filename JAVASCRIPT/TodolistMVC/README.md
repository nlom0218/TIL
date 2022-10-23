# 자바스크립트의 MVC 패턴

## 1. 개요

바닐라 자바스크립트와 MVC 패턴을 사용하여 `투두리스트`를 구현한다. 아직 정확한 개념이 잘 잡히지 않으므로 천천히 공부하며 필요하면 내용을 분리하여 추가 작성한다.

기본이 되는 `html` 구조는 아래와 같다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>TODO LIST</title>
  </head>
  <body>
    <div id="app"></div>
    <script type="module" src="./index.js"></script>
  </body>
</html>
```

---

## 2. MVC

- Model
  - 데이터를 관리하고 변경한다.(추가, 삭제, 수정)
- Controller
  - View의 이벤트에 의해서 동작되는 변경사항을 연결한다.
  - 이벤트 핸들러를 등록한다.
- View
  - 데이터를 받아 그대로 DOM에 추가한다.

---

## 3. TODO LIST

### TODO 등록하기

- [ ] 투두를 등록할 수 있는 폼을 만든다.
- [ ] 투두를 등록하면 input를 초기화한다.
- [ ] 투두를 관리하는 데이터 틀을 만든다.
- [ ] 투두를 등록하면 투두 데이터가 업데이트 된다.
- [ ] 투두 데이터가 업데이트 되면 그에 따라 View도 업데이트 된다.
- [ ] MVC 페턴으로 파일을 쪼갠다.
- [ ] 리펙토링

### TODO 완료 및 삭제하기

- [ ] 완료 버튼을 누르면 `done` 값이 `true`가 된다.
- [ ] `done` 값이 `false`인 `todo`만 랜더링을 한다.
- [ ] 삭제 버튼을 누르면 `todos`에서 제거된다.
- [ ] 제거된 `todo` 가 반영된 새로운 화면을 렌더링 한다.
- [ ] 완료된 투두라면 미완료로 바꾼다.
- [ ] 리팩토링

### TODO Localstorage에 저장하기

- [ ] 첫 로드 시 빈 투두 목록를 localstroage에 저장하기
- [ ] localstorage에 이미 저장된 투두 목록이 있다면 해당 투두 목록 불러오기
- [ ] 투두 생성시 localstorage의 투두 목록에 새로운 투두 추가하기
- [ ] 투두 수정시 localstorage의 투두 목록 수정하기
- [ ] 투두 삭세지 localstorage의 투두 목록 중 해당하는 투두 삭제하기
