# 코드의 재사용, 상속과 조합

## 1. 개요

클래스에서 코드를 재사용할 수 있는 두 가지 방법에 대해 살펴본다. 상속과 조합이다.

---

## 2. 중복된 코드 발생

로또 미션을 진행하던 중 아래와 같은 코드의 중복이 발생하였다.

```javascript
class Lotto {
  #numbers;
  constructor(numbers) {
    this.validation(numbers);

    this.#numbers = numbers;
  }

  validation(numbers) {
    if (numbers.length !== 6) throw new Error('로또 번호는 6개여야 합니다.');

    if (numbers.some((number) => number > 45 || number < 1))
      throw new Error('로또 번호는 1~45 사이의 정수여야 합니다.');

    if (numbers.length !== new Set(numbers).size)
      throw new Error('로또 번호는 중복되지 않아야 합니다.');
  }
}

class WinningLotto {
  #numbers;
  #bonusNumber;
  constructor(numbers, bonusNumber) {
    this.validation(numbers);

    this.#numbers = numbers;
    this.#bonusNumber = bonusNumber;
  }

  validation(numbers, bonusNumber) {
    if (numbers.length !== 6) throw new Error('로또 번호는 6개여야 합니다.');

    if (numbers.some((number) => number > 45 || number < 1))
      throw new Error('로또 번호는 1~45 사이의 정수여야 합니다.');

    if (numbers.length !== new Set(numbers).size)
      throw new Error('로또 번호는 중복되지 않아야 합니다.');

    if (numbers.includes(bonusNumber))
      throw new Error('보너스 번호는 당첨 번호와 중복될 수 없습니다.');
  }
}

const winningLotto = new WinningLotto([1, 2, 3, 4, 5, 6], 7);
console.log(winningLotto);
```

로또와 당첨번호 인스턴스를 생성할 때 실시하는 유효성 검사 중 3개가 중복되는 것이 보인다. 조금더 자세히 살펴보면 `numbers`라는 6개의 숫자를 요소를 가지는 배열을 검사하는 코드가 겹친다. 상속과 조합을 사용하여 중복된 코드를 제거해보자.

---

## 3. 상속(Inheritance)

`상속`은 상위 클래스에 중복 로직을 구현해두고 이를 하위 클래스에서 물려받아 코드를 재사용하는 방법이다. 이런 `상속`을 `is-a 관계`라고 불린다.

아래는 `상속`을 사용하여 중복된 코드를 제거한 결과이다.

```javascript
class Lotto {
  #numbers;

  constructor(numbers) {
    this.validation(numbers);

    this.#numbers = numbers;
  }

  validation(numbers) {
    if (numbers.length !== 6) throw new Error('로또 번호는 6개여야 합니다.');

    if (numbers.some((number) => number > 45 || number < 1))
      throw new Error('로또 번호는 1~45 사이의 정수여야 합니다.');

    if (numbers.length !== new Set(numbers).size)
      throw new Error('로또 번호는 중복되지 않아야 합니다.');
  }
}

class WinningLotto extends Lotto {
  #bonusNumber;

  constructor(numbers, bonusNumber) {
    super(numbers);
    this.validationWinningLotto(numbers, bonusNumber);

    this.#bonusNumber = bonusNumber;
  }

  validationWinningLotto(numbers, bonusNumber) {
    if (numbers.includes(bonusNumber))
      throw new Error('보너스 번호는 당첨 번호와 중복될 수 없습니다.');
  }
}

const winningLotto = new WinningLotto([1, 2, 3, 4, 5, 6], 7);
console.log(winningLotto);
```

`당첨 로또`는 `로또`를 상속받았기 때문에 로또 내의 유효성 검사를 사용할 수 있다. 이렇게 부모 클래스에 정의된 메소드를 물려받아 재사용하는 방법을 `상속`이라고 한다. 현재까진 아무런 문제가 없다.

하지만, `Lotto` 클래스를 생성할 때, 로또의 회차도 매개변수로 받아야 한다는 요구 사항이 생긴다면 대대적인 공사가 필요할 것이다. 우선 `Lotto` 클래스에만 로또 회차를 의미하는 `rounds`를 매개변수로 추가해보자.

```javascript
class Lotto {
  #numbers;
  #rounds;

  constructor(numbers, rounds) {
    this.validation(numbers, rounds);

    this.#numbers = numbers;
    this.#rounds = rounds;
  }

  validation(numbers, rounds) {
    if (numbers.length !== 6) throw new Error('로또 번호는 6개여야 합니다.');

    if (numbers.some((number) => number > 45 || number < 1))
      throw new Error('로또 번호는 1~45 사이의 정수여야 합니다.');

    if (numbers.length !== new Set(numbers).size)
      throw new Error('로또 번호는 중복되지 않아야 합니다.');

    if (!rounds) throw new Error('회차가 존재하지 않습니다.');

    if (rounds < 0) throw new Error('회차는 0이상이어야 합니다.');
  }
}

class WinningLotto extends Lotto {
  #bonusNumber;

  constructor(numbers, bonusNumber) {
    super(numbers);
    this.validationWinningLotto(numbers, bonusNumber);

    this.#bonusNumber = bonusNumber;
  }

  validationWinningLotto(numbers, bonusNumber) {
    if (numbers.includes(bonusNumber))
      throw new Error('보너스 번호는 당첨 번호와 중복될 수 없습니다.');
  }
}

const winningLotto = new WinningLotto([1, 2, 3, 4, 5, 6], 120, 7);
console.log(winningLotto);
```

`Lotto` 클래스에 새로운 `rounds` 필드가 생겼고 이를 위한 유효성검사도 추가되었다. 그래서 `당첨 번호` 인스턴스를 만들 때, 두번째 인자로 로또 회차, 세번째 인자로 보너스 번호를 넘겨주기로 했다. 이런 경우 어떤 에러가 발생하게 될까? 바로 `WinningLotto` 클래스 내부에서는 아직 `rounds`에 대해 받지 않았고 `super`를 통해 상위 클래스로 전달하지 않았기 때문에 결론적으론 `Lotto` 클래스엔 `rounds`를 받을 수 없다.

그렇다면 해결책은 무엇일까? 간단하다. `WinningLotto` 클래스에도 `Lotto` 클래스와 똑같이 `rounds`를 받고 이를 `super`를 통해 상위 클래스로 전달해주면 된다. 어렵지 않다. 하지만, 복잡한 클래스 설계를 생각해보자. 상위 클래스에서 하나가 바뀌어도 모든 하위 클래스에도 영향을 주기 때문에 의존성이 강하게 결합이 된다고 볼 수 있다. 이런한 점이 `상속`을 사용할 때의 단점이 된다.

다시 말해, 상위 클래스의 구현은 하위 클래스에게 노출되어 캡슐화가 약해지고, 하위 클래스와 상위 클래스는 강하게 결합하여 상위 클래스의 단순한 변경에도 하위 클래스에겐 치명적으로 다가갈 수 있다.

이렇듯 `상속`을 너무 남발하게 된다면 서로의 결합이 강하고 응집도가 떨어지며 유연하지 못한 코드가 탄생할 수 있다.

---

## 4. 조합(Composition)

`조합`은 중복되는 로직들을 갖는 객체를 구현하고, 이 객체를 주입받아 중복 로직을 호출함으로써 퍼블릭 인터페이스를 재사용하는 방법이다. 흔히 `조합`은 `Has-a 관계`라고 많이 불린다.

이번엔 `조합`을 사용하여 코드의 중복을 제거해보자.

```javascript
class Lotto {
  #numbers;
  constructor(numbers) {
    this.validation(numbers);

    this.#numbers = numbers;
  }

  validation(numbers) {
    if (numbers.length !== 6) throw new Error('로또 번호는 6개여야 합니다.');

    if (numbers.some((number) => number > 45 || number < 1))
      throw new Error('로또 번호는 1~45 사이의 정수여야 합니다.');

    if (numbers.length !== new Set(numbers).size)
      throw new Error('로또 번호는 중복되지 않아야 합니다.');

    if (!rounds) throw new Error('회차가 존재하지 않습니다.');

    if (rounds < 0) throw new Error('회차는 0이상이어야 합니다.');
  }

  has(bonusNumber) {
    return this.#numbers.includes(bonusNumber);
  }
}

class WinningLotto {
  #winningLotto;
  #bonusNumber;

  constructor(winningLotto, bonusNumber) {
    this.validation(winningLotto, bonusNumber);

    this.#winningLotto = winningLotto;
    this.#bonusNumber = bonusNumber;
  }

  validation(winningLotto, bonusNumber) {
    if (winningLotto.has(bonusNumber))
      throw new Error('보너스 번호는 당첨 번호와 중복될 수 없습니다.');
  }
}

const winningLotto = new WinningLotto(new Lotto([1, 2, 3, 4, 5, 6]), 7);
console.log(winningLotto);
```

중복되는 코드를 제거하기 위해 `WinningLotto` 인스턴스를 생성할 때, 첫번째 인자를 숫자 배열이 아닌, `Lotto` 인스턴스를 전달한다. 이처럼 인스턴스 변수로 Lotto 클래스를 가지는 것이 `조합(Composition)`이다. WinningLotto 클래스는 Lotto 클래스의 메서드를 호출하는 방식으로 동작하게 된다. 위 코드에선 보너스 번호가 로또 번호에 포함된 여부를 반환하는 `has` 메서드가 바로 그것이다.

메서드를 호출한다는 것은 메시지를 주고 받는다와 같다. 즉, 두 개의 클래스가 강하게 결합되는 것이 아닌 단지, 메세지를 주고 받으며 느슨하게 결합되는 장점을 가진다.

`상속`에서 로또 회차인 `rounds`를 추가했던 것과 같이 똑같이 추가해보자.

```javascript
class Lotto {
  #numbers;
  #rounds;

  constructor(numbers, rounds) {
    this.validation(numbers);

    this.#numbers = numbers;
    this.#rounds = rounds;
  }

  validation(numbers) {
    if (numbers.length !== 6) throw new Error('로또 번호는 6개여야 합니다.');

    if (numbers.some((number) => number > 45 || number < 1))
      throw new Error('로또 번호는 1~45 사이의 정수여야 합니다.');

    if (numbers.length !== new Set(numbers).size)
      throw new Error('로또 번호는 중복되지 않아야 합니다.');
  }

  has(bonusNumber) {
    return this.#numbers.includes(bonusNumber);
  }
}

class WinningLotto {
  #winningLotto;
  #bonusNumber;

  constructor(winningLotto, bonusNumber) {
    this.validation(winningLotto, bonusNumber);

    this.#winningLotto = winningLotto;
    this.#bonusNumber = bonusNumber;
  }

  validation(winningLotto, bonusNumber) {
    if (winningLotto.has(bonusNumber))
      throw new Error('보너스 번호는 당첨 번호와 중복될 수 없습니다.');
  }
}

const winningLotto = new WinningLotto(new Lotto([1, 2, 3, 4, 5, 6], 120), 7);
console.log(winningLotto);
```

`WinningLotto` 클래스의 내부의 수정 없이 `Lotto` 클래스에 `rounds` 필드를 추가하였다. 이렇듯 `조합`으로 코드의 중복을 제거하면 클래스 간의 결합도를 줄이면서 유연한 코드 작성이 가능하게 된다.

---

## 5. 그렇다면 언제 상속을?

코드를 작성하는데 100%의 정답은 없다. 단지 상황에 알맞는 코드를 구현하는 것이 최선일 뿐이다.

위의 예제를 통해 `상속` 보다 `조합` 방법을 사용하여 코드의 중복을 제거하는 것이 좋다는 것을 알 수 있었다. 그렇다면 상속은 사라져야 하는 것일까? 아니다. 아래와 같은 경우엔 `조합` 보다 `상속`이 더 알맞을 수 있다.

1. 확장을 고려하고 설계한 확실한 is - a 관계일 때
2. API에 아무런 결함이 없는 경우, 결함이 있다면 하위 클래스까지 전파돼도 괜찮은 경우

코드의 재사용을 위해서만 `상속`을 사용하지 말자. `상속`에 대한 접근은 코드의 재사용이 아니라, 코드의 확장에 있다.

---

## 6. Conclusion

> 우테코 레벨1 로또 미션에서 중복되는 코드를 제거하기 위해 난 클래스의 상속, 조합이 아닌 유틸 함수를 활용했다. 이런 방법은 생각지도 못했기 때문에 꼭 정리를 해야겠다고 생각을 하였다. 미션을 진행하면서 여러 가지의 고민을 하게 되는 듯 하다. 이런 과정에서 나만의 기준이 조금씩 생기고 있는데, 기준이 잘 잡혀 앞으로 잘 해쳐나갔으면 한다.  
> 상속과 조합에 대한 내용은 그다지 어렵지 않은 개념이라고 생각이 든다. 물론 코드 자체가 아주 간단해서 그렇게 느끼는 거지만, 바로 전에 공부한 클로저에 비하면 어떤 의미인지, 어떤 상황에서 사용하면 좋을지가 명확하게 떠올라서 더욱 쉽게 정리를 하였다. 다음 미션 때, 혹은 로또 미션 리팩터링 기회가 생긴다면 조합을 사용하여 클래스를 만들고 중복되는 코드를 제거해보자.

---

## 참고

[[OOP] 코드의 재사용, 상속(Inheritance)보다 합성(Composition)을 사용해야 하는 이유](https://mangkyu.tistory.com/199#recentEntries)  
[상속보다는 조합(Composition)을 사용하자.](https://developer.mozilla.org/ko/docs/Glossary/World_Wide_Web)

---

📅 2023-02-20
