# GlobalCSS

## ๐ ๋ฐ๋ก๊ฐ๊ธฐ

- [1. ๊ฐ์](#1-๊ฐ์)
- [2. global.css](#2-globalcss)
- [3. @import](#3-import)
- [4. Reset css](#4-reset-css)
- [5. ๋ฒ๊ฑฐ๋ก์ด ์ ](#5-๋ฒ๊ฑฐ๋ก์ด-์ )
- [์ฐธ๊ณ ](#์ฐธ๊ณ )

---

## 1. ๊ฐ์

> ๋๋ ์คํ์ผ ์ฝ๋์ ์ค๋ณต์ ํผํ๊ธฐ ์ํด ReactJS๋ก ๊ฐ๋ฐ์ ํ๋ ๊ฒฝ์ฐ [styled-components](https://styled-components.com/)๋ฅผ ์ฌ์ฉํ์ฌ ํ์ํ ์คํ์ผ์ ์ปดํฌ๋ํธ๋ฅผ ๋ง๋ค๊ณ  ๋ง๋  ์คํ์ผ ์ปดํฌ๋ํธ๋ฅผ ํ์ํ ๊ณณ์ importํ์ฌ ์ฌ์ฉํ์๋ค.

ReactJS, VueJS๊ฐ ์๋ Html, Css, Javasrcipt๋ก๋ง ์น ๊ฐ๋ฐ์ ํ๊ฒ ๋๋ ๊ฒฝ์ฐ ์คํ์ผ๋ง์ ์ํด `*.css(์คํ์ผ์ํธ)`์ ๋ง๋ค๊ณ  [Link style](https://nlom0218.github.io/TIL/CSS/StyleSheet.html#3-linking-style-sheet%EC%99%B8%EB%B6%80-%EC%8A%A4%ED%83%80%EC%9D%BC-%EC%8B%9C%ED%8A%B8-link-style)๋ฐฉ๋ฒ์ ํตํด `*.html`๊ณผ `*.css(์คํ์ผ์ํธ)`๋ฅผ ์ฐ๊ฒฐํ๋ค.

๋ด๊ฐ ์๊ฐํ๋ ์์ ๊ฐ์ ๋ฐฉ๋ฒ์ ๋จ์ ์ ์๋์ ๊ฐ๋ค.

- ๋จ์ 

  1. ๊ฐ`*.html`ํ์ผ์ ๋ชจ๋ ๋ค๋ฅธ `*.css(์คํ์ผ์ํธ)`์ ์ฐ๊ฒฐํ๋ ๊ฒฝ์ฐ ๊ฐ์ ์คํ์ผ์ ์ฝ๋๋ฅผ ์ค๋ณตํ์ฌ ์์ฑํด์ผ ํ๋ ๊ฒฝ์ฐ๊ฐ ์๊ธธ ์ ์๋ค.  
      ์๋ฅผ๋ค์ด ๊ฐ์ ์คํ์ผ์ ๋ฒํผ์ด ์๊ณ  ์ด๋ฅผ `index.html`๊ณผ `profile.html`์์ ํ์ํ๋ค๋ฉด `index.css`์ `profile.css`์ ๊ฐ์ ์คํ์ผ์ ์ฝ๋๋ฅผ ์์ฑํด์ผ ํ๋ค.  
      ์ด๋ฌํ ๋ฐฉ๋ฒ์ ๋ฒํผ ์คํ์ผ์ ์์ ์ ํด์ผํ๋ ๊ฒฝ์ฐ ํด๋น ์คํ์ผ์ ์ฝ๋๊ฐ ์ฌ์ฉ๋ ๋ชจ๋  `*.css(์คํ์ผ์ํธ)`๋ฅผ ์ฐพ์ ํ๋ํ๋ ์์ ํด์ผ ํ๋ค.

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

  2. `button.css`, `container.css`, `navbar.css` ๋ฑ ๊ฐ์ ๋งก๊ณ  ์๋ ์ญํ ์ `*.css(์คํ์ผ์ํธ)`์ ๋ง๋ค๋ฉด ์์ ๋จ์ ์ ํด์๊ฐ ๋๋ ๊ฐ`*.html`์์ ์์ฑํด์ผ ํ๋ `<link />`ํ๊ทธ๊ฐ ๋ง์ด์ง๋ค.  
      ๋ํ ์๋ก์ด ์คํ์ผ์ ๋ฌด์ธ๊ฐ๋ฅผ ์ถ๊ฐํ๊ฒ ๋๋ค๋ฉด ํด๋น ์คํ์ผ์ ์ฌ์ฉํ๋ `*.html`์ ๋ชจ๋ ์ฐพ์ ์๋ก์ด `<link />`ํ๊ทธ๋ฅผ ์์ฑํด์ผ ํ๋ค.

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

> ์ด๋ฌํ ๋จ์ ์ ํด๊ฒฐํ๊ณ ์ ๋ชจ๋  `*.html`์ ์ฐ๊ฒฐํ๋ ์๋ฅผ๋ค๋ฉด `global.css` ๋ง๋ค๊ณ  ์ฌ๊ธฐ์ ๋ชจ๋  `*.css(์คํ์ผ์ํธ)`์ `@import`ํ๋ ๋ฐฉ๋ฒ์ ๋ํด ์๊ฐํ๋ค.

---

## 2. global.css

`.css`ํ์ผ ์ด๋ฆ์ styles, global ๋ฑ ์ํ๋ ์ด๋ฆ์ผ๋ก ์์ฑํด๋ ๋์ง๋ง ๋๋ ๋ชจ๋  `*.html`์์ ์ฌ์ฉํ๊ธฐ ๋๋ฌธ์ ์ ์ญ์ด๋ผ๋ ์๋ฏธ๋ก **global**๋ฅผ ์ฌ์ฉํ๋ค.

> css ํด๋๋ฅผ ๋ง๋ค๊ณ  `global.css`๋ฅผ ๋ง๋ ๋ค.

```css
/* css/global.css */
body {
  font-size: 16px;
  color: rgba(20, 20, 20, 1);
}
...
```

---

## 3. @import

`@import` CSS ๊ท์น์ ๋ค๋ฅธ ์คํ์ผ ์ํธ์์ ์คํ์ผ ๊ท์น์ ๊ฐ์ ธ์ฌ ๋ ์ฐ์

์ฌ์ฉ๋ฒ

- `@import url;`

`url`: importํ  ์์์ ์์น๋ฅผ ๋ํ๋ด๋ ๋ฌธ์์ด ๋๋ url

> ํ์ํ `*.css(์คํ์ผ์ํธ)`๋ฅผ ๋ง๋ค๊ณ  `global.css`์ importํ๋ค.

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

> ์ดํ ๋ชจ๋  `*.html`์ `<link rel="stylesheet" href="css/global.css" />`๋ฅผ ์ถ๊ฐํ๋ค.  
> `*.html`์ ์์น์ ๋ฐ๋ฅธ ๊ฒฝ๋ก์์ฑ์ ํ์

```html
<!-- index.html -->
...
<head>
  <link rel="stylesheet" href="css/global.css" />
</head>
...
```

---

## 4. Reset css

๋ชจ๋  ์น ๋ธ๋ผ์ฐ์ ๋ ๋ํดํธ ์คํ์ผ์ ๊ฐ์ง๊ณ  ์์ด CSS๊ฐ ์์ด๋ ์๋ํจ. ํ์ง๋ง ์น ๋ธ๋ผ์ฐ์ ์ ๋ฐ๋ผ ๋ํดํธ ์คํ์ผ์ด ๋ค๋ฅด๊ณ  ์ง์ํ๋ tag๋ style๋ ์ ๊ฐ๊ฐ์ด์ ์ฃผ์๊ฐ ํ์

CSS Reset๋ ๋ชจ๋  ํ๊ทธ์ ์ ์ฉ๋ ์์์ margin๊ณผ padding์ ์ ๊ฑฐํ๋ ๋ฐฉ์์ผ๋ก ์์๋์ด **๋ธ๋ผ์ฐ์ ์ ๊ธฐ๋ณธ์์์ ๋์์ธ์ ๋ชจ๋ ์์ ์๋ ๊ฒ**์ผ๋ก ๋ฐ์ , ์ด๋ ๊ฒ **๋ชจ๋ 0์ผ๋ก ๋ง๋๋ ๋ฐฉ๋ฒ**์ ํตํด์ ๋ธ๋ผ์ฐ์ ๋ค์ ์์์ ํ๋๋ก ํต์ผ

๊ฐ์ฅ ์ ๋ชํ ๊ฒ์ [**์๋ฆญ๋ง์ด์ด์ CSS Reset**](https://meyerweb.com/eric/tools/css/reset/)

> Reset.css ์ ์ฉํ๊ธฐ

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

> css reset์ ์ ๋ต์ ์๋ค๊ณ  ํ๋ค. ๋ด๊ฐ ๋ง๋ค ์ฌ์ดํธ์ ํ์ํ reset๋ง ์ ์ฉํด์ ๋ง๋ค์ด๋ ๋๊ณ  ์์ ๊ฐ์ด `reset.css`๋ฅผ importํ ๋ค ์ถ๊ฐ๋ก reset์ด ํ์ํ ์คํ์ผ์ `global.css`์ ์์ฑํด๋ ๋๋ค.

---

## 5. ๋ฒ๊ฑฐ๋ก์ด ์ 

`global.css`๋ฅผ ์ฌ์ฉํด์ ๋ฒ๊ฑฐ๋ก์ด ์ ์ด๋ผ๊ธฐ ๋ณด๋ค๋ html, css, javascript๋ก๋ง ์ด์ฉํด์ ์น์ ๋ง๋ค ๋์ ๋ฒ๊ฑฐ๋ก์ด ์ ์ ๋ง์ฝ ์ ๋๋ก ๋ ๊ธฐํ, ๋์์ธ ์์ด ๋ฐ๋ก ์ฝ๋๋ฅผ ์์ฑํ๋ฉด์ ์น์ ๋ง๋ค๋ค ๋ณด๋ฉด ์ค๊ฐ์ค๊ฐ์ class, id๋ฅผ ์์ , ์ถ๊ฐํ์ฌ ์์ฑํ๋ ๊ฒฝ์ฐ๊ฐ ๊ฐํน ์๊ธด๋ค.  
๊ธฐํ, ๋์์ธ์ด ์๋๋ผ๋ `*.html`์ ์์ฑํ  ๋ ๋ฏธ๋ฆฌ ๋์์ธ ๊น์ง ์๊ฐํ๋ฉฐ class, id๋ฅผ ๋ถ์ฌํ๋ค๋ฉด ์ด๋์ ๋๋ ๊ด์ฐฎ์์ง ๋ชฐ๋ผ๋ class, id๋ฅผ ๋์ค์ ๋์์ธ์ ํ  ๋ ๋ง๋ค๊ฒ ๋๋ค๋ฉด ๋ชจ๋  `*.html`๋ฅผ ๋ค์ ์ด์ด์ ์ผ์ผํ class, id๋ฅผ ๋ถ์ฌํด์ผํ๋ ๋ฒ๊ฑฐ๋ก์์ด ์๊ธด๋ค.

> ๋ฌผ๋ก  ReactJS๋ก ๊ฐ๋ฐ์ ํ๋ ๊ฒฝ์ฐ์๋ ์ฌ์  ๊ธฐํ, ๋์์ธ์ด ์๋ค๋ฉด ์ฝ๋์ ์ปดํฌ๋ํธ๋ ๋์ฅํ์ด ๋๋ค. ์ด๊ฑด ๊ฒฝํ๋ด...  
> ํผ๊ทธ๋ง๋ก ๊ธฐํ ๋ฐ ๋์์ธํ๋ ๊ณต๋ถ, ์ฐ์ต๋ ํ์๐

---

## ์ฐธ๊ณ 

[poiemaweb 2-1 CSS ๊ธฐ๋ณธ๋ฌธ๋ฒ](https://poiemaweb.com/css3-syntax)  
[mdn @import](https://developer.mozilla.org/ko/docs/Web/CSS/@import)  
[2022 CSS Reset ๋ค์ ์จ๋ณด๊ธฐ!](https://velog.io/@teo/2022-CSS-Reset-%EB%8B%A4%EC%8B%9C-%EC%8D%A8%EB%B3%B4%EA%B8%B0)

---

[๐](#globalcss)
