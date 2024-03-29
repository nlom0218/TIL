# 프로세스와 컴파일 과정

## 1. 프로그램란?

파일 시스템에 존재하는 실행파일

ex) Chrom.exe, Hwp.exe

## 2. 프로세스란?

프로그램의 한 개의 인스턴스가 프로세스이며 프로그램은 하나지만, 이 프로그램을 실행하는 인스턴스는 여러 개일 수 있다.

프로세스는 CPU에 올라가서 실행된다.

> 피자 레시피(프로그램) -> 피자(프로세스)

---

## 3. 컴파일이란?

컴파일은 인간이 이해할 수 있는 언어로 작성된 소스 코드를 CPU가 이애할 수 있는 언어로 변환하는 작업이다.

## 4. 컴파일 과정

![컴파일 과정](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGb9WO%2FbtrdpL4fvcQ%2Fspc9IYinoZhgHRmJ0l0kjK%2Fimg.png)

컴파일 과정은 4가지 단계로 나누어진다.

---

### 4-1. 전처리 과정

![전처리 과정](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FxVMXr%2FbtrdqKw3ccJ%2FARAfM8BBcKlMQyN3rIQ691%2Fimg.png)

전처리 과정은 소스 코드 파일을 전처리 된 소스 코드 파일로 변환하는 과정이다.

대표적으로 세 가지 작업을 수행한다.

1. 주석 제거: 소스 코드에서 주석을 전부 제거한다.

2. 헤더 파일 삽입: #include 지시문을 만나면 해당하는 헤더 파일을 찾아 헤더 파일에 있는 모든 내용을 복사해서 소스 코드에 삽입한다.

3. 매크로 치환 및 적용: #define 지시문에 정의된 매크로를 저장하고 같은 문자열을 만나면 #define 된 내용으로 치환한다.

---

### 4-2. 컴파일 과정

![컴파일 과정](https://blog.kakaocdn.net/dn/bLTLoJ/btrdpsDQiXD/dPcd4AFAaTpTLp4S5ODj5K/img.png)

컴파일 과정은 컴파일러를 통해 전처리된 소스 코드 파일을 어셈블리어 파일로 변환하는 과정이다.

어셈블리어란? 기계어(명령어, 이진수로 이루어진 숫자)와 일대일 대응이 되는 컴퓨터 프로그래밍의 저급 언어이다.

이 과정에서 언어의 문법 검사가 이루어진다.

---

### 4-3. 어셈블리 과정

![어셈블리 과정](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fsz3Fu%2FbtrdqSBMnid%2Fqb4T0MZXpliiZ2xfFtuLM0%2Fimg.png)

어셈블리 과정을 통해 어셈블리어 파일을 오브젝트 파일로 변환한다.

오브젝트 파일이란? 어셈블리 코드는 이제 더 이상 사람이 알아볼 수 없는 기계어로 변환되는데 이를 오브젝트 코드라 부른다. 이런 오브젝트 코드로 구성된 파일을 오브젝트 파일이라고 부른다.

---

### 4-4. 링킹 과정

![링킹 과정](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdW1GTK%2FbtrdqLirXQS%2Fupv8Q3omleeiAIGGPlCJdk%2Fimg.png)

링킹 과정은 오브젝트 파일들을 묶어 실행 파일로 만드는 과정이다.

이 과정에서 오브젝트 파일들과 프로그램에서 사용하는 라이브러리 파일들을 링크하여 하나의 실행 파일을 만든다.

라이브러리를 링크하는 방법에 따라 정적 링킹과 동적 링킹으로 나눈다.

1. 정적 라이브러리
   - 정적 링킹 과정에서 프로그램에 필요한 부분을 라이브러리에서 찾아 실행 파일에 복사하는 방식의 라이브러리
   - 실행 파일이 정적 라이브러리를 복사해서 가지고 있으므로 실행할 때 라이브러리가 필요없다. 즉, 실행 파일만 있으면 프로그램이 동작하여 안정적이다.
   - 라이브러리에서 수정할 부분이 있으면 파일 전체를 다시 컴파일하여 재배포해야 한다.
   - 실행 파일 크기가 커진다.
   - 코드가 중복되어 메모리 자원을 낭비한다.
   - 사용하지 않는 함수들까지 전부 다 프로그램에 포함한다.
2. 동적 링킹
   - 동적 링킹 과정에서 라이브러리 내용을 복사하지 않고 해당 내용의 주소만 가지고 있다가 런타임에 실행 파일과 라이브러리가 메모리에 위치할 때 해당 주소로 가서 필요한 내용을 가져오는 방식의 라이브러리
   - 실행 파일 크기가 작아진다.
   - 동적 라이브러리를 메모리에 올려놓고 공유하기 때문에 메모리 자원을 효율적으로 사용한다.
   - 동적 라이브러리가 제대로 링크되어 있지 않거나 버전이 맞지 않는 등의 문제가 발생할 수 있다. 즉, 외부 의존도가 생긴다.

---

## 참고

[프로그렘과 프로세스의 차이점](https://wookkingkim.tistory.com/entry/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8%EA%B3%BC-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)  
[컴파일(Compile)에 대한 이해](https://bradbury.tistory.com/226)  
[라이브러리(Libray)에 대한 이해](https://bradbury.tistory.com/224)  
[위키백과 - 어셈블리어](https://ko.wikipedia.org/wiki/%EC%96%B4%EC%85%88%EB%B8%94%EB%A6%AC%EC%96%B4)

---

📅 2022-10-10
