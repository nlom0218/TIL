# Typescript๋?

## ๐ ๋ฐ๋ก๊ฐ๊ธฐ

- [1. ๊ฐ์](#1-๊ฐ์)
- [2. ํ์์คํฌ๋ฆฝํธ(typescript)๋?](#2-ํ์์คํฌ๋ฆฝํธtypescript๋)
- [3. ํ์์คํฌ๋ฆฝํธ์ ํน์ง](#3-ํ์์คํฌ๋ฆฝํธ์-ํน์ง)
- [4. Javascript vs Typescript](#4-javascript-vs-typescript)
- [5. Conclusion](#5-conclusion)
- [์ฐธ๊ณ ](#์ฐธ๊ณ )

---

## 1. ๊ฐ์

๋ง์ ๊ธฐ์๋ค์์ typescirpt์ ๋ํ ์ญ๋์ ๊ฐ์ง ๊ฐ๋ฐ์๋ค์ ํ์๋กํ๊ณ  ์๋ค. javascript๊ณผ ์ด๋ฆ์ด ๋น์ทํ typescript๋ ๋ฌด์์ด๋ฉฐ? javascript์๋ ์ด๋ค ์ฐจ์ด์ ์ด ์๋์ง ์์๋ณด์. ๋ํ typescript๊ฐ ๊ฐ์ง๊ณ  ์๋ ํน์ง๋ ํจ๊ป ์ดํด๋ณด์.

---

## 2. ํ์์คํฌ๋ฆฝํธ(typescript)๋?

ํ์์คํฌ๋ฆฝํธ๋ ๋ง์ดํฌ๋ก์ํํธ์์ ๊ฐ๋ฐํ ์คํ์์ค ํ๋ก๊ทธ๋๋ฐ ์ธ์ด์ด๋ฉฐ, ์๋ฐ์คํฌ๋ฆฝํธ์ ๋จ์ ์ ๋ณด์ํ๊ธฐ ์ํด ๋ง๋ค์ด์ก๋ค. ๋ชจ๋  ๋ธ๋ผ์ฐ์ , ํธ์คํธ, ์ด์์ฒด์ ์์ ๋์ํ๋ค.

> TypeScript is JavaScript with syntax for types.  
> TypeScript is a strongly typed programming language that builds on JavaScript, giving you better tooling at any scale.

์๋ ํ์์คํฌ๋ฆฝํธ ๊ณต์ ํํ์ด์ง์์ ์ค๋ชํ๊ณ  ์๋ ํ์์คํฌ๋ฆฝํธ์ ๋ํ ์ค๋ช์ด๋ค. ์ด๋ฅผ ํ ๋๋ก ํ์์คํฌ๋ฆฝํธ๋ฅผ ์ค๋ชํ์๋ฉด ์๋์ ๊ฐ๋ค.

- ์๋ฐ์คํฌ๋ฆฝํธ๋ฅผ ๊ธฐ๋ฐ์ ๋ง๋  ๊ฐ๋ ฅํ ์ธ์ด์ด๋ค.
- ํ์์ ์ ํ๊ธฐ ๋๋ฌธ์ ์ด๋ ํ ํ๊ฒฝ์์๋ ํจ์ฌ ์ข์ ๊ฒฝํ์ ์ ์ฌํ๋ค.
- ํ์์ด ์๋ ๊ฐ๋ ฅํ ์ธ์ด์ด๋ค.

์ฌ๊ธฐ์ ๋งํ๋ ํ์์ด๋ ํ๋ก๊ทธ๋จ์์ ๋ค๋ฃฐ ์ ์๋ ๊ฐ์ ์ข๋ฅ(string, boolean, int ๋ฑ)๋ฅผ ์๋ฏธํ๋ค.

---

## 3. ํ์์คํฌ๋ฆฝํธ์ ํน์ง

์๋๋ ํ์์คํฌ๋ฆฝํธ ๊ณต์ ํํ์ด์ง์ ์๋ ํน์ง์ ๊ฐ์ ธ์จ ๊ฒ์ด๋ค.

1. JavaScript and More
   > TypeScript adds additional syntax to JavaScript to support a tighter integration with your editor. Catch errors early in your editor.
   - ํ์์คํฌ๋ฆฝํธ๋ ์๋ฐ์คํฌ๋ฆฝํธ์ ๋ฌธ๋ฒ์ ์ถ๊ฐํ์ฌ vscode์ ๊ฐ์ ์๋ํฐ์์ ์ค๋ฅ๋ฅผ ์ก๋๋ฐ ๋์์ค๋ค.
   ***
2. A Result Yout Can Trust
   > TypeScript code converts to JavaScript, which runs anywhere JavaScript runs: In a browser, on Node.js or Deno and in your apps.
   - ํ์์คํฌ๋ฆฝํธ๋ ์๋ฐ์คํฌ๋ฆฝํธ๊ฐ ์คํ๋๋ ํ๊ฒฝ(๋ธ๋ผ์ฐ์ , ๋ธ๋JS, Deno)์์ ์๋ฐ์คํฌ๋ฆฝํธ๋ก ๋ณํ๋๋ค.
   ***
3. Safety at Scale
   > TypeScript understands JavaScript and uses type inference to give you great tooling without additional code.
   - ํ์์คํฌ๋ฆฝํธ๋ ์ถ๊ฐ๊ฐ์ ์ธ ์ฝ๋ ์์ฑ ์์ด ํ์์ ์ ํ์ ์ถ๋ก ํ  ์ ์๋ค.

๋ฟ๋ง ์๋๋ผ ์๋์ ๊ฐ์ ๋ค์ํ ํน์ง์ ๊ฐ์ง๊ณ  ์๋ค.

- ์ปดํ์ผ ์ธ์ด, ์ ์  ํ์ ์ธ์ด: ํ์์คํฌ๋ฆฝํธ๋ ์ปดํ์ผ๋ฌ ๋๋ ๋ฐ๋ฒจ(Babel)์ ํตํด ์๋ฐ์คํฌ๋ฆฝํธ ์ฝ๋๋ก ๋ณํ๋๋ค. ์ฝ๋ ์์ฑ ๋จ๊ณ์์ ํ์์ ์ฒดํฌํด ์ค๋ฅ๋ฅผ ํ์ธํ  ์ ์๊ณ  ๋ฏธ๋ฆฌ ํ์์ ๊ฒฐ์ ํ๊ธฐ ๋๋ฌธ์ ์คํ ์๋๊ฐ ๋งค์ฐ ๋น ๋ฅด๋ค. ํ์ง๋ง ์ฝ๋ ์์ฑ ์ ๋งค๋ฒ ํ์์ ๊ฒฐ์ ํด์ผ ํ๊ธฐ ๋๋ฌธ์ ๋ฒ๊ฑฐ๋กญ๊ณ  ์ฝ๋๋์ด ์ฆ๊ฐํ๋ฉด์ ์ปดํ์ผ ์๊ฐ์ด ์ค๋ ๊ฑธ๋ฆฐ๋ค๋ ๋จ์ ์ด ์๋ค.

- ์๋ฐ์คํฌ๋ฆฝํธ ์ํผ์: ํ์์คํฌ๋ฆฝํธ๋ ์๋ฐ์คํฌ๋ฆฝํธ ๊ธฐ๋ณธ ๋ฌธ๋ฒ์ ํ์์คํฌ๋ฆฝํธ์ ๋ฌธ๋ฒ์ ์ถ๊ฐํ ๊ฒ์ด๋ค.

- ๊ฐ์ฒด ์งํฅ ํ๋ก๊ทธ๋๋ฐ ์ง์: ํ์์คํฌ๋ฆฝํธ๋ ES6์์ ์๋กญ๊ฒ ์ฌ์ฉ๋ ๋ฌธ๋ฒ์ ํฌํจํ๊ณ  ์์ผ๋ฉฐ ํด๋์ค, ์ธํฐํ์ด์ค, ์์, ๋ชจ๋ ๋ฑ๊ณผ ๊ฐ์ ๊ฐ์ฒด ์งํฅ ํ๋ก๊ทธ๋๋ฐ ํจํด์ ์ ๊ณตํ๋ค.

- ์ ์ง์  ์ ํ ๊ฐ๋ฅ: ๊ธฐ์กด ์๋ฐ์คํฌ๋ฆฝํธ๋ก ๋ง๋ค์ด์ง ํ๋ก์ ํธ์ ์ถ๊ฐ ๊ธฐ๋ฅ์ด๋ ํน์  ๊ธฐ๋ฅ์๋ง ํ์์คํฌ๋ฆฝํธ๋ฅผ ๋์ํจ์ผ๋ก์จ ํ๋ก์ ํธ๋ฅผ ์ ์ง์ ์ผ๋ก ์ ํํ  ์ ์๋ค.

---

## 4. Javascript vs Typescript

์์ผ๋ก typescript ํํธ์์ ๋ง์ ๋ด์ฉ์ typescript๋ฅผ ๋ค๋ฃจ๊ธฐ ๋๋ฌธ์ ์ฌ๊ธฐ์๋ ํ๋์ ์์๋ฅผ ํตํด ๋ ์ธ์ด์ ์ฐจ์ด์ ์ ์ดํด๋ณด์.

`a`์ `b`๋ผ๋ ์ธ์๋ฅผ ๋ฐ์์ ๋ํ๋ ํจ์์ธ `add function`๋ฅผ ๋ง๋ค์ด๋ณด์.

๋จผ์  ์๋ฐ์คํฌ๋ฆฝํธ์ ์ฝ๋๋ ์๋์ ๊ฐ๋ค.

```js
const add = (a, b) => a + b;
```

์ธ์์ ์ฌ๋ฌ ํ์์ ๋ฃ์ด ํจ์๋ฅผ ์คํํด๋ณด์.

```js
add(1, 2); // 3
add("2", 3); // "23"
add("Hello", "world"); // "Helloworld"
add(null, 3); // 3
add(true, false); // 1
```

์์ ๋ณด์ด๋ ๊ฒ๊ณผ ๊ฐ์ `a`, `b`์ ์ด๋ ํ ํ์์ ์ธ์๋ฅผ ๋ฃ๊ณ  ํจ์๋ฅผ ์คํํด๋ ๊ฒฐ๊ณผ๊ฐ์ด ๋์ค๋ ๊ฒ์ ๋ณผ ์ ์๋ค.

์ด๋ฒ์ ์์ `add function`์ ๊ทธ๋๋ก ํ์์คํฌ๋ฆฝํธ๋ก ์คํํ๋ฉด ์ด๋ป๊ฒ ๋ ๊น? `a`, `b`์ ์ด๋ ํ ํ์๋ ์ ์ธํ์ง ์์ ์๋ํฐ๋ ์๋์ ๊ฐ์ ์๋ฌ๋ฅผ ๋ณด์ฌ์ค ๊ฒ์ด๋ค.

![type error](../image/Typescript/Typescript/typeError1.png)

์์ ์๋ฌ๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด์๋ `a`, `b`์ ํ์์ ์ ํด์ค์ผ ํ๋ค.

```ts
const add = (a: number, b: number) => a + b;
```

์์ ์ฝ๋์ฒ๋ผ ๊ฐ๊ฐ์ ์ธ์์ `number`๋ผ๋ ํ์์ ์ง์ ํด์ฃผ์๋ค. ์ด๋ ๊ฒ ์ธ์์ ํ์์ ์ ํด๋๋ฉด `number`ํ์์ด ์๋ ๋ค๋ฅธ ํ์์ ์ธ์๋ก ๋ฃ์ด ์คํ์ ํ๊ฒ ๋๋ค๋ฉด ํ์์คํฌ๋ฆฝํธ๋ ๋ค์ ์๋์ ๊ฐ์ ์๋ฌ๋ฅผ ๋ณด์ฌ์ค ๊ฒ์ด๋ค.

```ts
add(true, 4);
```

![type error](../image/Typescript/Typescript/typeError2.png)

์ด๋ ๋ฏ ๊ฐ๋ฐ์๋ค์ ์ฝ๋๊ฐ ์ปดํ์ผ์ด ๋๊ธฐ ์  ํ์์คํฌ๋ฆฝํธ์์ ์๋ฌ๋ฅผ ๋ณด์ฌ์ฃผ์ด ์ฝ๋์ ๋ฌธ์ ๋ฅผ ์๋ ค์ฃผ์ด ์๋ฌ๋ฅผ ํด๊ฒฐํ  ์ ์๋๋ก ๋์์ค๋ค.

[typescript palyground](https://www.typescriptlang.org/ko/play?#code/Q)์์ ์ฌ๋ฌ ํ์์คํฌ๋ฆฝํธ ์ฝ๋๋ฅผ ์์ฑํ๋ฉด์ ์๋ฐ์คํฌ๋ฆฝํธ ์ฝ๋์ ๋น๊ตํ  ์ ์๋ค. ์๋๋ ์์ `add function`๋ฅผ ํ์์คํฌ๋ฆฝํธ palyground์์ ์์ฑํ๊ณ  ์๋ฐ์คํฌ๋ฆฝํธ ์ฝ๋์ ๋น๊ตํ ์ฌ์ง์ด๋ค.

![typescript playground](../image/Typescript/Typescript/typescriptPalyground.png)

---

## 5. Conclusion

> ์์ง ํ์์คํฌ๋ฆฝํธ์ ๋ํด ์ต์ํ์ง ์๋ค. ๋ฆฌ์กํธ์ ํ์์คํฌ๋ฆฝํธ๋ฅผ ํจ๊ป ๊ณต๋ถํ๋ฉด์ ์ ๋ฆฌ๋ฅผ ๊พธ์คํ ํ์ฌ ์์ผ๋ก ํ์์คํฌ๋ฆฝํธ๋ฅผ ๊พธ์คํ ์ฌ์ฉํ  ์ ์๋๋ก ํ์. ํนํ ๋ฆฌ์กํธ์์ ์ฌ์ฉํ๋ ํ์์คํฌ๋ฆฝํธ์ ๋ํด์ ๋ง์ด ๋ฐฐ์ฐ์.๐

---

## ์ฐธ๊ณ 

[ํ์์คํฌ๋ฆฝํธ ๊ณต์ ํํ์ด์ง](https://www.typescriptlang.org/)  
[ํ์ฉ๋๊ฐ ๋์์ง๋ ์น ํ๋ก ํธ์๋ ์ธ์ด, ํ์์คํฌ๋ฆฝํธ(TypeScript)](https://s-core.co.kr/insight/view/%ED%99%9C%EC%9A%A9%EB%8F%84%EA%B0%80-%EB%86%92%EC%95%84%EC%A7%80%EB%8A%94-%EC%9B%B9-%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EC%96%B8%EC%96%B4-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD/)

---

[๐](#typescript๋)
