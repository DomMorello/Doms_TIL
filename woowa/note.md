피드백 내용:
- 상수를 선언해서 사용하긴 했는데 정작 중요한 의미를 담을 수 있는 숫자 부분에서도 상수를 선언해서 하도록 신경써야 해야 한다.
- 불필요한 변수를 줄이기 위해 노력해라(어떤 변수를 갖고 있는 이를 통해 얻어낼 수 있는 값이라면 함수로 분리하는 것이 더 좋다)

## 1. 자판기

---

우아한테크코스 4기의 프리코스 3주차 미션은 자판기 구현이다. 구현 내용은 [프리코스 3주차 미션 저장소](https://github.com/DomMorello/javascript-vendingmachine-precourse/tree/dom)에 업로드했다.

3주차 미션을 진행하면서 고민한 내용들에 대해 개인적으로 공부하고 정리해보았다.

<br>

## 2. 구현 중에 했던 고민들

---

### 2-1. 클래스가 서로 어떻게 상호작용할 것인가?

**여러 개의 클래스를 분리한 후 서로 관계를 맺어 하나의 프로그램을 완성**하는 것이 3주차 미션의 목표이다. 
### 2-2. UI를 어떻게 할까?

UI를 잘 만들면 예외를 처리하는 코드를 줄일 수 있다.

### 2-3. 생성자 함수 VS 클래스

과제에서 **클래스를 분리하는 것**이 2주차의 목표라고 했다. 그런데 JS 문법을 보면 생성자 함수도 있고 클래스 문법도 있다. 클래스 문법은 비교적 최신 JS 문법이다. 클래스에 대해서 공부를 해봤는데 생성자 함수와의 차이점에 대해서 크게 모르겠다. 아래에서 보다시피 과제에서도 클래스 형식을 쓸지 생성자 함수 형식을 쓸지를 선택하라고 한다.

```js
function Car(name) {
  this.name = name;
}

class Car {
  constructor(name) {
    this.name = name;
  }
}
```

그렇다는 말은 생성자 함수로 객체를 만들어 사용하는 것도 2주차 목표 중 하나인 클래스의 분리라고 할 수 있다. 그래서 처음에는 생성자 함수를 사용하는 방식으로 하려고 했다. 그런데 생성자 함수를 통해서 객체를 생성하면 **객체를 생성할 때마다 생성자 함수 안에 정의된 함수가 새로 정의되고 할당**된다. 같은 객체들이 공통적으로 사용할 같은 함수인데 여러 번 정의, 할당되는 것은 낭비기 때문에 이를 해결할 방법이 필요했다.

그 방법은 **프로토타입**을 사용하는 것이다.

```js
function Person(name) {
  this.name = name;
}

Person.prototype.sayHi = function() {
  console.log(this.name + ' says hi');
};
const dom = new Person('dom');
const morello = new Person('morello');

dom.hi();
morello.hi();
```

위 코드에서처럼 **프로토타입** 함수를 선언하면 생성자 함수를 통해 객체가 생성되더라도 `sayHi` 함수는 한 번만 정의되고 할당된다.

좋은 방법처럼 보이지만 이 방법에 문제가 있었다. [**에어비엔비 코드컨벤션**](https://github.com/ParkSB/javascript-style-guide)을 보면 다음과 같은 항목이 있다.

![image](./airbnb.png)

위와 같이 **프로토타입을 직접 조작**하는 것을 허용하지 않는다. 대신에 클래스를 사용하라고 한다.
그렇다면 클래스는 객체를 생성할때마다 새로운 함수를 정의하고 할당하는 낭비를 해결할 방법은 없을까?

```js
class Person {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    console.log('..');
  }
}
```

위 함수처럼 `sayHi` 함수를 클래스 내부에 정의하면 그 함수는 **프로토타입**에 저장된다. 즉, `Person.prototype` 에 저장이 되기 때문에 위에서 말한 문제는 클래스 문법을 사용함으로써 알아서 해결되는 것이다.

### 2-4. 클래스 내부에서만 사용되는 함수

기능 구현을 하다가 `Input` 클래스를 만들어서 내부에 유효성을 검사하는 메소드를 만들었다. 입력값과 관련이 있기 때문에 입력값을 set하기 전에 유효성을 검사하기 위해서다.

다음 코드를 보면 처음에 만든 `Input` 클래스이다.

> Input.js

```js
export default class Input {
  ...
  isValidCarNames(carNames) {
    /* 유효성 검증 코드 */
  }

  setCarNames(carNames) {
    if (this.isValidCarNames(carNames)) {
      this.carNames = carNames;
      return ;
    }
    this.carNames = null;
  }
  ...
}
```
위 코드를 보면 `setCarNames` 함수에서 유효성을 검증한 후에 값을 할당한다. 그렇게 유효성을 검증하기 위해서 사용하는 `isValidCarNames` 함수는 해당 클래스 내부에서만 사용된다. 이 클래스를 통해서 만들어진 객체를 사용할 때 클래스 외부에서 다음과 같이 사용할 수 있다는 말이다.
```js
const userInput = new Input();

userInput.isValidCarNames(['123','456']);
```
그런데 이렇게 사용하려고 만든 함수가 아니다. 객체가 전혀 사용할 일이 없고 함수 내부에서만 필요에 의해서 사용되는 함수이기 때문에 이 방법에 대해서 의문이 생겼다.

**정적 메서드**와 **프로토타입 메서드**의 차이는 프로토타입 체인이 다르다는 것이다. **정적 메서드**는 메서드 내부에서 클래스를 호출한 객체의 프로퍼티에 접근할 수 없다. 왜냐하면 객체가 호출할 수 없는 메서드이기 때문이다. 그래서 자바스크립트 Deep Dive에 따르면 '**this를 사용하지 않는 메서드는 정적 메서드로 정의하는 것이 좋다**'라고 한다.

이러한 이유로 위 코드를 다음과 같이 정적 메서드를 사용하는 방법으로 수정했다.

```js
export default class Input {
  ...
  static isValidCarNames(carNames) {
    /* 유효성 검증 코드 */
  }

  setCarNames(carNames) {
    if (Input.isValidCarNames(carNames)) {
      this.carNames = carNames;
      return ;
    }
    this.carNames = null;
  }
  ...
}
```

---

## 3. JavaScript

---
### 3-1. 클래스와 생성자 함수의 차이점

1. 클래스는 `new` 연산자 없이 호출하면 에러가 발생한다.
2. 클래스는 상속을 지원하는 `extends` 와 `super` 키워드를 제공한다.
3. 클래스는 호이스팅이 발생하지 않는 것처럼 동작한다. 하지만 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.
4. 클래스 내의 모든 코드에는 **strict mode**가 지정되어 실행되며 이를 해제할 수 없다.
5. 클래스의 `constructor`, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 `[[Enumerable]`]의 값이 `false`다. 다시 말해, 열거되지 않는다.

### 3-2. every

> every() 메서드는 배열 안의 모든 요소가 주어진 판별 함수를 통과하는지 테스트합니다. Boolean 값을 반환합니다.
>
> [MDN - every](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/every)

every는 callback이 거짓을 반환하는 요소를 찾을 때까지 배열에 있는 각 요소에 대해 한 번씩 콜백함수를 실행한다. 콜백함수에서 제시하는 조건에 만족하지 못하는 요소를 찾으면 바로 `false`를 반환한다. 즉, 배열의 모든 요소가 콜백함수의 조건을 만족할 때 `true`를 반환한다.

> 참고: 빈 배열에서 호출하면 무조건 true를 반환한다.

> Input.js

```js
  static isEmptyName(names) {
    return names.find((name) => name === '');
  }
```
입력값의 예외를 처리할 때 위와 같이 `find`함수를 사용했는데 `every`함수를 사용해서도 문제를 처리할 수 있다.


## 4. Git

---
### 4-1. rebase를 이용한 지난 커밋로그 수정


## References

- 모던 자바스크립트 Deep Dive (이웅모 저)
- [모던 자바스크립트 튜토리얼](https://ko.javascript.info/)
- [우아한형제들 기술블로그 - 생각하라, 객체지향처럼](https://techblog.woowahan.com/2502/)
- [함수 생성자와 클래스의 차이](https://uiyoji-journal.tistory.com/101)
