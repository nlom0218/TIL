# GlobalCSS

## 👉 바로가기

- [1 개요](#1-개요)
- [2 global.css](#2-globalcss)
- [3 @import](#3-import)
- [4 Reset css](#4-reset-css)
- [5 번거로운 점](#5-번거로운-점)
- [참고](#참고)

---

## 1 개요

> 나는 스타일 코드의 중복을 피하기 위해 ReactJS로 개발을 하는 경우 [styled-components](https://styled-components.com/)를 사용하여 필요한 스타일을 컴포넌트를 만들고 만든 스타일 컴포넌트를 필요한 곳에 import하여 사용하였다.

ReactJS, VueJS가 아닌 Html, Css, Javasrcipt로만 웹 개발을 하게 되는 경우 스타일링을 위해 `*.css`을 만들고 [Link style](https://github.com/nlom0218/TIL/blob/main/CSS/StyleSheet.md#3-linking-style-sheet%EC%99%B8%EB%B6%80-%EC%8A%A4%ED%83%80%EC%9D%BC-%EC%8B%9C%ED%8A%B8-link-style)방법을 통해 `*.html`과 `*.css`를 연결한다.

내가 꼽은 위와 같은 방법의 단점은 아래와 같다.

- 단점

  1. 각`*.html`파일에 모두 다른 `*.css`을 연결하는 경우 같은 스타일의 코드를 중복하여 작성해야 한다.  
      예를들어 같은 스타일의 버튼이 있고 이를 `index.html`과 `profile.html`에서 필요하다면 `index.css`와 `profile.css`에 같은 스타일의 코드를 작성해야 한다.  
      이러한 방법은 버튼 스타일을 수정을 해야하는 경우 해당 스타일의 코드가 사용된 모든 `*.css`를 찾아 하나하나 수정해야 한다.

     ```html
     <!-- index.html -->
     ...
     <head>
       <link rel="stylesheet" href="index.css" />
     </head>
     ...
     ```

     ```html
     <!-- profile.html -->
     ...
     <head>
       <link rel="stylesheet" href="profile.css" />
     </head>
     ...
     ```

     ```css
     /* index.css */
     .button {
       color: red;
     }
     ```

     ```css
     /* profile.css */
     .button {
       color: red;
     }
     ```

  2. `button.css`, `container.css`, `navbar.css` 등 각자 맡고 있는 역할의 `*.css`을 만들면 위의 단점은 해소가 되나 각`*.html`에서 작성해야 하는 `<link />`태그가 많이진다.  
      또한 새로운 스타일의 무언가를 추가하게 된다면 해당 스타일을 사용하는 `*.html`을 모두 찾아 새로운 `<link />`태그를 작성해야 한다.

     ```css
     /* button.css */
     div {
       color: red;
       ...;
     }
     ```

     ```css
     /* container.css */
     div {
       color: red;
       ...;
     }
     ```

     ```css
     /* navbar.css */
     div {
       color: red;
       ...;
     }
     ```

     ```html
     <!-- index.html -->
     ...
     <head>
       <link rel="stylesheet" href="button.css" />
       <link rel="stylesheet" href="container.css" />
       <link rel="stylesheet" href="navbar.css" />
     </head>
     ...
     ```

     ```html
     <!-- profile.html -->
     ...
     <head>
       <link rel="stylesheet" href="button.css" />
       <link rel="stylesheet" href="container.css" />
       <link rel="stylesheet" href="navbar.css" />
     </head>
     ...
     ```

> 이러한 단점을 해결하고자 모든 `*.html`에 연결하는 예를들면 `global.css` 만들고 여기에 모든 `*.css`을 `@import`하는 방법에 대해 소개한다.

---

## 2 global.css

`.css`파일 이름은 styles, global 등 원하는 이름으로 작성해도 되지만 나는 모든 `*.html`에서 사용하기 때문에 전역이라는 의미로 **global**를 사용한다.

> css 폴더를 만들고 `global.css`를 만든다.

```css
/* css/global.css */
body {
  font-size: 16px;
  color: rgba(20, 20, 20, 1);
}
...
```

## 3 @import

`@import` CSS 규칙은 다른 스타일 시트에서 스타일 규칙을 가져올 때 쓰임

사용법

- `@import url;`

`url`: import할 자원의 위치를 나타내는 문자열 또는 url

> 필요한 `*.css`를 만들고 `global.css`에 import한다.

```css
/* button.css */
div {
  color: red;
  ...;
}
```

```css
/* container.css */
div {
  color: red;
  ...;
}
```

```css
/* navbar.css */
div {
  color: red;
  ...;
}
```

```css
/* css/global.css */
@import "button.css";
@import "container.css";
@import "navbar.css";

body {
  font-size: 16px;
  color: rgba(20, 20, 20, 1);
}
...
```

> 이후 모든 `*.html`에 `<link rel="stylesheet" href="css/global.css" />`를 추가한다.  
> `*.html`의 위치에 따른 경로작성은 필수

```html
<!-- index.html -->
...
<head>
  <link rel="stylesheet" href="css/global.css" />
</head>
...
```

---

## 4 Reset css

모든 웹 브라우저는 디폴트 스타일을 가지고 있어 CSS가 없어도 작동함. 하지만 웹 브라우저에 따라 디폴트 스타일이 다르고 지원하는 tag나 style도 제각각이서 주의가 필요

CSS Reset는 모든 태그에 적용된 서식에 margin과 padding을 제거하는 방식으로 시작되어 **브라우저의 기본요소의 디자인을 모두 없애자는 것**으로 발전, 이렇게 **모두 0으로 만드는 방법**을 통해서 브라우저들의 서식을 하나로 통일

가장 유명한 것은 [**에릭마이어의 CSS Reset**](https://meyerweb.com/eric/tools/css/reset/)

> Reset.css 적용하기

```css
/* reset.css */
html,
body,
div,
span,
applet,
object,
iframe,
h1,
h2,
h3,
h4,
h5,
h6,
p,
blockquote,
pre,
a,
abbr,
acronym,
address,
big,
cite,
code,
del,
dfn,
em,
img,
ins,
kbd,
q,
s,
samp,
small,
strike,
strong,
sub,
sup,
tt,
var,
b,
u,
i,
center,
dl,
dt,
dd,
ol,
ul,
li,
fieldset,
form,
label,
legend,
table,
caption,
tbody,
tfoot,
thead,
tr,
th,
td,
article,
aside,
canvas,
details,
embed,
figure,
figcaption,
footer,
header,
hgroup,
menu,
nav,
output,
ruby,
section,
summary,
time,
mark,
audio,
video {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  font: inherit;
  vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section {
  display: block;
}
body {
  line-height: 1;
}
ol,
ul {
  list-style: none;
}
blockquote,
q {
  quotes: none;
}
blockquote:before,
blockquote:after,
q:before,
q:after {
  content: "";
  content: none;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}
```

```css
/* css/global.css */
@import "button.css";
@import "container.css";
@import "navbar.css";
@import "reset.css";

body {
  font-size: 16px;
  color: rgba(20, 20, 20, 1);
}
...
```

> css reset의 정답은 없다고 한다. 내가 만들 사이트에 필요한 reset만 적용해서 만들어도 되고 위와 같이 `reset.css`를 import한 뒤 추가로 reset이 필요한 스타일을 `global.css`에 작성해도 된다.

---

## 5 번거로운 점

`global.css`를 사용해서 번거로운 점이라기 보다는 html, css, javascript로만 이용해서 웹을 만들 때의 번거로운 점은 만약 제대로 된 기획, 디자인 없이 바로 코드를 작성하면서 웹을 만들다 보면 중간중간에 class, id를 수정, 추가하여 작성하는 경우가 간혹 생긴다.  
기획, 디자인이 없더라도 `*.html`을 작성할 때 미리 디자인 까지 생각하며 class, id를 부여한다면 어느정도는 괜찮을지 몰라도 class, id를 나중에 디자인을 할 때 만들게 된다면 모든 `*.html`를 다시 열어서 일일히 class, id를 부여해야하는 번거로움이 생긴다.

> 물론 ReactJS로 개발을 하는 경우에도 사전 기획, 디자인이 없다면 코드와 컴포넌트는 난장판이 된다. 이건 경험담...  
> 피그마로 기획 및 디자인하는 공부, 연습도 하자😀

---

## 참고

[poiemaweb 2-1 CSS 기본문법](https://poiemaweb.com/css3-syntax)  
[mdn @import](https://developer.mozilla.org/ko/docs/Web/CSS/@import)  
[2022 CSS Reset 다시 써보기!](https://velog.io/@teo/2022-CSS-Reset-%EB%8B%A4%EC%8B%9C-%EC%8D%A8%EB%B3%B4%EA%B8%B0)

---

[👆](#globalcss)
