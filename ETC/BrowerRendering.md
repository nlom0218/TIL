# 브라우저 렌더링 과정

## 1. 개요

브라우저가 HTML, CSS, 자바스크립트로 작성된 텍스트 문서를 어떻게 파싱(해석)하여 브라우저에 렌더링하는지 알아보자.

- 파싱(parsing): 파싱은 프로그래밍 언어의 문법에 맞게 작성된 텍스트 문서를 읽어 들여 토큰으로 분해하고, 트리 구조의 자료구조인 파스 트리를 생성하는 일련의 과정
- 렌더링(rendering): HTML, CSS, 자바스크립트로 작성된 문서를 파싱하여 브라우저에 시각적으로 출력하는 것

---

## 2. HTML 파싱과 DOM 생성

서버에 html 파일을 요청하면 서버는 해당 html 파일을 브라우저에 응답을 해준다. 이후 아래의 과정을 통해 HTML 파싱과 DOM이 생성된다.

1. 서버는 `바이트(2진수`)를 인터넷을 경유하여 응답한다.
2. 브라우저는 인코딩 방식(예: UTF-8)을 기준으로 `문자열`로 변환한다.
3. 문법적 의미를 갖는 코드의 최소 단위인 `토큰(token)`들로 분해한다.
4. 각 토큰들을 객체로 변환하여 `노드(node)`들을 생성한다. 이때 생성된 노드는 DOM를구성하는 기본 요소가 되며 토큰의 내용에 따라 문서 노드, 요소 노드, 어트리뷰트 노드, 텍스트 노드가 생성된다.
5. HTML 요소 간의 부자 관계를 반영하여 모든 노드들이 `트리 자료구조`로 구성한다. 이를 `DOM(Document Object Model)`이라 부른다.

즉, DOM은 HTML 문서를 파싱한 결과물이다.

![HTML 파싱과 DOM 생성](/image/ETC/BrowerRendering/html-parsing.png)

---

## 3. CSS 파싱과 CSSOM 생성

DOM을 생성하는 도중 CSS를 로드하는 `link 태그`나 `style 태그`를 만나면 DOM 생성을 일시 중단한다.

이후 href 어트리뷰트에 지정된 CSS 파일을 서버에 요청하여 로드한 CSS 파일이나 style 태그 내의 CSS를 파싱 과정을 거치며 해석한다.

해당 과정은 아래와 같이 이루어진다.

1. 바이트
2. 문자
3. 토큰
4. 노드
5. CSSOM

즉, HTML과 동일한 과정을 거친다. 단, HTML 파싱으로는 DOM이 생성되는 반면 CSS 파싱으로는 `CSSOM(CSS Object Model)`이 생성된다.

![css-parsing](/image/ETC/BrowerRendering/css-parsing.png)

CSS 파싱과 HTML 파싱은 동시에 이루어지지 않는다. 즉, CSS 파싱을 하는 동안 HTML 파싱은 잠시 중단을 한다.

이후 CSS 파싱이 모두 완료되면 중단된 시점부터 다시 HTML을 파싱하기 시작하여 DOM 생성을 재개한다.

> 의문점이 생긴다. head 태그내에 있는 link 태그를 만나 CSS 파싱과 CSSOM를 만들기 시작하면 부모 자식 관계를 어떻게 알 수 있는 것일까? body 태그는 모든 태그들의 부모가 될 수 있지만 div 태그는 어떤 태그가 부모이고 어떤 태그가 자식인지는 HTML를 모두 파싱을 해야 알 수 있는 것이 아닌가?  
> 혹은 HTML 파싱을 모두 하지 않더라도 CSS 파싱을 하는 도중 부모 자식 관계를 반영하기 위해 HTML를 참고하는 것인가? 이래야 CSSOM를 만들기 위한 과정이 납득이 된다.

---

## 4. 렌더 트리 생성

HTML과 CSS를 파싱하여 각각 DOM과 CSSOM를 생성을 하였다면 드디어 이 둘을 렌더링을 위해 `렌더 트리(render tree)`로 결합된다.

![render tree](/image/ETC/BrowerRendering/rendertree.png)

렌더 트리는 렌더링을 위한 트리 구조의 자료구조다. 렌더 트리는 브라우저 화면에 렌더링되는 노드만으로 구성된다. 즉, meta 태그, scrip 태그, CSS에 의해 비표시(예: display: none;)되는 노드들은 포함하지 않는다.

이후 완성된 렌더 트리는 각 HTML 요소의 레이아웃(위치와 크기)을 계산하는 데 사용되며 브라우저 화면에 픽셀을 렌더링하는 페인팅처리에 입력된다.

![렌더 트리와 레이아웃/페인트](/image/ETC/BrowerRendering/layout-paint.png)

### 4-1. 레이아웃

렌더링 트리를 생성하면서 해당 노드의 계산된 스타일을 계산한다. 하지만 기기에 따른 노드들의 정확한 위치와 크기를 계산하기 않았다.

노드가 실질적으로 뷰포트에서 표시될 위치와 크기를 정하는 일을 `레이아웃`이라고 하며 상황에 따라서는 `리플로우`라고도 한다.

### 4-2. 페인팅

레이아웃을 통해 렌더링 트리의 각 노드가 실질적으로 차지하는 값을 계산하고 노드들을 기하학적 형태를 구했다. 이를 화면에 그려서 사용자에게 표시하는 작업을 `페인팅` 또는 `래스터화`라고 부른다.

### 4-3. 반복 실행

브라우저의 렌더링 과정은 반복해서 실행될 수 있다. 아래의 경우 레이아웃 계산과 페인팅이 재차 실행된다.

1. 자바스크립트에 의한 노드 추가 또는 삭제
2. 브라우저 창의 리사이징에 의한 뷰포트크기 변경
3. HTML 요소의 레이아웃에 변경을 발생시키는 `width/height, margin, padding, display, position, top/right/bottom/left 등의 스타일 변경

이에 관한 내용은 아래에서 다시 다룬다.

---

## 5. 리플로우와 리페인트

자바스크립트에 의해 DOM API가 사용된 경우 변경된 DOM과 CSSOM은 다시 렌더 트리로 결합되고 변경된 렌더 트리를 기반으로 레이아웃과 페인트 과정을 거쳐 브라우저의 화면에 다시 렌더링한다. 이를 리플로우, 리페인트라 한다.

- 리플로우: 레이아웃 계산을 다시 하는 것을 말하며, 노드 추가/삭제, 요소의 크기/위치 변경, 윈도우 리사이징 등 레이아웃에 영향을 주는 변경이 발생한 경우에 한하여 실행
- 리페인트: 재결합된 렌더 트리를 기반으로 다시 페인트를 하는 것

레이아웃에 영향이 없는 변경은 리플로우 없이 리페인트만 실행된다.

![리플로우와 리페인트](/image/ETC/BrowerRendering/reflow-repaint.png)

---

## 6. 자바스크립트 파싱과 실행 그리고 HTML 파싱 중단

CSS 파싱 과정과 마찬가지로 자바스크립트 파일을 로드하는 script 태그나 자바스크립트 코드를 콘텐츠로 담은 script 태그를 만나면 DOM 생성을 일시 중단하고 자바스크립트 파싱을 시작한다.

자바스크립트 파싱이 HTML 파싱, CSS 파싱과 다른 점은 브라우저의 렌더링 엔진이 아닌 자바스크립트 엔진이 처리한다는 것이다. 이 자바스크립트 엔진은 CPU가 이해할 수 있는 저수준 언어로 변환하고 실행하는 역할을 한다.

아래는 자바스크립트가 파싱되고 실행되는 과정을 나타낸 사진다.

![자바스크립트 파싱과 실행](/image/ETC/BrowerRendering/js-parsing.png)

### 6-1. 자바스크립트 파싱에 의한 HTML 파싱 중단

만약 자바스크립트가 DOM를 조작하는 코드가 있다면 해당 스크립트는 HTML 파싱이 끝난 이후에 실행되어야 한다. 이렇듯 script 태그의 위치는 중요한 의미를 갖는다.

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <script>
      const $apple = document.querySelector('#apple');
      $apple.style.color = 'blue';
    </script>
    <title>Document</title>
  </head>
  <body>
    <div id="apple">사과</div>
  </body>
</html>
```

위와 같은 코드는 `body` 태그를 파싱하기 전 `body` 노드를 가져와 스타일을 변경하고 있기 때문에 오류가 발생한다. 그러므로 body 요소의 가장 아래에 자바스크립트를 위치시키는 것이 일반적으로 좋다.

그 이유는 다음과 같다.

- DOM이 완성되지 않는 상태에서 자바스크립트가 DOM을 조작하면 에러가 발생할 수 있다.
- 자바스크립트 로딩/파싱/실행으로 인해 HTML 요소들의 렌더링에 지장받는 일이 발생하지 않아 페이지 로딩 시간이 단축된다.

---

## 7. Conclusion

> 렌더링되는 과정을 정리해보았다. 흐름을 이해하는 것에는 큰 어려움이 없었지만 CSSOM이 생성되는 과정에서 노드의 부모 자식 관계를 파악할 수 있는 이유에 대해서는 의문점이 생겼다. 또한 DOM 조작을 통해 리플로우와 리페인트가 일어나는 과정을 구체적으로 알고 싶어졌다. 예를들어 `innerText`로 DOM를 조작한 후 `alert` 메서드가 실행이 된다면 DOM이 조작되기 전 `alert` 메서드가 실행이 된다. 이는 오늘 살펴본 리플로우, 리페인트와 관련이 있다고 생각을 하는데 이에 대한 정확한 이유를 알고 싶어졌다.

---

## 참고

도서 - 모던 자바스크립트 Deep Dive  
[렌더링 트리 및 렌더링 프로세스](https://rexiann.github.io/2021/09/11/rendering-process-and-rendering-tree.html)

---

📅 2023-01-13