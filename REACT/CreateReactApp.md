# Create React App

## ๐ ๋ฐ๋ก๊ฐ๊ธฐ

- [1. ๊ฐ์](#1-๊ฐ์)
- [2. ์ค๋น์ฌํญ](#2-์ค๋น์ฌํญ)
- [3. Create React App์ด๋?](#3-create-react-app์ด๋)
- [4. create react app](#4-create-react-app)
- [5. create react app with typescript](#5-create-react-app-with-typescript)
- [6. error](#6-error)
- [์ฐธ๊ณ ](#์ฐธ๊ณ )

---

## 1. ๊ฐ์

React ํ๋ก์ ํธ๋ฅผ ์ฝ๊ณ  ๋น ๋ฅด๊ฒ ๋ง๋ค ์ ์๋๋ก ๋์์ฃผ๋ Create React App์ ๋ํด ์์๋ณด์.

---

## 2. ์ค๋น์ฌํญ

create react app๋ฅผ ์ฌ์ฉํ๊ธฐ ์ํด์๋ ์๋์ ๊ฐ์ ์ค๋น์ฌํญ์ด ํ์ํ๋ค.

1. 14.0.0 ํน์ ์์ ๋ฒ์ ์ node

   > `$ node -v` ๋ช๋ น์ด๋ฅผ ํตํด node์ ๋ฒ์ ์ ์ ์ ์๋ค.  
   > ![node -v](../image/React/CreateReactApp/setup1.png)

2. 5.6 ํน์ ์์ ๋ฒ์ ์ npm
   > `$ npm -v` ๋ช๋ น์ด๋ฅผ ํตํด npm์ ๋ฒ์ ์ ์ ์ ์๋ค.  
   > ![npm -v](../image/React/CreateReactApp/setup2.png)

์์ง node๋ฅผ ์ค์นํ์ง ์์๋ค๋ฉด ์๋์ ๋งํฌ๋ฅผ ํตํด ์ค์นํ  ์ ์๋ค. npm์ node๋ฅผ ์ค์นํ๋ฉด ํจ๊ป ์ค์น๋๋ค.  
[download nodejs](https://nodejs.org/ko/)

---

## 3. Create React App์ด๋?

React๋ก ๊ฐ๋ฐํ๊ธฐ ์ํด์๋ ์นํฉ(Webpack), ๋ฐ๋ฒจ(Babel)๋ฑ ๋ฏธ๋ฆฌ ๋ฐฐ์์ผํ๊ณ  ์ค์ ํด์ผ ํ  ๊ธฐ์ ๋ค์ด ์๋ค. ์ด๋ฌํ ๊ด๋ จ ๊ธฐ์ ์ ์ ๊ฒฝ์ฐ์ง ์๊ณ  React App์ ๋ฐ๋ก ๊ฐ๋ฐํ  ์ ์๋ ๋ฐฉ๋ฒ์ด Create React App์ด๋ผ๋ CLI(์ปค์บ๋ ๋ผ์ธ ์ธํฐํ์ด์ค)๋๊ตฌ ์ด๋ค. ํด๋น ๋๊ตฌ๋ฅผ ์ฌ์ฉํ๋ฉด ๊ณจ์น ์ํ ๋ฌธ์ ์์ด ์์ฃผ ๊ฐ๋จํ๊ฒ Reactํ๋ก์ ํธ๋ฅผ ๊ตฌ์ฑํ  ์ ์๋ค.

---

## 4. create react app

ํฐ๋ฏธ๋์ ์ด๊ณ  ์๋์ ๊ฐ์ด ๋ช๋ น๋ฌ๋ฅผ ์๋ ฅํ์ฌ React ํ๋ก์ ํธ๋ฅผ ์์ฑํ๋ค.

> $ npx create-react-app my-app

---

## 5. create react app with typescript

React ํ๋ก์ ํธ์ typescript๋ฅผ ํฌํจ์ํค๊ณ  ์ถ๋ค๋ฉด ์๋์ ๊ฐ์ ๋ช๋ น์ด๋ฅผ ์๋ ฅํด์ผ ํ๋ค.

> $ npx create-react-app my-app --template typescript

---

## 6. error

`create react app`๋ช๋ น์ด๋ฅผ ์คํํ๋ค ๋ณด๋ฉด ์๋์ ๊ฐ์ ์๋ฌ๊ฐ ๋ํ๋ React ํ๋ก์ ํธ๋ฅผ ์์ฑํ  ์ ์๋ ๊ฒฝ์ฐ๊ฐ ์๋ค.

![create react app error](../image/React/CreateReactApp/createreactapperror.png)

- ํด๊ฒฐ๋ฐฉ๋ฒ
  - ์ฒซ๋ฒ์งธ ๋ฐฉ๋ฒ
    1.  $ npm uninstall -g create-react-app
    2.  $ npm add create-react-app
    3.  $ npx create-react-app myapp
  - ๋๋ฒ์งธ ๋ฐฉ๋ฒ: $ npx create-react-app@latest myapp

---

## ์ฐธ๊ณ 

[Create React App: ์์ฝ๊ฒ ํ๋ก์ ํธ ๋ง๋ค๊ธฐ](https://www.daleseo.com/create-react-app/)  
[React ์ค๋ฅ ํด๊ฒฐ You are running `create-react-app` 4.0.2, which is behind the latest release (4.0.3).](https://velog.io/@milkyway/React-%EC%98%A4%EB%A5%98-%ED%95%B4%EA%B2%B0-You-are-running-create-react-app-4.0.2-which-is-behind-the-latest-release-4.0.3)  
[[์๋ฌํด๊ฒฐ] You are running `create-react-app` 4.0.3, which](https://sezzled.tistory.com/108)  
[Create React App Getting Started](https://create-react-app.dev/docs/getting-started)  
[์๋ก์ด React ์ฑ ๋ง๋ค๊ธฐ](https://ko.reactjs.org/docs/create-a-new-react-app.html)

---

[๐](#create-react-app)
