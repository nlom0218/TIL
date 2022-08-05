# Transition

## 1. 개요

트랜지션(transition)은 CSS 프로퍼티의 값이 변화할 때, 프로퍼티 값의 변화가 일정 시간(durationi)에 걸쳐 일어나도록 하는 것이다. 예를 들어 웹 요소의 배경색을 바꾸거나 도형의 테두리를 사각형에서 원형으로 바꾸는 것처럼 스타일 속성이 바뀌는 것을 말한다.

transition의 프로퍼티는 아래와 같다.

| 프로퍼티                   | 설명                                                                                                             | 기본값 |
| :------------------------- | :--------------------------------------------------------------------------------------------------------------- | :----- |
| transition-property        | 트랜지션의 대상이 되는 CSS 프로퍼티를 지정한다.                                                                  | all    |
| transition-duration        | 트랜지션이 일어나는 지속시간을 초 단위 또는 밀리 초 단위로 지정한다.                                             | 0s     |
| transition-timing-function | 트래닞션 효과를 위한 수치 함수를 지정한다.                                                                       | ease   |
| transition-delay           | 프로퍼티가 변화한 시점과 트랜지션이 실제로 시작하는 사이에 대기하는 시간을 초 단위 또는 밀리 초 단위로 지정한다. | 0s     |
| transition                 | 모든 트랜지션 프로퍼티를 한번에 지정한다.(shorthand syntax)                                                      |        |

---

## 2. transition-property

트랜지션을 만들기 위해서는 `transition-property` 속성을 사용해야 한다. 해당 속성을 사용함으로써 트랜지션의 대상이 되는 CSS 프로퍼티명을 지정한다. 복수의 프로퍼티를 지정하는 경우 쉼표(,)로 구분한다.

기본형 `transition-property: all || none || <속성이름>`

모든 CSS 프로퍼티가 트랜지션의 대상이 될 수 없다. 아래는 트랜지션이 대상이 될 수 있는 CSS 프로퍼티이다.

| 구분       | 프로퍼티                                                                                                                 |
| :--------- | :----------------------------------------------------------------------------------------------------------------------- |
| Box model  | width, height, max-width, max-height, min-width, min-height, padding, margin, border-color, border-width, border-spacing |
| Background | background-color, background-position                                                                                    |
| 좌표       | top, left, right, bottom                                                                                                 |
| 텍스트     | color, font-size, font-weight, letter-spacing, line-height, text-indent, text-shadow, vertical-align, word-spacing       |
| 기타       | opacity, outline-color, outline-offset, outline-width, visibility, z-index                                               |

---

## 3. transition-duration

`transition-duration` 프로퍼티는 트랜지션에 일어나는 지속시간을 초 단위 또는 밀리 초 단위로 지정한다. 해당 프로퍼티를 통해 자연스럽게 바뀌는 애니메이션 효과를 만들 수 있다. 프로퍼티값을 지정하지 않을 경우 기본값 0s이 적용되어 어떠한 트랜지션 효과도 볼 수 없다.

기본형 `transition-duration: <시간>`

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        width: 120px;
        height: 120px;
        background-color: green;
        transition-property: width, height;
        transition-duration: 1s, 2s;
      }
      div:hover {
        width: 240px;
        height: 240px;
      }
    </style>
  </head>
  <body>
    <div></div>
  </body>
</html>
```

위의 코드의 경우 div box에 마우스를 올리게 되면 가로의 길이는 1초 동안 총 120px만큼 늘어나게 되고 높이는 2초 동안 120px만큼 늘어나게 된다.

---

## 4. transition-timing-function

해당 속성은 트래지션 효과의 변화 흐름, 시간에 따른 변화 속도와 같은 일종의 변화의 리듬을 지정한다. 트랜지션 효과의 시작, 중간, 끝에서 속도를 지정해 전체 속도 곡선을 만들 수 있다. 속도 곡서은 미리 정해진 키워드나 베지에 곡선을 이용해 표현한다.

기본형 `transition-timing-function: linear || ease || ease-in || ease-out || ease-in-out || cubic-bezier(n, n, n, n)`

아래는 transition-timing-function의 프로퍼티 값과 설명이다.
|프로퍼티 값|설명|
|:---|:---|
|ease|기본값으로 느리게 시작하여 점점 빨라졌다가 느리지면서 종료한다.|
|linear|시작부터 종료까지 등속 운동을 한다.|
|ease-in|느리게 시작한 후 일정한 속도에 다다르면 그 상태로 등속 운동한다.|
|ease-out|일정한 속도의 등속으로 시작해서 점점 느려지면서 종료한다.|
|ease-in-out|느리게 시작하고 느리게 끝난다.|
|cubic-bezier(n, n, n, n)|베지에 함수를 정의해서 사용한다. 이때 n값은 0~1 사이만 사용할 수 있다.|

---

## 5. transition-delay

`transition-delay` 프로퍼티는 트랜지션 효과를 언제부터 시작할 것인지 설정한다. 즉, `transition-delay`로 대기 시간을 지정하여 프로퍼티의 값이 변화하여도 즉시 트랜지션이 실행되지 않고, 일정 시간 대기한 후 트랜지션이 실행되도록 한다. 사용할 수 있는 값은 초(s)나 밀리초(ms)이며, 기본값은 0이다.

기본형 `transtition-delay: <시간>`

---

## 6. transition

위에서 설명한 모든 트랜지션 프로퍼티를 한번에 지정할 수 있는 shorthand이다.

기본형 `transition: <transition-property값> || <transition-duration값 || <transition-timing-function값> || <transition-delay값>`

기본값 `transition: all 0 ease 0`

프로퍼티값을 작성하는 순서는 상관이 없다. 다만 transition-duration은 반드시 지정해야 한다.

---

## 참고

[poiemaweb 2-14 트랜지션](https://poiemaweb.com/css3-transition)  
도서 - HTML + CSS + 자바스크립트 웹 표준의 정석

---

[👆](#transition)
