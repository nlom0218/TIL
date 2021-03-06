# Position

## ๐ ๋ฐ๋ก๊ฐ๊ธฐ

- [1. ๊ฐ์](#1-๊ฐ์)
- [2. static (๊ธฐ๋ณธ์์น)](#2-static-๊ธฐ๋ณธ์์น)
- [3. relative (์๋์์น)](#3-relative-์๋์์น)
- [4. absolute (์ ๋์์น)](#4-absolute-์ ๋์์น)
- [5. fixed (๊ณ ์ ์์น)](#5-fixed-๊ณ ์ ์์น)
- [6. overflow ํ๋กํผํฐ](#6-overflow-ํ๋กํผํฐ)
- [7. Conclusion](#7-conclusion)
- [์ฐธ๊ณ ](#์ฐธ๊ณ )

---

## 1. ๊ฐ์

`position` ํ๋กํผํฐ๋ ์์์ ์์น๋ฅผ ์ ์ํ๋ค. ํด๋น ํ๋กํผํฐ๋ฅผ ์ฌ์ฉํ๋ฉด ์น ๋ฌธ์์์ ์์์ ์์น๋ฅผ ์์ ๋กญ๊ฒ ์ ํ  ์ ์๋ค. `top`, `bottom`, `left`, `right` ํ๋กํผํฐ์ ํจ๊ป ์ฌ์ฉํ์ฌ ์์น๋ฅผ ์ง์ ํ๋ค.

์๋์ ํ๋ `position` ํ๋กํผํฐ๊ฐ์ด๋ค.
|์ข๋ฅ|์ค๋ช|
|:---|:---|
|static|๋ฌธ์์ ํ๋ฆ์ ๋ง์ถฐ ๋ฐฐ์นํ๋ค. ๊ธฐ๋ณธ๊ฐ|
|relative|์์นซ๊ฐ์ ์ง์ ํ  ์ ์๋ค๋ ์ ์ ์ ์ธํ๋ฉด static๊ณผ ๊ฐ๋ค.|
|absolute|relative๊ฐ์ ์ฌ์ฉํ ์์ ์์๋ฅผ ๊ธฐ์ค์ผ๋ก ์์น๋ฅผ ์ง์ ํด ๋ฐฐ์นํ๋ค.|
|fixed|๋ธ๋ผ์ฐ์  ์ฐฝ์ ๊ธฐ์ค์ผ๋ก ์์น๋ฅผ ์ง์ ํด ๋ฐฐ์นํ๋ค.|

---

## 2. static (๊ธฐ๋ณธ์์น)

๊ธฐ๋ณธ์ ์ธ ์์์ ๋ฐฐ์น ์์์ ๋ฐ๋ผ ์์์ ์๋๋ก, ์ผ์ชฝ์์ ์ค๋ฅธ์ชฝ์ผ๋ก ์์์ ๋ฐ๋ผ ๋ฐฐ์น๋๋ฉด ๋ถ๋ชจ ์์ ๋ด์ ์์๋ก์ ์กด์ฌํ  ๋๋ ๋ถ๋ชจ ์์์ ์์น๋ฅผ ๊ธฐ์ค์ผ๋ก ๋ฐฐ์น๋๋ค.

์ขํ ํ๋กํผํฐ(`top`, `bottom`, `left`, `right`)๋ฅผ ๊ฐ์ด ์ฌ์ฉํ  ์ ์์ผ๋ฉฐ ์ฌ์ฉํ  ๊ฒฝ์ฐ ๋ฌด์๋๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      .parent {
        width: 150px;
        height: 150px;
      }
      .children {
        background-color: bisque;
        height: 100%;
      }
    </style>
  </head>
  <body>
    <div class="parent">
      <div class="children">Static</div>
    </div>
  </body>
</html>
```

![static](../image/CSS/Position/static.png)

---

## 3. relative (์๋์์น)

๊ธฐ๋ณธ ์์น๋ฅผ ๊ธฐ์ค์ผ๋ก ์ขํ ํ๋กํผํฐ(`top`, `bottom`, `left`, `right`)๋ฅผ ์ฌ์ฉํ์ฌ ์์น๋ฅผ ์ด๋์ํฌ ์ ์๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      .parent {
        width: 150px;
        height: 150px;
        background-color: rgb(78, 78, 78);
      }
      .child {
        position: relative;
        background-color: bisque;
        height: 100%;
        top: 20px;
        left: 20px;
      }
    </style>
  </head>
  <body>
    <div class="parent">
      <div class="child">relative</div>
    </div>
  </body>
</html>
```

![relative](../image/CSS/Position/relative.png)

---

## 4. absolute (์ ๋์์น)

๋ถ๋ชจ ์์ ๋๋ ๊ฐ์ฅ ๊ฐ๊น์ด ์๋ ์กฐ์ ์์(static ์ ์ธ)๋ฅผ ๊ธฐ์ค์ผ๋ก ์ขํ ํ๋กํผํฐ(`top`, `bottom`, `left`, `right`)๋งํผ ์ด๋ํ๋ค. ์ฆ, relative, absolute, fiexed ํ๋กํผํฐ๊ฐ ์ ์ธ๋์ด ์๋ ๋ถ๋ชจ ๋๋ ์กฐ์ ์์๋ฅผ ๊ธฐ์ค์ผ๋ก ์์น๊ฐ ๊ฒฐ์ ๋๋ค.

๋ง์ผ ๋ถ๋ชจ ๋๋ ์กฐ์ ์์๊ฐ staitc์ธ ๊ฒฝ์ฐ, document body๋ฅผ ๊ธฐ์ค์ผ๋ก ํ์ฌ ์ขํ ํ๋กํฐํฐ๋๋ก ์์นํ๊ฒ ๋๋ค.

absolute ํ๋กํผํฐ ์ ์ธ ์, block ๋ ๋ฒจ ์์์ width๋ inline ์์์ ๊ฐ์ด content์ ๋ง๊ฒ ๋ณํ๋๋๋ก ์ ์ ํ width๋ฅผ ์ง์ ํ์ฌ์ผ ํ๋ค.

relative ํ๋กํผํฐ๋ ๊ธฐ๋ณธ ์์น๋ฅผ ๊ธฐ์ค์ผ๋ก ์ขํ ํ๋กํผํฐ(`top`, `bottom`, `left`, `right`)๋ฅผ ์ฌ์ฉํ์ฌ ์์น๋ฅผ ์ด๋์ํจ๋ค. ํ์ง๋ง absoulte ํ๋กํฐํผ ์์๋ ๋ถ๋ชจ ์์์ ์์ญ์ ๋ฒ์ด๋ ์์ ๋กญ๊ฒ ์ด๋๋ ์ง ์์นํ  ์ ์๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      body {
        display: flex;
      }
      .parent {
        width: 150px;
        height: 150px;
        margin: 50px;
        background-color: rgb(78, 78, 78);
      }
      .children-relative {
        position: relative;
        background-color: bisque;
        height: 100%;
        top: 20px;
        left: 20px;
      }
      .children-absolute {
        position: absolute;
        background-color: bisque;
        width: 150px;
        height: 150px;
        top: 20px;
        left: 20px;
      }
    </style>
  </head>
  <body>
    <div class="parent">
      <div class="children-absolute">absolute</div>
    </div>
    <div class="parent">
      <div class="children-relative">relative</div>
    </div>
  </body>
</html>
```

![asoulte](../image/CSS/Position/absolute.png)

---

## 5. fixed (๊ณ ์ ์์น)

๋ถ๋ชจ ์์์ ๊ด๊ณ์์ด ๋ธ๋ผ์ฐ์ ์ viewport๋ฅผ ๊ธฐ์ค์ผ๋ก ์ขํ ํ๋กํผํฐ(`top`, `bottom`, `left`, `right`)๋ฅผ ์ฌ์ฉํ์ฌ ์์น๋ฅผ ์ด๋์ํจ๋ค.

์คํฌ๋กค์ด ๋๋๋ผ๋ ํ๋ฉด์์ ์ฌ๋ผ์ง์ง ์๊ณ  ํญ์ ๊ฐ์ ๊ณณ์ ์์นํ๋ค.

fixed ํ๋กํผํฐ ์ ์ธ ์, block ๋ ๋ฒจ ์์์ width๋ inline ์์์ ๊ฐ์ด content์ ๋ง๊ฒ ๋ณํ๋๋๋ก ์ ์ ํ width๋ฅผ ์ง์ ํ์ฌ์ผ ํ๋ค.

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      div {
        background-color: rgb(137, 137, 137);
        color: white;
        padding: 10px;
        box-sizing: border-box;
      }
      .nav {
        position: fixed;
        width: 100%;
        top: 0;
        right: 0;
      }
      .side{
        position: fixed;
        height: 100%;
        top: 0;
        right: 0;
      }
    </style>
  </head>
  <body>
    <div class="nav">fixed nav-bar</div>
    <div class="side">fixed side-bar</div>
</html>
```

![fixed](../image/CSS/Position/fixed.png)

---

## 6. overflow ํ๋กํผํฐ

overflow ํ๋กํผํฐ๋ ์์ ์์๊ฐ ๋ถ๋ชจ ์์์ ์์ญ์ ๋ฒ์ด๋ฌ์ ๋ ์ฒ๋ฆฌ ๋ฐฉ๋ฒ์ ์ ์ํ๋ค.

์๋๋ overflow ํ๋กํผํฐ ๊ฐ์ ๋ํ ์ค๋ช์ด๋ค.

| ํ๋กํผํฐ ๊ฐ | ์ค๋ช                                                                                           |
| :---------- | :--------------------------------------------------------------------------------------------- |
| visible     | ์์ญ์ ๋ฒ์ด๋ ๋ถ๋ถ์ ํ์ํ๋ค. ๊ธฐ๋ณธ๊ฐ                                                          |
| hidden      | ์์ญ์ ๋ฒ์ด๋ ๋ถ๋ถ์ ์๋ผ๋ด์ด ๋ณด์ด์ง ์๊ฒ ํ๋ค.                                                |
| scroll      | ์์ญ์ ๋ฒ์ด๋ ๋ถ๋ถ์ด ์์ด๋ ์คํฌ๋กค ํ์ํ๋ค.(ํ์ฌ ๋๋ถ๋ถ ๋ธ๋ผ์ฐ์ ๋ auto์ ๋์ผํ๊ฒ ์๋ํ๋ค.) |
| auto        | ์์ญ์ ๋ฒ์ด๋ ๋ถ๋ถ์ด ์์๋๋ง ์คํฌ๋กค ํ์ํ๋ค.                                                 |

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      body {
        display: flex;
      }
      div {
        background-color: rgb(187, 187, 187);
        padding: 10px;
        box-sizing: border-box;
        width: 200px;
        height: 200px;
        margin-right: 20px;
      }
      .visible {
        overflow: visible;
      }
      .hidden {
        overflow: hidden;
      }
      .scroll {
        overflow: scroll;
      }
      .auto {
        overflow: auto;
      }
    </style>
  </head>
  <body>
    <div class="visible">
      <h3>Visibel</h3>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Beatae veniam quisquam tenetur architecto amet possimus omnis ullam error, eius dolores aliquid? Eaque recusandae esse quidem. Provident labore id cupiditate ratione.</p>
    </div>
    <div class="hidden">
      <h3>Hidden</h3>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Beatae veniam quisquam tenetur architecto amet possimus omnis ullam error, eius dolores aliquid? Eaque recusandae esse quidem. Provident labore id cupiditate ratione.</p>
    </div>
    <div class="scroll">
      <h3>Scroll</h3>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Beatae veniam quisquam tenetur architecto amet possimus omnis ullam error, eius dolores aliquid? Eaque recusandae esse quidem. Provident labore id cupiditate ratione.</p>
    </div>
    <div class="auto">
      <h3>Auto</h3>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Beatae veniam quisquam tenetur architecto amet possimus omnis ullam error, eius dolores aliquid? Eaque recusandae esse quidem. Provident labore id cupiditate ratione.</p>
    </div>
</html>
```

![overflow](../image/CSS/Position/overflow.png)

---

## 7. Conclusion

> `position: relative;`๋ฅผ ์ง๊ธ๊น์ง๋ ๋จ์ง `absolute`๊ฐ์ ์ค์ ํ๊ธฐ ์ํด ๋ถ๋ชจ์์ ์ค ์ํ๋ ์์์ ์์ฑํด์ผํ๋ ํ๋กํผํฐ๋ก ์๊ณ  ์์๋ค. ํ์ง๋ง `relative`๋ง์ ํน์ง์ด ์์๊ณ  ๊ทธ ์ฐ์์๊ฐ `absolute`์ ๊ต์ฅํ ๋น์ทํ๋ค๋ ๊ฒ์ ์์๋ค. ๋จ์ง ๋ฐ๋ก ์์ ๋ถ๋ชจ ์์์ธ์ง ์๋์ง์ ์ฐจ์ด๋ง ์๋ค. ํ์ง๋ง ์ง๊ธ๊น์ง ๋จ๋์ผ๋ก `relative`๋ฅผ ์ฌ์ฉํ์ง ์์๋ ๊ฒ์ ์๊ฐํ๋ฉด ํด๋น ํ๋กํผํฐ ๋์ ํ์ฌ `padding`์ ์ฌ์ฉํ  ์ ์์ด์ ๊ทธ๋ฐ ๊ฒ์ด์ง ์์๊น ์ถ๋ค.  
> ๊ทธ๋ฆฌ๊ณ  ์ง๊ธ๊น์ง ์ปจํ์ธ  ๋ด์ scroll๋ฅผ ์ฌ์ฉํ๊ธฐ ์ํด์๋ `absolute`๋ฅผ ์ ๊ทน์ ์ผ๋ก ์ฌ์ฉํ๋๋ฐ ๊ตณ์ด ๊ทธ๋ฌ์ง ์๊ณ  `relative`๋ฅผ ์ฌ์ฉํด๋ ๋  ๋ฏ ํ๋ค~

---

## ์ฐธ๊ณ 

[poiemaweb 2-8 ์์์ ์์น ์ ์](https://poiemaweb.com/css3-position)  
๋์ - HTML + CSS + ์๋ฐ์คํฌ๋ฆฝํธ ์น ํ์ค์ ์ ์

---

[๐](#position)
