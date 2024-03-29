# 프리코스 2주 차 - 숫자 야구 게임 미션

## 1. 개요

22년 11월 2일 15시에 2주 차 미션 안내 이메일이 도착했다. 일주일 동안 이루어진 우아한테크코스 프리코스 2주 차 숫자 야구 게임 미션에 대한 간단한 소개와 소감을 작성한다.

---

## 2. 미션 소개

프리코스 2주 차의 미션은 숫자 야구 게임을 만드는 것이었다. 이전과 다른 점은 DOM를 조작하지 않고 콘솔로 게임을 진행하는 것이었다. 프론트엔드면 DOM를 조작하여 웹 페이지에 띄우는 것이 중요하다고 생각을 하여 의문이 들었지만 2주 차가 끝난 코수타(코치들과의 수다 타임)에서 콘솔로 게임을 진행하는 이유를 설명해 주셨다. 그 이유는 일단 자바스크립트라는 언어에만 집중할 수 있도록 하기 위함이었다.

프리코스 1주 차 미션에서 추가된 요구 사항도 있는데 자세한 내용은 아래의 링크를 참고하면 된다.

[우아한 테크코스 2주 차 숫자 야구 게임 미션 저장소](https://github.com/woowacourse-precourse/javascript-baseball)

---

## 3. 기능 목록 작성

프리코스의 1주 차 미션인 온보딩 미션은 여러 문제들이 있었지만 각각의 문제에서 요구하는 사항은 복잡함이 없었다. 그렇기 때문에 기능 목록을 빠르게 작성하고 코드를 작성할 수 있었다. 하지만 이번 프리코스 2주 차 미션은 그렇지 않았다.

먼저 난 완벽한 기능 목록을 작성하고 싶었다. 어떤 함수가 필요하고 어떤 객체가 필요하고 또 어떤 파일, 폴더로 나누어서 코드를 작성할지 미리 정해놓고 시작을 하고자 하였다. 그래서 기능 목록을 작성하느라 꼬박 하루라는 시간이 걸렸다.

코드를 작성하기 전 구체적인 기능 목록을 작성하니 막상 코드를 작성할 땐 빠르게 마무리할 수 있었다. 테스트 코드의 통과까지 약 6시간이 걸렸던 것으로 기억한다.

또한 꼼꼼하게 기능 목록을 작성하니 요구 사항에 대해 더 자세히 살펴볼 수 있었다. 특히 예외상항을 많이 생각해 볼 수 있어 오류를 잡아내는데 큰 도움이 되었다.

기능 목록의 힘에 대해 몸소 느낄 수 있었던 미션이었다. 하지만 의문점이 들었다. 기능 목록이 어디까지 꼼꼼해야 하는가에 대해 고민을 하게 되었다. 작은 기능을 만드는 것에도 여러 함수가 필요할 때가 있다. 이때 이런 함수들을 모두 기능 목록에 작성하는 것이 좋을지, 아니면 코드를 작성하다가 유연하게 함수를 추가, 삭제하는 것이 좋을지... 이런 고민이 들었다.

그리고 파일, 폴더, 함수명 등과 같은 세부적인 내용을 미리 생각하여 기능 목록에 정리하는 것은 투머치라고 생각이 들었다. 코드를 작성하다보면 더 좋은 구조를 생각할 수 있고 미쳐 생각하지 못했던 함수도 구현해야 할 가능성이 있기 때문에 이런 점은 유연하게 생각하며 기능 목록을 작성하도록 해야겠다.

다음 미션의 목표는 `꼼꼼하고 명확하지만 간결한 기능 목록 작성`이다.

[2주 차 미션의 기능 목록 링크](https://github.com/nlom0218/javascript-baseball/tree/nlom0218/docs)

---

## 4. 함수가 한 가지 일만 하도록 최대한 작게 만들어라.

이번 2주 차 미션에 추가된 요구 사항 중 하나가 바로 `함수가 한 가지 일만 하도록 최대한 적게 만들어라.`이다. 실제 작성하였던 코드를 통해 다시 한 번 요구 사항에 대해 생각해보자.

```javascript
const game = {
  // ...
  getResult: (computerAnswer, playerAnswer) => {
    let ballCount = 0;
    let strikeCount = 0;

    [...playerAnswer].forEach((number, idx) => {
      if (![...(computerAnswer + '')].includes(number)) return;

      if ((computerAnswer + '')[idx] === number) strikeCount++;
      else ballCount++;
    });

    return { ballCount, strikeCount };
  },

  printResult: (ballCount, strikeCount) => {
    if (ballCount === 0 && strikeCount === 0) {
      Console.print(OUTPUT.NOTHING);
      return;
    }

    const ballCountPrint = ballCount !== 0 ? `${ballCount}${OUTPUT.BALL} ` : '';
    const strikeCountPrint =
      strikeCount !== 0 ? `${strikeCount}${OUTPUT.STRIKE}` : '';

    Console.print(ballCountPrint + strikeCountPrint);

    if (strikeCount === 3) Console.print(MESSAGE.SUCCESS);
  },
  // ...
};
```

위 코드의 `getResult` 함수는 컴퓨터의 정답과 플레이어의 답변을 매개변수로 받아 볼의 개수와 스트라이크의 개수를 객체로 반환하는 역할을 한다.

`printResult` 함수는 볼의 개수와 스트라이크의 개수를 매개변수로 받아 콘솔에 게임 결과를 출력하는 역할을 한다.

사실 위의 두 함수는 하나로 묶어서 작성할 수 있다. 바로 아래와 같이 말이다.

```javascript
const game = {
  // ...
  printResult: (computerAnswer, playerAnswer) => {
    let ballCount = 0;
    let strikeCount = 0;

    [...playerAnswer].forEach((number, idx) => {
      if (![...(computerAnswer + '')].includes(number)) return;

      if ((computerAnswer + '')[idx] === number) strikeCount++;
      else ballCount++;
    });

    if (ballCount === 0 && strikeCount === 0) {
      Console.print(OUTPUT.NOTHING);
      return;
    }

    const ballCountPrint = ballCount !== 0 ? `${ballCount}${OUTPUT.BALL} ` : '';
    const strikeCountPrint =
      strikeCount !== 0 ? `${strikeCount}${OUTPUT.STRIKE}` : '';

    Console.print(ballCountPrint + strikeCountPrint);

    if (strikeCount === 3) Console.print(MESSAGE.SUCCESS);
  },
  // ...
};
```

`printResult` 함수 내에서 볼의 개수와 스트라이크의 개수를 모두 구하고 이를 바탕으로 게임 결과를 콘솔로 출력한다. 하지만 이렇게 되면 함수는 한 가지 일만 하지 않는다. 결과를 얻는 일과 결과를 출력하는 일, 총 두 가지의 일을 하기 때문에 이를 두 부분을 나누었다.

함수가 한 가지의 일만 할 수 있도록 최대한 작게 만든다면 일단 함수의 길이가 짧아져 가독성이 좋아지고 만약 코드를 수정하게 될 때 수정이 필요한 코드를 쉽게 찾을 수 있는 장점을 가지게 된다.

---

## 5. 테스트 코드를 통해 작성한 코드가 정상 동작함을 확인한다.

2주 차 미션에 새롭게 추가된 요구 사항 중 하나는 `Jest를 이용하여 본인이 정리한 기능 목록이 정상 동작함을 테스트 코드로 확인한다.`이다. Jest라... 처음 접하는 프레임워크이다.

지금까지 테스트 코드를 한번도 작성하지 않았다. 테스트 코드의 필요성에 대해 생각을 해 본적도 없었을 뿐 더러 테스트라는 것은 콘솔로 출력하는 것 만으로도 충분했기 때문이다.

이번 미션을 위해 Jest를 학습하며 테스트 코드를 직접 작성도 해 보았다. 학습하는 과정을 통해 테스트 코드를 작성하는 것이 번거롭고 귀찮은 것이라 생각을 했지만 나를 위해 그리고 나와 함께 협업하는 동료를 위해 필요하다는 것을 깨닫게 되었다.

아직 Jest로 테스트 코드를 작성하는 것에 익숙하지 않다. 공식 문서를 보면 엄청 많은 메서드들이 존해하고 함수를 만드는? 그런 메서드도 존재한다. 모든 내용을 한 번에 공부하는 것 보다, 필요한 상황이 다가오면 TIL에 정리하며 학습하도록 하자.

이번 미션의 테스트 코드를 작성 하면서 Jest의 메서드 중 가장 만족하는 것은 `describe.each(table)(name, fn, tiemout)`와 ` test.each(table)(name, fn, timeout)`이다. 자세한 내용은 TIL의 `Jest` 파트를 참고하면 된다.

---

## 6. 변수 이름, 함수 이름 짓기

리팩토링 중 가장 많이 번복했던 것이 바로 변수와 함수의 이름 짓기였다. 정말 제출 마감 시간 직전까지 이름을 어떻게 지어야 하는지 고민을 하였다. 지금 다시 보면 마음에 들지 않는 이름들이 눈에 띄지만 이런 고민하는 과정도 이후에 좋은 변수명, 함수명을 짓기 위한 밑거름이라고 생각한다.

중요한 핵심은 `의미 있고 의도가 드러나는 이름을 짓자`라고 생각한다. 미래의 내가 또는 동료들이 이름만 보고도 핵심을 파악할 수 있는 이름을 짓도록 하자.

이름 짓는 방법에 대해 여러 블로그의 글을 보다가 재밌는 문구가 있어 남긴다.

> "좋은 변수명 생각날 때까지 숨 참는다."

---

## 7. Conclusion

> 벌써 4주간의 프리코스 중 2주가 마무리되었다. 뭔가... 아쉬운 마음이 든다. 남은 시간을 더 알차게 보낼 수 있도록 하자.  
> 현재는 프리코스 3주 미션을 진행하고 있다. 아직 끝나지는 않았지만 지금까지 프리코스를 통해 느낀 점은 프리코스가 특정한 지식을 전달하는 것이 아니라 학습하는 습관, 코드를 작성하는 습관을 기르는 것에 초점이 둔 과정이라고 생각한다. 남은 시간을 조금이라도 더 프리코스에 녹아낼 수 있도록 노력하자! 파이팅이다!

---

📅 2022-11-12
