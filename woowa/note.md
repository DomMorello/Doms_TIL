구조: MVC 패턴에 대한 고민
주석을 아무데나 막 썼는데 주석없이 잘 읽히도록 해야된다고 해서 이름 고민 많이 함
기능담위 커밋에 대해서 고민을 많이 하고 나를 위한 커밋이 아닌 다른 사람을 위한 커밋을 남기기 위해 노력
클린코드 책

## 1. 숫자 야구 게임

우아한테크코스 3기의 프리코스 1주차 미션은 숫자 야구 게임 구현이다. 구현 내용은 [프리코스 1주차 미션 저장소](https://github.com/DomMorello/javascript-baseball-precourse/tree/dom)에 업로드했다.

1주차 미션을 진행하면서 고민한 내용들에 대해 개인적으로 공부하고 정리해보았다.

<br>

## 2. Javascript 기초

VanillaJS 를 충분히 공부하지 않고 React를 다루다 보니 JS에 대한 기본기 훈련이 잘 안 돼있다고 생각한다. 그래서 이번 프리코스를 통해서 부족했던 JS 기본 역량을 보완하는 것이 목표 중에 하나이다. **모던 자바스크립트 Deep dive** 와 여러 레퍼런스를 참고해서 잘 몰랐던 내용에 대해서 정리하면서 학습했다.

### 2-1. new와 생성자 함수

```js
export default function BaseballGame() {
  // this = {};  (빈 객체가 암시적으로 만들어짐)

  this.play = function (computerInputNumbers, userInputNumbers) {
    return "결과 값 String";
  };

  // return this;  (this가 암시적으로 반환됨)
}
```
위 코드는 미션에서 주어지는 코드인데 `this` 키워드를 이용해서 함수를 정의하고 있고 함수의 컨벤션이 대문자로 시작하는 것으로 보아 생성자 함수임을 알 수 있다.

new BaseballGame()과 같이 코드를 작성하면

1. 빈 객체를 만들어 this에 할당한다.
2. 함수 본문을 실행한다. 위의 경우 this에 새로운 프로퍼티(play 함수)를 추가해 this를 수정한다.
3. this를 반환한다.

<br>

### 2-2. this

```js
let user = {
  firstName: "dom",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi(); // dom
```
화살표 함수는 일반 함수와는 다르게 `this`를 고유하게 갖지 않는다. 위와 같이 화살표 함수 내부에서 `this`를 쓰면 더 상위의 범위에서 `this`를 참조한다. 위 경우에서는 user 객체를 가리킨다.

즉, 별개의 this가 만들어지는 건 원하지 않고, 외부 컨텍스트에 있는 this를 이용하고 싶은 경우 화살표 함수가 유용하다.

<br>

### 2-3. forEach 문에서 조건 만족시 break 하기

`forEach` 반복문을 사용해서 검사를 하면서 어떤 조건을 만족하면 반복을 멈추고 `break`나 `return`을 사용해서 나오고 싶을 것이다. 조건을 찾았는데 계속해서 반복문을 찾는 것은 무의미하기 때문이다.

```js
const a = [1,2,3,4];

a.forEach((element) => {
    console.log(element);
    if (element === 2)
        return ;
});

//outut
//1
//2
//3
//4
```
하지만 `forEach` 구문에서 어떤 조건을 만족했다고 해서 `return`을 해봤자 `forEach` 구문 내부의 콜백함수기 때문에 그 콜백함수만 빠져나오는 것이지 아무 의미가 없다. 위 코드를 실행해보면 1, 2 까지만 출력하고 3, 4는 실행하지 않을 것이라고 생각할 수도 있지만 이는 틀리다.

> 예외를 던지지 않고는 forEach()를 중간에 멈출 수 없습니다. 중간에 멈춰야 한다면 forEach()가 적절한 방법이 아닐지도 모릅니다.<br><br>
MDN – foreach

MDN 설명에 나와있듯이 `forEach` 에서 중간에 반복을 멈추고 나올 방법은 없다. 조건을 검색하는 반복에서는 이 함수를 사용하는 것이 적절하지 않다.

<br>

### 2-4. 옵셔널 체이닝 연산자
> ES11(ECMANSript2020)에서 도입된 옵셔널 체이닝 연산자 `?.` 는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.<br><br>
모던 자바스크립트 Deep Dive p.122

사용자의 입력값인 `userInput` 을 받아오는데 여기서 유효하지 않은 값이 들어오면 `userInput`이 `null`이 되도록 했다. 그런데 return을 할 때 `userInput.join("")` 을 하는데 여기서 `userInput`이 null 이면 에러가 나는 상황에 봉착했다.

그래서 return을 하기 전에 null 검사를 해야 하는 상황인데 여기서 옵셔널 체이닝 연산자를 쓰지 않으면 다음과 같이 했었어야 했다.

```js
export default function getUserInput() {
  let userInput = null;
  const userInputArray = {
    ...
    userInput = userInputArray;
  }
  return userInput && userInput.join("");
  //여기서 userInput이 null일 수도 있기 때문에 && 연산자를 통해 검사
}
```
그런데 옵셔널 체이닝 연산자를 이용하면 다음과 같이 수정할 수 있다.

```js
export default function getUserInput() {
  let userInput = null;
  const userInputArray = {
    ...
    userInput = userInputArray;
  }
  return userInput?.join("");
  //옵셔널 체이닝 연산자 사용
}
```
위 코드와 같이 **옵셔널 체이닝 연산자**를 사용하면 userInput이 null이거나 undefined이면 undefined를 반환하고 아니라면 우항의 프로퍼티를 참조한다.

<br>

## 3. 어떤 구조로 프로그래밍 해야 할까

### 3-1. 첫 설계의 착오

코드를 짤 때 항상 필요없는 행동을 최소화하기 위해서 노력한다. 그래서 이 게임의 시작은 항상 사용자의 입력이 있는 순간 시작된다는 사실을 인지하고 사용자의 입력을 받는 것을 최우선으로 코드를 짜기 시작했다.

그런데 처음에 구현할 기능 순서대로 목록을 작성했는데 개발을 하다 보니 로직이 잘못 됐다는 것을 깨달았다. 사용자 입력이 있을 때 컴퓨터의 입력을 생성하고 비교하는 게임 로직을 실행하려고 했는데 사용자 입력을 받는 이벤트가 발생할 때마다 그 후에 종속적으로 computerInput을 랜덤생성하면 계속해서 computerInput이 사용자 입력이 있을 때마다 바뀌기 때문에 정답을 맞출 가능성이 현저히 떨어지고 게임의 본질적인 기능을 하지 못한다는 사실을 알았다.

그래서 처음 설계한 로직을 수정하여 컴퓨터의 입력값이 먼저 있는 상태에서 사용자의 입력을 받고 비교하여 게임을 진행하는 로직이 다음으로 오도록 수정했다.

> playGame.js
```js
export default function playGame(play) {
  const $userSubmitButton = document.querySelector("#submit");
  const computerInput = getComputerInput();

  $userSubmitButton.addEventListener("click", (event) => {
    event.preventDefault();
    const userInput = getUserInput();
    console.log("test: ", computerInput, userInput);
  });
}
```
위 코드와 같이 컴퓨터 입력 값을 먼저 생성한 후에 사용자의 입력값을 받고 그 두 개의 입력값을 비교하여 게임 로직을 실행하는 순서로 수정했다.

<br>

### 3-2. 함수를 기능 단위로 나눠야 한다 vs 구조적으로 가능한 설계를 해야 한다

원래는 이렇게 구조를 생각했다. 기능별로 함수를 나눠야 하기 때문에 `getUserInput` 함수에서는 userInput의 값을 검사해서 유효하면 얻어오도록 하려고 했다. 그런데 이벤트가 발생하는 시점에 콜백함수를 실행하는데 위와 같이 코드를 짜면 userInput에는 항상 null값만 들어온다. 왜냐하면 다른 곳에서 이 함수를 호출해서 return 값을 받아서 사용하려고 했는데 함수를 실행하는 시점에는 `userInput`에 null값이 들어가있기 때문이다. 즉, 야구게임의 경우 사용자의 입력을 받는 순간 그 콜백함수에서 모든 게임 로직이 연속되게 실행돼야 한다는 것이다. 이벤트가 발생했을 때마다 새로운 값을 return 받으려고 했지만 구조적으로 불가능하다는 사실을 알아서 다음과 같이 구조를 바꿨다.

> getUserInput.js
```js
export default function getUserInput() {
  const $userSubmitButton = document.querySelector("#submit");

  $userSubmitButton.addEventListener("click", (event) => {
    event.preventDefault();
    const userInputArray = document
      .querySelector("#user-input")
      .value.split("")
      .map((elem) => +elem);
    if (alertErrorMessage(isValidInput(userInputArray))) {
      compareComputerUserInput(getComputerInput(), userInputArray); //콜백함수 안에서 다음 게임 로직을 연속해서 실행
    }
  });
}
```
위 코드를 보면 userInput 값을 반환하는 것이 아니라 이 함수 안에서 이벤트가 발생하면 입력값의 유효성을 검사한 후에 게임로직을 이 콜백함수 안에서 연속해서 실행하는 것이다. 

게임이 진행되는 방식이 이벤트의 발생에 따라서 결과값을 보여줘야 하기 때문에 이렇게 하는 것이 옳은 구조라고 생각해서 바꾸게 되었다. 

하지만 이런 식으로 구조를 만들면 `getUserInput` 이라는 함수 안에서 게임로직까지 실행되므로 한 기능을 가진 함수를 실행하는 원칙에 어긋나고 유지보수하기 어려운 코드가 될 것이다.

그래서 이벤트리스너를 게임을 시작하는 로직에 모아두고 그 안에서 userInput을 받아오면 한 가지 기능을 하는 함수를 만들 수 있다는 생각이 들었다.

> playGame.js
```js
export default function playGame(play) {
  const $userSubmitButton = document.querySelector("#submit");

  $userSubmitButton.addEventListener("click", (event) => {
    event.preventDefault();
    const userInput = getUserInput();
    // userInput만 받아오는 한 가지 기능을 하는 함수
  });
}
```
> getUserInput.js
```js
export default function getUserInput() {
  let userInput = null;
  const userInputArray = document
    .querySelector("#user-input")
    .value.split("")
    .map((elem) => +elem);
  if (alertErrorMessage(isValidInput(userInputArray))) {
    userInput = userInputArray;
  }
  return userInput;
}
```
위와 같이 `playGame` 함수에서 전체적인 이벤트리스너 구조를 만들어 놓고 그 콜백함수 안에서 userInput을 받아오면 `getUserInput` 함수는 userInput을 받아오는 한 가지 기능을 하는 함수로 탈바꿈이 가능했다. 이렇게 구조를 바꾸니까 한 가지 기능을 가진 함수를 만들 수 있었고 구조적으로도 충분히 가능한 코드를 만들 수 있었다.

<br>

### 3-3. 

입력 예외를 어느 시점, 어디에서 검사하고 예외를 호출해야 하는가? View or Domain? View에서 입력 예외를 검사 및 처리하는 로직이 Domain 내부 로직을 과도하게 관여 및 침범하는 것이 아닌가?

BaseballNumbers 도메인 객체는 중복되지 않은 1~9 사이의 임의의 3자리 수로 구성되는 생성 규칙을 가지고 있다. View에서 별도로 입력값 예외를 검사하지 않더라도, 잘못된 입력값이 들어온다면 도메인 객체 생성시 예외가 발생하긴 한다.

그렇다면 View에서는 입력값에 대한 예외 검증 및 처리가 불필요한가? 프로그램 사이즈가 작더라도, View는 View대로 Domain은 Domain대로 예외 검사 및 처리를 모두 잘 수행해야 좋은 프로그램이라고 생각한다.

예를 들어, Spring으로 웹 어플리케이션에서 회원가입 기능을 구현한다고 해보자. 회원 정보가 담기는 User 도메인 객체에 잘못된 패턴의 입력값(이메일, 비밀번호 등)이 들어오면 @Valid를 통해 BindingException 예외가 발생한다. @ControllerAdvice가 적합한 ExceptionHandler를 호출하여 예외를 처리한다.

이처럼 도메인 단에서 예외를 검사 및 처리하지만, 정규식 및 HTML 기능을 이용해 일차적으로 프론트엔드 단(View 페이지)에서 입력값에 대한 유효성 검사(대소문자 및 특수문자 포함 여부 등의 패턴 검사 혹은 공백 검사 등)를 진행함으로써 예외를 꼼꼼하게 처리할 수 있다.

예외 검증 및 처리를 어느 수준으로 할지는 전적으로 개발자에게 달려있지만, Domain과 View 모두 꼼꼼하게 예외를 처리해주는 것이 좋은 개발 습관이라고 생각한다.

> InputView.java

```java
private void validateInputBaseballNumbers(String inputBaseballNumbers) {
    if (inputBaseballNumbers.length() != VALID_INPUT_BASEBALL_NUMBER_COUNTS) {
        throw new IllegalArgumentException();
    }
    boolean isDuplicated = inputBaseballNumbers.chars()
            .filter(number -> MINIMUM_BASEBALL_NUMBER <= number && number <= MAXIMUM_BASEBALL_NUMBER)
            .distinct()
            .count() != VALID_INPUT_BASEBALL_NUMBER_COUNTS;
    if (isDuplicated) {
        throw new IllegalArgumentException();
    }
}
```

View에서 입력값 예외 검사를 진행하기 위해서는, BaseballNumbers라는 객체의 생성 조건(중복되지 않은 1-9 사이의 임의의 수 3자리)을 이용하게 된다. 처음에는 이러한 View의 예외 처리가, Domain 객체의 내부 정보 및 자율성을 과도하게 침해하는 것이 아닌지 의문이 들었다.

하지만 View에서 입력 예외 검사 및 처리를 하기 위해서는, Domain 객체의 내부 정보 및 로직(객체 생성에 사용되는 데이터의 형태 등 규칙)이 View에 필연적으로 포함될 수 밖에 없다고 결론을 내렸다.

### 3.2. 입력 예외 검사 및 처리 : 재입력 여부

입력 예외가 발생하면 단순히 예외를 호출하는 것으로 충분한가? View에서 try-catch를 통해 예외를 처리하고, 올바른 값을 다시 재입력받도록 코드를 짜야하는가?

대부분의 웹 어플리케이션은 로그인 입력값이 잘못되었다고 웹 앱이 종료되지 않고, 오히려 안내 문구와 함께 회원 정보를 재입력할 수 있도록 유저 편의를 봐준다.

입력값 예외가 발생했을 때, 별도의 처리 없이 프로그램이 종료되게 할지 혹은 경고 문구와 함께 다시 재입력을 받도록 할지는 ***3.1.*** 절과 마찬가지로 개발자에게 달려있다.

> InputView.java

```java
public List<Integer> inputBaseballNumbers() {
    System.out.print(INPUT_BASEBALL_NUMBERS_MESSAGE);
    String inputBaseballNumbers = scanner.nextLine();
    while (!isValidInputBaseballNumbers(inputBaseballNumbers)) {
        inputBaseballNumbers = scanner.nextLine();
    }
    return inputBaseballNumbers.chars()
            .map(numberCharacter -> numberCharacter - CHAR_TO_INT_CONVERTER_ASCII_CHARACTER)
            .boxed()
            .collect(Collectors.toList());
}

private boolean isValidInputBaseballNumbers(String inputBaseballNumbers) {
    try {
        validateInputBaseballNumbers(inputBaseballNumbers);
        return true;
    } catch (IllegalArgumentException e) {
        System.out.print(WRONG_INPUT_MESSAGE);
        return false;
    }
}

private void validateInputBaseballNumbers(String inputBaseballNumbers) {
    if (inputBaseballNumbers.length() != VALID_INPUT_BASEBALL_NUMBER_COUNTS) {
        throw new IllegalArgumentException();
    }
    boolean isDuplicated = inputBaseballNumbers.chars()
            .filter(number -> MINIMUM_BASEBALL_NUMBER <= number && number <= MAXIMUM_BASEBALL_NUMBER)
            .distinct()
            .count() != VALID_INPUT_BASEBALL_NUMBER_COUNTS;
    if (isDuplicated) {
        throw new IllegalArgumentException();
    }
}
```

숫자 야구 미션 프로그래밍 요구사항에 예외가 발생한 경우 재입력을 받게 하라는 별도의 명시 사항은 없었다. 하지만 유저 편의성을 위해 입력 예외를 try-catch로 잡고 while을 통해 재입력을 받을 수 있도록 했다.

### 3.3. 입력 예외 검사 및 처리 : Domain 예외

발생 가능한 모든 입력 예외 케이스를 View에서 try-catch로 완벽하게 처리했다면, 입력값을 활용하는 비즈니스 로직에서 발생 가능한 예외는 try-catch로 처리하지 않아도 되는가?

> BaseballNumbers.java

```java
public static BaseballNumbers generateInputBaseballNumbers(List<Integer> inputBaseballNumbers) {
    validateDuplication(inputBaseballNumbers);
    List<BaseballNumber> baseballNumbers = inputBaseballNumbers.stream()
            .map(BaseballNumber::valueOf)
            .collect(Collectors.toList());
    return new BaseballNumbers(baseballNumbers);
}

private static void validateDuplication(List<Integer> inputBaseballNumbers) {
    int distinctCounts = (int) inputBaseballNumbers.stream()
            .distinct()
            .count();
    if (distinctCounts != NECESSARY_BASEBALL_NUMBER_COUNTS) {
        throw new IllegalArgumentException();
    }
}
```

BaseballNumbers 객체는 정수 리스트를 파라미터로 받아 생성할 수 있다. 이 때 정수 리스트가 중복되지 않은 1~9 사이의 임의의 수 3개로 구성되어 있지 않다면 예외가 발생한다.

> Application.java

```java
BaseballNumbers userBaseballNumbers =
          BaseballNumbers.generateInputBaseballNumbers(inputView.inputBaseballNumbers());
```

메인 어플리케이션에서 유저가 입력한 값을 통해 BaseballNumbers 객체를 생성한다. InputView에서 발생 가능한 모든 입력 예외 케이스를 완벽하게 처리하고 있다면, ``inputBaseballNumbers()`` 메서드는 중복되지 않은 1~9 사이의 임의의 수 3개 리스트를 항상 정상적으로 반환한다. 따라서 BaseballNumbers 객체를 생성할 때 중복 예외가 절대 발생하지 않을 것이다.

그렇다면 해당 비즈니스 로직 부분은 try-catch로 예외를 명시적으로 처리할 필요가 없을까?

Spring 어플리케이션은 @ControllerAdvice 및 Logger를 통해 try-catch 없이 발생한 예외 처리 및 로그 기록을 자동화할 수 있다.

반면 숫자 야구같은 콘솔 게임 프로그램같은 현재의 경우는, 결국 예외 처리를 어느 수준으로 할지에 대한 문제로 귀결되며 전적으로 개발자에게 달려있다.

> Application.java

```java
public class Application {
    public static void main(String[] args) {
        final Scanner scanner = new Scanner(System.in);
        InputView inputView = new InputView(scanner);
        BaseballGameMachine baseballGameMachine =
                new BaseballGameMachine(BaseballNumbers.generateRandomBaseballNumbers());
        GameState gameState = GameState.initiate();
        while (gameState.isAbleToPlay()) {
            try {
                baseballGameMachine = baseballGameMachine.prepareNextTry(gameState);
                BaseballNumbers userBaseballNumbers =
                        BaseballNumbers.generateInputBaseballNumbers(inputView.inputBaseballNumbers());
                GameResult gameResult = baseballGameMachine.play(userBaseballNumbers);
                OutputView.outputGameResult(gameResult);
                gameState = GameState.findGameState(inputView.inputGameState(gameResult));
            } catch (RuntimeException e) {
                System.out.println(e.getMessage());
            }
        }
        scanner.close();
    }
}
```

굳이 도메인의 비즈니스 로직에서 발생할 수 있는 예외를 처리하고자 한다면, Application의 메인 코드에서 try-catch를 위처럼 삽입할 수 있겠다.

다만 Indent(depth)가 2로 증가하고, 가독성이 나쁘기 때문에 맘에 드는 구조는 아니다. 어차피 RuntimeException은 Unchecked Exception이기 때문에, 상황과 필요성을 고려하여 처리하면 될 것 같다. 나는 도메인의 비즈니스 로직에 대한 예외 처리는 하지 않는 것으로 결정했다.

> BaseballNumberDuplicationException.java

```java
public class BaseballNumberDuplicationException extends RuntimeException {
    private static final String ERROR_MESSAGE = "숫자가 중복되었습니다. (%d)";

    public BaseballNumberDuplicationException(int baseballNumber) {
        super(String.format(ERROR_MESSAGE, baseballNumber));
    }
}
```

View는 View대로 Domain은 Domain대로 예외 처리와 검증이 필요하다고 생각하지만, 현재 규모의 프로그램에서는 View에서 예외 처리를 제대로 하는 것과 Domain 관련 커스텀 예외 선언만으로도 충분하다고 생각한다.

### 3.4. try-catch Recursion vs While Loop

> InputView.java

```java
public List<Integer> inputBaseballNumbers() {
    System.out.print(INPUT_BASEBALL_NUMBERS_MESSAGE);
    String inputBaseballNumbers = scanner.nextLine();
    while (!isValidInputBaseballNumbers(inputBaseballNumbers)) {
        inputBaseballNumbers = scanner.nextLine();
    }
    return inputBaseballNumbers.chars()
            .map(numberCharacter -> numberCharacter - CHAR_TO_INT_CONVERTER_ASCII_CHARACTER)
            .boxed()
            .collect(Collectors.toList());
}

private boolean isValidInputBaseballNumbers(String inputBaseballNumbers) {
    try {
        validateInputBaseballNumbers(inputBaseballNumbers);
        return true;
    } catch (IllegalArgumentException e) {
        System.out.print(WRONG_INPUT_MESSAGE);
        return false;
    }
}
```

현재는 입력 예외가 발생하면 try-catch로 잡고, while 반복문을 통해 재입력을 받는다.

> InputView.java

```java
public List<Integer> inputBaseballNumbers() {
    System.out.print(INPUT_BASEBALL_NUMBERS_MESSAGE);
    String inputBaseballNumbers = getInputBaseballNumbers();
    return inputBaseballNumbers.chars()
            .map(numberCharacter -> numberCharacter - CHAR_TO_INT_CONVERTER_ASCII_CHARACTER)
            .boxed()
            .collect(Collectors.toList());
}

private String getInputBaseballNumbers() {
    try {
        String inputBaseballNumbers = scanner.nextLine();
        validateInputBaseballNumbers(inputBaseballNumbers);
        return inputBaseballNumbers;
    } catch (IllegalArgumentException e) {
        return getInputBaseballNumbers();
    }
}
```

예외가 발생했을 때 catch에서 메서드를 재귀 호출하면, 짧은 코드로 while 반복문 효과를 낼 수 있다.

그러나 여러 레퍼런스를 참고해보니, 예외를 통해 프로그램의 정상적인 흐름을 제어하는 것은 나쁜 프로그래밍 습관이라고 한다. 또한 재귀 호출의 특성상, 수 많은 잘못된 입력값 공격이 들어오면 StackOverFlowErrorr가 발생할 수 있다. 따라서 반복이 필요하다면 while을 사용하자.

+ 추가 (2020/12/03)

[관련 아티클](https://woowacourse.github.io/javable/2020-04-30/iteration_vs_recursion)에 내가 고민했던 내용이 명쾌하게 정리되어 있었다.

* 재귀 장점
  * 반복 구조를 단순한 로직으로 구현이 가능함.
  * 변수와 코드의 길이가 줄어 가독성이 상승함.
  * 컴파일러가 꼬리 재귀 최적화를 지원하면 재귀의 단점(메모리, 성능)을 보완할 수 있음.
* 재귀 단점
  * 콜 스택이 쌓여서 StackOverFlow 발생할 수 있음.
  * 스택을 구성하고 해제하는 과정에서 반복문보다 오버헤드가 들기 때문에 성능이 떨어짐.
* 결론
  * 성능이 중요한 과거에는 반복문이 당연했지만, 하드웨어가 발전한 오늘 날에는 성능보다 협업이 강조되는 만큼 가독성(재귀)를 고려할 필요가 있다.
    * 특히 컴파일러가 꼬리 재귀의 최적화를 지원해주면 성능까지 얻을 수 있다.
  * 그러나 StackOverFlow를 무시할 수 없으니, 이왕이면 깊이가 예측가능한 경우에 사용하는 것이 안전하고 현명하다.

꼬리 재귀에 대한 개념은 어렵지 않으니 해당 원문 링크를 참고하길 바란다. 궁금해서 찾아봤는데, [Java 8은 꼬리 재귀 최적화를 지원하지 않는다고 한다.](https://stackoverflow.com/questions/22866491/does-java-8-have-tail-call-optimization)

<br>

## 4. equals 오버라이드

코드를 짜면서 생각나는 김에 Effective Java의 **equals는 일반 규약을 지켜 재정의하라** 및 **equals를 재정의하려거든 hashCode도 재정의하라** 챕터들을 복습했다.

> BaseballNumber.java

```java
public class BaseballNumber {
    public static final int RANGE_MINIMUM = 1;
    public static final int RANGE_MAXIMUM = 9;
    private static final Map<Integer, BaseballNumber> CACHE = new HashMap<>();

    private final int baseballNumber;

    private BaseballNumber(int baseballNumber) {
        this.baseballNumber = baseballNumber;
    }

    public static BaseballNumber valueOf(int baseballNumber) {
        validateBaseballNumberRange(baseballNumber);
        return CACHE.computeIfAbsent(baseballNumber, BaseballNumber::new);
    }

    private static void validateBaseballNumberRange(int baseballNumber) {
        if (baseballNumber < RANGE_MINIMUM || baseballNumber > RANGE_MAXIMUM) {
            throw new IllegalArgumentException();
        }
    }
}
```

BaseballNumber 객체는 캐시를 사용하기 때문에 1번 ~ 9번 등 총 9개의 객체만 존재한다. 프로그램 로직 중, 두 개의 BaseballNumber 리스트의 원소가 동일한지 여부를 체크하여 스트라이크와 볼 개수를 계산한다.

처음에는 내부 int 값을 비교하기 위해 ``eqauls()``를 오버라이드 했지만, 그럴 필요가 없음을 느끼고 상속받은 Object의 ``equals()``를 그대로 사용하게 되었다.

``equals()`` 메서드는 같은 객체의 참조 여부는 중요하지 않고 객체가 가지는 값이 논리적으로 같은지 확인이 필요할 때 오버라이드를 한다. 특히 값(Value) 클래스에서 Map의 키나 Set의 요소로 객체를 저장하려고 사용하려면, 같은 값의 객체가 이미 있는지 비교해야 하기 때문에 ``equals()`` 및 ``hashCode()``의 오버라이드가 필수이다.

BaseballNumber는 싱글톤과 유사하게 각 값(1 ~ 9) 당 최대 하나의 객체만 존재하도록 인스턴스를 제어하고 있다. 사실상 논리적인 일치(값 비교)와 객체 참조 일치가 동일한 의미이기 때문에 == 연산을 통해 레퍼런스 주소값을 비교하는 Object의 ``equals()``가 논리적인 ``equals()``를 수행한다.

<br>

## 5. 구현 기능 목록 정리

처음 미션을 시작하면서 README 파일에 구현 기능을 정리하면서, "어느 정도로 상세하게 정리해야 하지?" 고민하면서 지우고 쓰기를 반복했다.

처음에는 핵심 클래스 및 세부 메서드까지 설계해서 정리하는데 많은 시간을 쏟았다. 그러나 처음부터 너무 세세한 구현 기능 목록 정리에 힘을 쏟는 것이 비효율적이라고 느꼈다. 개발을 진행하다 보면, 처음에 생각했던 클래스/객체 설계 및 메서드 로직을 불가피하게 수정하는 경우가 많았기 때문이다.

처음부터 너무 완벽한 설계에 집착하기 보다는, 정상 및 비정상(예외) 관련 핵심 기능들을 리스트업하고 개발하면서 주기적으로 목록을 업데이트했다.

완벽해야 한다는 강박관념을 항상 경계하자!

<br>

## 6. Commit Message : 한글 vs 영어

관습적으로 Commit 메시지를 영어로 써왔는데, 이번에 미션을 진행하면서 내 Commit 메시지들을 보니 부족함이 많이 느껴졌다.

![image](https://user-images.githubusercontent.com/56240505/100646024-d65a3a80-3380-11eb-8e4f-2b19b96cdba4.png)

세세하고 명확하게 Commit 메시지를 작성하려고 노력했는데, 모국어가 아니다보니 의도한 바와 다르게 불명확 및 장황한 메시지들이 많았다. 작성 시점에는 괜찮다고 생각했던 메시지들도 추후에 다시 읽을 때는 한 번에 이해 되지 않았다.

또한 처음에는 Body에도 '왜, 무엇을 변경했는지'에 대해 열심히 서술했는데, 뒤로 갈수록 Body를 생략한 Commit 메시지들이 많았다.

Commit 메시지를 나 혼자 읽는 것이라면 상관 없으나, 협업 환경에서는 팀원들에게 꽤 불편할 것이라는 생각이 든다. 협업 팀마다 Commit 메시지 규칙이 다르겠지만, 한국인들하고 작업하는게 명확한 경우 한글 Commit 메시지가 가독성이 조금 더 나을 것 같다.

다음 2주차 미션 때는 Commit 메시지를 한글로 작성하면서 차이점을 느껴 보고, 어떤 방식이 나한테 더 맞는지 고민해 봐야겠다.

<br>

## 7. Test Method Name : 한글 vs 영어

예전에는 테스트 메서드 이름도 영어로 명명했는데, Camel 표기법을 사용하다보니 ``isStrikeThrowsExceptionWhenParameterIsOutOfRange`` 같이 메서드 명이 길어지는 경우 가독성이 심하게 떨어졌다.

[[Java] 인기있는 Unit Test 네이밍 규칙](https://hilucky.tistory.com/216) 글을 참고했다. Given-When-Then을 구분해서 Snake 표기법으로 ``MethodName_StateUnderTest_ExpectedBehavior``와 같이 네이밍하니 가독성이 좋아졌다.

다만 스네이크 표기법을 사용하더라도, 영어를 통해 메서드 이름을 작성하다 보면 Commit 메시지처럼 장황해지는 경우가 많았다. 테스트 메서드 이름을 한글로 작성하면 어떨까?

영어보다 짧아져서 가독성도 증가하고, 테스트가 실패했을 때 어느 기능에서 문제가 발생했는지 직관적으로 알 수 있다는 장점이 있다. 또한 실제 배포시에 테스트는 컴파일에 포함되지 않기 때문에 문제가 되지 않는다.

![image](https://user-images.githubusercontent.com/56240505/100697537-9d9a7f80-33d9-11eb-8f95-a83eedcd7957.png)

다만 한글을 사용하면서 다음과 같은 불편한 점들이 있었다.
* 한글 특성상 Camel 케이스를 적용하기 힘들기 때문에 _를 많이 사용함.
* 네이밍 일관성 부족. (User -> 유저, 회원, 사용자 등)
* 코드 검색시에 누락됨.

특별한 팀 규칙이 없다면, 자기가 보기 편한 방식으로 작성하면 될 듯 싶다. 나는 테스트 메서드 이름을 Given-When-Then 형식으로 작성하는데 한글과 Snake 표기법을 사용했다.

테스트 메서드 이름만으로 테스트의 의도 및 정보를 충분하게 표현하기 어려운 경우가 종종 있다. 자세하게 쓰려고 하면 메서드 명이 너무 길어지기 때문이다. 이럴 때는 @DisplayName을 사용해서 테스트에 대한 추가 정보를 명시해준다.

<br>

## 8. 정적 팩토리 메서드 생성 기준

Effective Java에 나와있는 정적 팩토리 메서드의 장점 중 하나는 가독성이 좋다는 것이다. 메서드 이름을 가질 수 있어서, 일반적인 생성자와 비교했을 때 반환 객체를 잘 묘사할 수 있다.

> BaseballNumbers.java

```java
public static BaseballNumbers generateRandomBaseballNumbers() {
    Set<Integer> randomNumbers = new HashSet<>();
    while (!isGenerationComplete(randomNumbers)) {
        int randomNumber = RandomUtils.nextInt(BaseballNumber.RANGE_MINIMUM, BaseballNumber.RANGE_MAXIMUM);
        randomNumbers.add(randomNumber);
    }
    List<BaseballNumber> baseballNumbers = randomNumbers.stream()
            .map(BaseballNumber::valueOf)
            .collect(Collectors.toList());
    return new BaseballNumbers(baseballNumbers);
}

public static BaseballNumbers generateInputBaseballNumbers(List<Integer> inputBaseballNumbers) {
    validateDuplication(inputBaseballNumbers);
    List<BaseballNumber> baseballNumbers = inputBaseballNumbers.stream()
            .map(BaseballNumber::valueOf)
            .collect(Collectors.toList());
    return new BaseballNumbers(baseballNumbers);
}
```

다양한 방식으로 객체를 생성해야 하는 경우(생성자가 여러개인 경우), 정적 팩토리 메서드를 통해 객체 생성 의도 및 방식을 상세하게 표현할 수 있어서 좋다.

> Application.java

```java
//Before
BaseballGameMachine baseballGameMachine =
        new BaseballGameMachine(BaseballNumbers.generateRandomBaseballNumbers());

//After
BaseballGameMachine baseballGameMachine = BaseballGameMachine.initiate();
```

처음에는 위 코드를 보고 ``BaseballGameMachine.initiate()``가 의미를 드러낼 수 있어서 new 생성자보다 좋다고 생각했다. 사실 정적 팩토리 메서드는 new 연산자보다 가독성이 좋을 수 밖에 없다. 이름을 가지고 있기 때문이다.

하지만 BaseballGameMachine은 생성자가 1개이며, 파라미터 또한 1개밖에 받지 않는 비교적 생성이 간단한 객체이다. 내가 가독성이라는 미명 하에 간단한 객체 생성마저도 new 생성자가 아닌 정적 팩토리 메서드를 관습적으로 사용하고 있음을 느꼈다.

정적 팩토리 메서드를 관습적으로 남발하기 보다는, 정말로 필요한 상황에 대해 고려해볼 필요가 있다고 생각했다.

일단 정적 팩토리는 다음과 같은 단점이 존재한다.
* 정적 팩토리 메서드만 있는 클래스라면, 생성자가 없으므로 하위 클래스를 못 만든다.
* 정적 팩토리 메서드는 다른 정적 메서드와 잘 구분되지 않는다. (문서만으로 확인하기 어려울 수 있음.)

그렇다면 어떤 경우에 정적 팩토리 메서드 사용을 고려해야 할까? 명확한 기준은 없지만, 예전부터 느낀 점들을 토대로 몇 가지 기준을 세워보았다.

* 생성자가 2개 이상인 경우.
* 생성자에 들어가는 파라미터가 많은 경우.

이 두 가지 경우는 객체의 생성 의도를 쉽게 유추하기 어렵기 때문에, 생성 의도를 명확하게 표현하기 위해 정적 팩토리 메서드를 사용한다.

따라서 BaseballGameMachine과 같이 1개의 생성자 밖에 없고, 비교적 간단한 파라미터로 생성이 가능한 경우 정적 팩토리 메서드가 꼭 필수라고 생각되지 않는다. 이후, 이런 기준에 따라서 불필요하고 과도한 정적 팩토리 메서드들을 리팩토링했다.

<br>

---

## References

* [how to call recursively a method in a try-catch block](https://stackoverflow.com/questions/21783914/how-to-call-recursively-a-method-in-a-try-catch-block)
* [When to use mutable objects?](https://stackoverflow.com/questions/44772949/when-to-use-mutable-objects)
* [(이펙티브 자바 3판) 3장 - 모든 객체의 공통 메서드, equals는 일반 규약을 지켜 재정의하라](https://perfectacle.github.io/2018/11/26/effective-java-ch03-item10-equals-method/)
* [8) equals 메서드를 오버라이딩 할 때는 보편적 계약을 따르자](http://egloos.zum.com/hahaha333/v/3906967)
* [[Java] 인기있는 Unit Test 네이밍 규칙](https://hilucky.tistory.com/216)
* [JUnit5 @Nested, @DisplayName 활용](https://www.freeism.co.kr/wp/archives/1061)
* [반복문(iteration) vs 재귀(recursion)](https://woowacourse.github.io/javable/2020-04-30/iteration_vs_recursion)
* [Does Java 8 have tail call optimization?](https://stackoverflow.com/questions/22866491/does-java-8-have-tail-call-optimization)
* Effective Java 3/E (Joshua Bloch 저)