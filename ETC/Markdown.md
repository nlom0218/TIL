# Markdown

# 1. 개요

마크다운(Markdown)은 마크업 언어의 일종으로, 존 그루버(John Gruber)와 아론 스워츠(Aaron Swartz)가 만들었다. 온갖 태그로 범벅된 HTML 문서 등과 달리, 읽기도 쓰기도 쉬운 문서 양식을 지향한다. 그루버는 마크다운으로 작성한 문서를 HTML로 변환하는 Perl스크립트도 만들었다. 확장자는 .md또는 .markdown을 쓰지만, 전자가 압도적으로 많이 쓰인다.

# 2. 마크다운 문법

## 2-1 문단 제목

```
# H1
## H2
### H3
#### H4
##### H5
###### H6
####### H7 => 지원하지 않음
```

# H1

## H2

### H3

#### H4

##### H5

###### H6

또는 다음과같이 쓸 수 있다.

```
H1
======
H2
------

위와 같이 작성하게 되면 저장시 자동으로
# H1
## H2
로 바뀌게 된다.
```

---

## 2-2 목록

순서있는 목록은 숫자와 점을 사용한다.

```
 1. 첫번째
 2. 두번째
 3. 세번째

 숫자는 반드시 맞춰 쓸 필요는 없다. 어차피 마크다운 기본문법에서는 각 행이 HTML의 <li> 태그로 변환되는 것일 뿐이라 변호 정보가 사라지기 때문이다.
```

1.  첫번째
2.  두번째
3.  세번째

- 순서없는 목록(글머리 기호: `*`, `+`, `-`지원)

```
* 빨강
    * 녹색
        * 파랑

+ 빨강
    + 녹색
        + 파랑

- 빨강
    - 녹색
        - 파랑

*, +, - 는 혼합해서 사용 가능하다.
```

- 빨강
  - 녹색
    - 파랑

* 빨강
  - 녹색
    - 파랑

- 빨강
  - 녹색
    - 파랑

---

## 2-3 인용문

```
> 첫 번째 인용문입니다.
> > 두 번째 인용문입니다.
> > > 세 번째 인용문입니다.
> > > > 네 번째 인용문입니다.
```

> 첫 번째 인용문입니다.
>
> > 두 번째 인용문입니다.
> >
> > > 세 번째 인용문입니다.
> > >
> > > > 네 번째 인용문입니다.

인용문 안에는 다른 마크다운 요소를 포함할 수 있다.

````
> ### h3
> > - List
        - Item
    ```javascript
        console.log("Hello world")
    ```
````

> ### h3
>
> > - List
> >   - Item
> >     - 아이템의 아이템1입니다.
> >     - 아이템의 아이템2입니다.
> >
> > ```javascript
> > console.log("Hello world");
> > ```

---

## 2-4 코드

### 2-4-1 들여쓰기

    this is a code block.

### 2-4-2 코드블럭

- `<pre><code>{code}</code></pre>` 방식
    <pre>
    <code>
        const name = "HD"
        console.log(name)
    </code>
    </pre>

- 코드블럭코드("``")을 이용하는 방법

  ```
  const name = "HD"
  console.log(name)

  ```

  - 깃헙에서는 코드블럭코드 시작점에 사용하는 언어를 선언하여 문법강조가 가능하다.

    ```javascript
    const name = "HD";
    console.log(name);
    ```

---

## 2-5 수평선

```
 * * *
 ***
 *****
 - - -
 -----------------------

 => 모두 --- 로 변환된다?
```

---

---

---

---

---

---

## 2-6 링크

- 외부링크

  ```
  사용문법: [Title](link)
  적용예: [Teachercan](https://teachercan.com)
  ```

  [Teachercan](https://teachercan.com)

- 문서 내부 링크 이동(북마크, 바로가기, 목차): 링크대신 #---으로 작성, 자동완성이 된다.

  [문단 제목](#2-1-문단-제목)

  [목록](#2-2-목록)

  [인용문](#2-3-인용문)

- 폴더의 다른 파일로 이동: 링크대신 /---으로 작성, 자동완성이 된다.

  [README.md](/README.md)

---

## 2-7 강조

```
*single asterisks*
_single underscores_
**double asterisks**
__double underscores__
~~cancelline~~
```

_single asterisks_  
_single underscores_  
**double asterisks**  
**double underscores**  
~~cancelline~~

---

## 2-8 이미지

```
![Alt text](image url)
```

![노마드 코더 인터뷰](/image/food.png)

사이즈 조절 기능이 없기 때문에 `<img width="" height=""></img>`를 이용한다.

<img src="https://images.unsplash.com/photo-1504674900247-0877df9cc836?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80" width="450px" height="300px"  alt="food"></img><br/>
<img src="https://images.unsplash.com/photo-1504674900247-0877df9cc836?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80" width="40%" height="30%"  alt="food"></img><br/>

<img src="./image/food.png" width="450px" height="300px"></img>

---

## 출저

[나무위키-마크다운](https://namu.wiki/w/%EB%A7%88%ED%81%AC%EB%8B%A4%EC%9A%B4)  
[[공통] 마크다운 markdown 작성법](https://gist.github.com/ihoneymon/652be052a0727ad59601)  
[[Markdown] 마크다운 문서 내부 링크 이동하는 법](https://young-cow.tistory.com/21#1-%EA%B0%9C%EC%9A%94)
