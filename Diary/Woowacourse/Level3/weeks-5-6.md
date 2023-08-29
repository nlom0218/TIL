# 레벨3 5~6주차 회고

## 배포 준비 완료

2차 데모데이를 기준으로 하루스터디의 핵심 기능은 모두 완성되었다. 스터디를 개설하고 참여하고 진행하고 기록을 보는 것까지 모두 가능했다. 하지만 아쉬웠던 점은 배포를 하지 않았기 때문에 2차 데모데이에서 실제로 동작하는 서비스를 보여주지 못했던 것이다. 심지어 클라이언트와 서버의 연동도 확인하지 않아 어떤 에러가 있는지 파악도 못한 상태였다. 때문에 2차 데모데이가 끝난 이후 **우선적으로 해야할 일이 바로 배포를 완료**하여 실제 사용자들이 하루스터디 서비스를 이용할 수 있게 만드는 것이었다.

스터디 개설, 진행에 대한 추가 작업을 마무리 한 뒤, 클라이언트와 서버를 배포 완료하였고, 실제 배포가 잘 되었는지 확인을 해보았지만 역시나... 예상대로 정상 동작을 하지 않았다. 내가 맡았던 부분인 기록 페이지에서도 통신 에러가 나타났고, 개설 페이지에서도 에러가 나타났다. 또한 캐시도 항상 남아있었기 때문에 새롭게 배포를 할 때마다 캐시를 수동으로 지워줘야 하는 번거로움이 있었다.

---

## 왜 문제가 생겼을까?

한번의 시도로 배포를 성공하지 못했다. 더 정확히 말해서 서버와 api 통신이 잘 이루어지지 않았다. 여기에는 여러 문제들이 있었다. 이런 문제들을 마주하고 해결하는 과정을 정리해보고자 한다.

### 느슨한 타입

하루스터디 프로젝트에서는 typescript를 사용하고 있다. 타입스크립트의 장점 중 하나는 런타임에서의 오류를 미리 알 수 있어 오류를 빨리 파악하고 해결할 수 있는 것이다. 하지만 이러한 장점도 **얼마나 정확한 타입을 사용하느냐? 에** 따라 장점으로 다가올 수도 있고 단점으로 다가올 수 있다. 타입오류가 없어 타입이 정확하다고 생각하여 다른 곳에서 문제를 찾느라 시간을 소비하는 것이 대표적인 단점이라고 할 수 있다.

그렇다면 어떤 과정에서 느슨한 타입으로 인해 문제가 발생했을까? 바로 스터디 개설과정이다. 코드를 살펴보기 전, 스터디를 개설하기 위해선 3가지의 값이 필요하다. 스터디 이름, 사이클 횟수, 사이클 당 학습 시간이 그것이다. 이를 api를 통해 서버로 전송해야 한다. 다음이 이에 해당하는 코드(오류가 발생하는 코드)이다.

1. requestCreateStudy: 스터디 개설을 위한 api 요청 함수

```tsx
export const requestCreateStudy = async (
  studyName: string,
  totalCycle: number,
  timePerCycle: number
) => {
  const response = await http.post(`/api/studies`, {
    body: JSON.stringify({ name: studyName, totalCycle, timePerCycle }),
  });
  // ...
};
```

2. createStudy: 스터디 개설하기를 클릭하면 실행되는 함수, requestCreateStudy가 실행된다.

```tsx
const createStudy = async (
  studyName: string,
  timePerCycle: number,
  totalCycle: number
) => {
  try {
    // ...
    return await requestCreateStudy(
      studyName,
      totalCycle,
      timePerCycle,
      accessToken
    );
  } catch (error) {
    // ...
  }
};
```

이 둘을 살펴보자. 우선 requestCreateStudy함수는 3개의 매개변수를 가진다. 이는 다음과 같다.

1.  studyName: string 타입으로 스터디 이름에 해당한다.
2.  totalCycle: number 타입으로 스터디의 총 사이클 횟수에 해당한다.
3.  timePerCycle: number 타입으로 사이클 당 학습시간에 해당한다.

때문에 requestCreateStudy함수를 사용하는 곳에서는 3개의 인자를 전달해야 한다. 이어서 createStudy함수를 보면,  requestCreateStudy함수를 호출하고 3개의 인자를 전달하고 있다. 하지만 이곳에서 api 통신 오류가 나타났다. 그 이유는 조금더 자세히 살펴보면 알 수 있다. 전달해야 할 인자의 순서가 잘못되었기 때문이다. 즉, totalCycle를 넘겨야 할 자리에 timePerCycle를 넘기고 있고 timePerCycle를 넘겨야 할 자리에 totalCycle를 넘기고 있다.

이런 오류를 빌드 이전에 왜 알지 못했을까? 그 이유는 바로 **totalCycle, timePerCycle 모두 number타입**이기 때문이다. 물론 둘다 number가 맞다. 하지만 값이 서로 다르다. 다음은 각각의 인자가 가질 수 있는 값들이다.

- totalCycle: 1, 2, 3, 4, 5, 6, 7, 8
- timePerCycle: 20, 25, 30, 35, 40, 45, 50, 55, 60

확실히 다른 값일 뿐 더러 서비스 내에서 정해진 값들이다. 때문에 이를 number타입으로 관리하는 것보다 상수로 만들어 유니온으로 관리하는 것이 좋다. 이를 적용하여 오류를 해결할 수 있었다. 다음은 이를 적용한 코드이다.

```tsx
// Constants
const TOTAL_CYCLE_OPTIONS = [1, 2, 3, 4, 5, 6, 7, 8] as const;
const STUDY_TIME_PER_CYCLE_OPTIONS = [
  20, 25, 30, 35, 40, 45, 50, 55, 60,
] as const;

// Types
type TotalCycleOptions = (typeof TOTAL_CYCLE_OPTIONS)[number]; // 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8
type StudyTimePerCycleOptions = (typeof STUDY_TIME_PER_CYCLE_OPTIONS)[number]; // 20 | 25 | 30 | 35 | 40 | 45 | 50 | 55 | 60

// API
export const requestCreateStudy = async (
  studyName: string,
  totalCycle: TotalCycleOptions, // 타입 적용
  timePerCycle: StudyTimePerCycleOptions // 타입 적용
) => {
  const response = await http.post(`/api/studies`, {
    body: JSON.stringify({ name: studyName, totalCycle, timePerCycle }),
  });
  // ...
};

// Hooks
const createStudy = async (
  studyName: string,
  totalCycle: TotalCycleOptions, // 타입 적용
  timePerCycle: StudyTimePerCycleOptions // 타입 적용
) => {
  try {
    // ...
    return await requestCreateStudy(
      studyName,
      totalCycle,
      timePerCycle,
      accessToken
    );
  } catch (error) {
    // ...
  }
};
```

이제 totalCycle과 timePerCycle의 타입은 느슨한 number가 아니라 서비스에 정한 특정 값의 모음(유니온)이다. 위와 같은 코드에서는 인자의 순서가 잘못되었을 경우 오류를 알려주어 빌드 전에 오류를 미리 파악하고 해결할 수 있다.

위의 문제를 찾는 것도 시간이 걸렸다. 타입 지정엔 문제가 없다고 생각하여 다른 곳에서 문제를 찾으려 했기 때문이다. 물론 number란 타입이 잘못된 것은 아니다. 하지만 특정 값이 정해졌다면 number라는 느슨한 타입보다는 유니온으로 만들어 타입을 좁히는 것이 좋다.

### 명세를 잘 보자

이번 문제는 쉽게 해결할 수 있는 문제이다. 쉽게 해결 수 있으면서 가장 많이 나타나는 문제이다. 바로 클라이언트와 서버의 일치하지 않는 api주소 혹은 request body, response body이다. 이러한 문제는 api문서화가 잘 되지 않거나 명세가 업데이트되었지만 공지가 안되어 확인을 못할 때, 혹은 명세를 제대로 읽지 않았을 경우에 발생한다. 기술적인 문제가 아니라 단순한 휴먼 에러이기 때문에 모든 팀원이 이를 실수하지 않도록 노력을 해야 한다고 생각한다.

아무튼, 이번에 내가 맡은 스터디 기록 페이지에서는 다음과 같은 두 개의 api가 사용된다.

1.  스터디의 메타데이터를 불러오는 api: api/studies/studyId/metadata;
2.  스터디원의 스터디 기록을 불러오는 api: api/studies/studyId/members/memberId/content

이중 오류가 나타났던 api는 스터디원의 스터디 기록을 불러오는 api이다. api주소는 맞다. 하지만 응답 데이터의 타입이 서로 달랐다. 다행히 배포하기 전 명세를 살펴보다가 서로 다른 점을 찾아 수정을 하였다. 과연... 내가 처음에 잘 못 본 것인지, 아니면 중간에 명세가 바뀐 것인지는 모르겠지만 그래도 빠르게 문제를 해결할 수 있었다.

다음은 내가 초기에 작성한 스터디원의 스터디 기록의 응답 타입이다.

```ts
type MemberRecordContent = {
  cycle: number;
  plan: {
    toDo: string;
    completionCondition: string;
    expectedProbability: string;
    expectedDifficulty: string;
    whatCanYouDo: string;
  };
  retrospect: {
    doneAsExpected: string;
    experiencedDifficulty: string;
    lesson: string;
  };
};
```

하지만 실제론 다음과 같은 타입의 응답이 온다.

```ts
type MemberRecordContent = {
  content: {
    cycle: number;
    plan: {
      toDo: string;
      completionCondition: string;
      expectedProbability: string;
      expectedDifficulty: string;
      whatCanYouDo: string;
    };
    retrospect: {
      doneAsExpected: string;
      experiencedDifficulty: string;
      lesson: string;
    };
  };
};
```

\`content\` 키가 생겼고 해당 키의 값에 cycle, plan, retrospect가 담겨 온다. 객체 자체로 오느냐 키에 담겨서 오느냐에 차이이다. 내가 \`content\`를 보지 못했던 것일까? 너무 기본적인 실수라 조금 어이는 없지만 서버에서 보내주는 응답 타입에 맞게 수정하였다. 다음은 이에 해당하는 PR이다.

[https://github.com/woowacourse-teams/2023-haru-study/pull/158](https://github.com/woowacourse-teams/2023-haru-study/pull/158)

다음부턴 명세에 대한 실수를 하지 않도록 꼼꼼하게 스웨거를 확인해야겠다.

---

## 웹 접근성(처음 사용해보는 보이스 오버)

3차 스프린트에서는 기능 개발뿐 아니라 웹 접근성도 신경을 쓰며 개발을 해야 했다. 이중 가장 쉽고 기본적인 것을 적용하였다. 바로 페이지 언어 표시, 시멘틱 태그 사용, 레이블 제공이 그것이다. 뿐만 아니라 하루 스터디의 핵심 기능을 보이스 오버로만 사용할 수 있도록 구현을 해야 했다.

보이스 오버라는 기능을 준의 웹 접근성 강의에서 처음 사용해보았다. 처음 사용하는 것이어서 그런지 굉장히 부자연스럽고 어려웠다. 그렇다면 지금까지 보이스 오버를 사용하지 않았던 이유는 무엇일까? 단지 이를 사용할 필요성을 느끼지 않았기 때문이다. 굳이 보이스 오버를 켜서 웹 서비스를 이용하지 않아도 충분하다. 하지만 보이스 오버가 반드시 필요한 사람들이 있다. 바로 시각장애인들이다. 그들은 소리를 통해 현재 어떤 버튼 위에 있는지, 해당 페이지에서는 어떤 작업을 할 수 있는지 알아야 한다.

아직 어색하긴 하지만 눈을 감고도 하루 스터디의 핵심 기능을 사용할 수 있도록 개선을 하였다. 이 부분은 룩소가 진행하였고 PR를 통해 어떻게 진행을 했는지 알 수 있었다.

[https://github.com/woowacourse-teams/2023-haru-study/pull/248](https://github.com/woowacourse-teams/2023-haru-study/pull/248)

aira(Accessible Rich Internet Application) 태그를 적절히 사용하면 탭을 사용하여 현재의 영역을 이동할 때마다 현재 위치에 대한 설명을 들을 수 있다. 여러 종류가 있기 때문에 이를 잘 정리해 볼 필요가 있다고 생각한다. 하는 김에 웹접근성에 대해서도 쭉 정리를 해보아야겠다.... 글 써야지...

다음은 준의 웹 접근성 강의에서 여러 삽질을 시도한 코드가 담긴 레포이다. 그중 재밌는 코드를 가져왔다.

[https://github.com/nlom0218/a11y-airline/tree/nlom0218](https://github.com/nlom0218/a11y-airline/tree/nlom0218)

위 `div` 태그 내의 `text`는 단지 보이스 오버를 위한 값이다. 즉, 화면에는 보여지지 않고 `text`가 업데이트될 때마다 보이스 오버를 통해 `text`의 내용을 들을 수 있다. 조금 더 어떤 상황이었는지 설명을 붙이자면, 성인 승객의 수가 증가, 감소할 때마다 클릭이 잘 되었는지 알려주는 역할을 한다. 또한 최소, 최대 인원수가 되었을 때도 알려준다.

동작은 잘 되지만 뭔가 이상하다. 조금더 세련되게 바꾸고 싶은 마음이 들었다. 때문에 다음과 같은 방법도 여러 크루들을 통해 알게 되었다. 다음의 코드에서 `input`태그는 사용자의 동작에 따라 `count`가 계속 바뀐다. 때문에 변할 때마다 `aria-label`를 다시 읽어주기 위해  `aria-live` 속성을 `polite`로 정하였다.

![position 어지러운 코드](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FkIzYl%2Fbtsr5XhlOH8%2FNJfA0ao4KWjjUT2g8tvqLk%2Fimg.png)

aira뿐 아니라 웹 접근성에 대한 좋은 자료가 있어 남긴다. 이를 토대로 언제 한 번 날 잡고 공부해보자.

[https://github.com/lezhin/accessibility](https://github.com/lezhin/accessibility)

---

## 8월 4일 3차 데모 데이

다 쓴 글이 날아가서 마음이 매우 아프다............

잠실 캠퍼스로 이동한 지 벌써 2주가 흘렀다. 3차 스프린트도 끝이 났고 3차 데모 데이도 금방 찾아왔다. 이러다가 곧 수료하는 날이 올 거 같다.(아직 부족한데 말이다.)

이번 3차 데모 데이의 발표는 나와 히이로가 맡게 되었다. 데모 데이 전날엔 ppt를 만들면서 발표 준비를 하였다. 발표 시간은 서비스 시연을 포함하여 총 15분이다. 생각보다 짧은 시간이다. 때문에 많은 내용을 ppt에 담을 순 없었다. 실제로 많은 내용을 담았다가 시간이 너무 오버되는 바람에 절반정도를 뺄 수밖에 없었다. 아쉽지만 발표시간도 지켜야 하기 때문에 발표내용을 간추리고 간추렸다.

발표의 차례는 총 5가지로 다음과 같다.

1.  3차 스프린트에서 마주친 문제상황
2.  필수 요구사항 반영
3.  하루스터디의 협업
4.  시연
5.  4차 데모데이 목표

[https://docs.google.com/presentation/d/12KHuR8B-19mPafBuv-sZURkP4bWZsIQ_XjkgRr5A2A8/edit?usp=sharing](https://docs.google.com/presentation/d/12KHuR8B-19mPafBuv-sZURkP4bWZsIQ_XjkgRr5A2A8/edit?usp=sharing)

2차 스프린트 때와 비교하여 3차 스프린트 때에는 추가된 기능이 없어 아쉬움이 많이 남았다. 하지만 안정적인 서비스 운영도 사용자에게 좋은 서비스 경험을 주기 때문에 2주 동안 작업한 내용들이 아쉽지는 않다. 물론 지금도 소소한 버그가 있기 때문에 꾸준히 개선하고 있는 상황이다. 그렇다고 앞으로 계속 안정적인 서비스를 만들기 위해서만 프로젝트를 진행하진 않을 것이다. 부족한 기능을 추가하는 것도 생각하고 있다. 그중 다음과 같이 아쉬움을 바탕으로 다음 4차 스프린트 목표를 세웠다.

1.  스터디 기록을 다시 확인할 수 없다는 아쉬움 -> **로그인 + 자신의 스터디 기록 페이지**
2.  함께 스터디를 진행한다는 느낌이 들지 않는다는 아쉬움 -> **함께 시작 기능 + 타이머 동기화 기능**

2개의 기능을 4차 스프린트 때 모두 구현하면 좋겠지만 지금까지의 경험 상 그렇지 못할 확률이 크다. 때문에 우선순위를 두고 하나씩 완성해 보려고 한다.
