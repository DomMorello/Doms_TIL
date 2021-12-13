피드백 내용:
- 상수를 선언해서 사용하긴 했는데 정작 중요한 의미를 담을 수 있는 숫자 부분에서도 상수를 선언해서 하도록 신경써야 해야 한다.
- 불필요한 변수를 줄이기 위해 노력해라(어떤 변수를 갖고 있는 이를 통해 얻어낼 수 있는 값이라면 함수로 분리하는 것이 더 좋다)

후기:
- 전반적인 리팩토링 과정에서 의미있는 커밋로그를 남기는 것이 매우 어려웠다.

TODO: 
탭핸들러 함수 나눠야 한다.
상품추가 이름 중복 검사

## 1. 자판기

우아한테크코스 4기의 프리코스 3주차 미션은 자판기 구현이다. 구현 내용은 [프리코스 3주차 미션 저장소](https://github.com/DomMorello/javascript-vendingmachine-precourse/tree/dom)에 업로드했다.

3주차 미션을 진행하면서 고민한 내용들에 대해 개인적으로 공부하고 정리해보았다.

<br>

## 2. 구현 중에 했던 고민들

---

### 2-1. 클래스가 서로 어떻게 상호작용할 것인가?

**여러 개의 클래스를 분리한 후 서로 관계를 맺어 하나의 프로그램을 완성**하는 것이 3주차 미션의 목표이다. 클래스를 어떻게 **객체지향**적으로 분리하고 어떻게 서로 관계를 맺을지를 고민해서 적용했다. 기본적으로 모든 프리코스 과제와 같이 **MVC 패턴**을 유지하려고 노력했다.

...

### 2-2. 비즈니스 로직과 UI 를 분리하라
1,2주차 과제를 진행하면서 **MVC 패턴**을 유지하려고 노력했다. 특히 디렉토리 구조를 명확하게 나눠서 input, view 디렉토리를 두고 UI를 모두 view 디렉토리에 넣고 진행했다. 그런데 2주차 피드백을 보고 이렇게 하는 것이 답은 아니라는 것을 느꼈다.

![image](./feedback.png)

피드백 내용을 살펴보면 위 사진과 같다. 내가 지금까지 했던 방식과는 다르게 `Car` 클래스 내부에서 UI 로직을 갖고 있는 것을 볼 수 있다. 이 피드백을 받고 지금까지 했던 방식과는 다르게 해보려고 생각했다. 우선 `Car`와 밀접하게 관련된 UI 를 보여주는 함수가 `Car` 클래스 내부에 선언돼 있는 것이 합리적이다. 내가 기존에 했던 방식으로 했을 때는 **View 클래스에 서로 관련 없는 UI 로직이 다 뭉쳐져 있었다**. 이런 부분은 확실히 그 해당 클래스에 역할이 무엇인지를 명확하게 밝힐 수 없는 구조인 것 같다.

...

### 2-3. innerHTML vs insertAdjscentHTML
...
(https://oniondev.tistory.com/17)
(https://developer.mozilla.org/ko/docs/Web/API/Element/insertAdjacentHTML)

### 2-4. UI에 보여질 상태를 어떻게 관리할 것인가?
이번 주차 과제 요구사항 중 하나는 '**새로고침을 하여도 최근 작업내역이 보여야 한다**'이다. 이는 `localStorage`를 이용해서 마치 백엔드의 DB에 저장된 값을 불러와서 화면을 보여주는 것과 같은 효과를 낼 수 있다. 그런데 어떤 이벤트가 발생했을 때 어떤 클래스의 메서드를 이용해서 뷰를 보여주면 새로고침을 했을 때는 조작된(추가된) DOM 정보가 모두 사라지기 때문에 최근 작업내역을 보여줄 수가 없다.

그래서 이렇게 생각했다. 새로고침을 하지 않고 프로그램을 사용자가 계속 조작하는 경우와 새로고침을 했을 때의 경우를 나눠서 생각했다. 사용자가 새로고침을 하지 않고 입력값을 넣고 하는 등의 문제는 각 클래스에 있는 `render()` 메서드를 사용해서 DOM을 조작하면 된다. 그러면 UI에 보여줄 상태 데이터들을 최신의 상태로 잘 볼 수 있을 것이다.

그렇다면 새로고침을 했을 때는 어떻게 최근 작업내역을 보여줄까? `localStorage`에 최근까지 작업한 내용을 전부 불러와서 view를 init할 때 보여주면 된다. 

...

### 2-5. DOM append 이후 참조에 대한 문제
DOM 노드를 만들어서 보여줄 때 어떤 문제를 발견했다. 이러한 문제가 발생하는 원인에 대해서 깊이있게 공부하진 못했지만 합리적인 추측이 가능했다. 문제는 다음과 같다.

> initProductAdd.js

```js
function renderProductAddList($productAdd) {
  const $listContainer = document.createElement('div');

  $listContainer.innerHTML = `
    <h3>${PRODUCT_LIST_TITLE}</h3>
    <table id="${PRODUCT_LIST_TABLE_ID}" bgcolor="black" border="1" style="border-collapse:collapse;">
      <tr align="center" bgcolor="white" height="40">
        <td align="center" width="160">${PRODUCT_NAME_TITLE}</td>
        <td align="center" width="100">${PRODUCT_PRICE_TITLE}</td>
        <td align="center" width="100">${PRODUCT_QUANTITY_TITLE}</td>
      </tr>
    </table>
  `;
  $productAdd.append($listContainer);
  console.log('test: ', document.querySelector(`#${PRODUCT_LIST_TABLE_ID}`));
  ...
}
```
위 코드를 보면 `innerHTML` 함수를 이용해서 새로운 `<table>`을 만들어서 새로 생성한 `<div>` 에 `append`해서 화면을 구성한다.

그런데 쉽게 생각했을 때 코드가 순서대로 실행되면서 DOM 노드를 만들어서 조작 과정을 전부 마치고 `console.log()`로 생성된 DOM 노드를 참조해서 출력하는 것이니까 정상적으로 작동할 것처럼 보인다.

하지만 결과는 `test: null` 이 출력된다. 즉, DOM 노드를 생성해서 조작하는 과정을 다 거쳤지만 그 직후 참조를 하려고 하면 해당 노드를 참조할 수 없다.

여기서 신기한 점은 만약에 `setTimeout` 함수를 이용해서 `console.log()` 부분을 지연실행시키면 어떻게 될까?

```js
function renderProductAddList($productAdd) {
  const $listContainer = document.createElement('div');

  $listContainer.innerHTML = `
    <h3>${PRODUCT_LIST_TITLE}</h3>
    <table id="${PRODUCT_LIST_TABLE_ID}" bgcolor="black" border="1" style="border-collapse:collapse;">
      ${productListHeaderTemplate()}
    </table>
  `;
  $productAdd.append($listContainer);
  setTimeout(() => {
    console.log('test: ', document.querySelector(`#${PRODUCT_LIST_TABLE_ID}`));
  }, 0);
  ...
}
```
위 코드에서 변경된 부분은 `console.log()` 부분이 `setTimeout` 함수 안으로 이동한 것 뿐이다. 이렇게 수정한 결과 정상적으로 콘솔에 해당 DOM node를 참조할 수 있었다.

이렇게 되는 이유를 깊이 있게 탐구하지는 못했지만 다음과 같을 것이다.

JS는 코드를 한 줄 한 줄 내려가면서 실행한다. `innerHTML` 함수를 실행하고 `append`함수를 실행한다. 그렇게 정상적으로 DOM node가 생성되었을 것이다. 하지만 그 직후 바로 해당 DOM node를 참조하면 참조할 수가 없다. 아마도 **document.querySelector가 실제로 브라우저에 렌더링된 DOM 노드를 참조하려고 하는데 아직 렌더링과정까지는 완료가 안 됐기 때문**이다.

브라우저는 DOM 을 파싱하고 렌더링하는 일련의 과정을 거치는데 그 과정에 시간이 걸리는 것이다. 그래서 `setTimeout`으로 지연 실행시켰을 때는 정상적으로 작동한 것이다. 

그렇다면 이 문제를 어떤 방식으로 해결해야 할까?

위와 같이 `setTimeout`으로 지연실행시키면 되겠지만 정확히 어떤 식으로 문제가 해결돼서 동작하는지 확신이 없기 때문에 더 안전한 방법을 찾아야 한다.

위의 코드에서 `querySelector`로 DOM 노드를 참조하려는 이유는 `localStorage`에 있는 데이터를 가져와서 있으면 보여주기 위해서였다. 그런데 이 의도를 다른 방법으로 구현하면 문제 해결이 가능했다.

**template literal안에서 map 함수를 통해 배열을 만든 후 `join`함수로 문자열로 만들면 된다.**

```js
function productListTemplate({ name, price, quantity }) {
  return `
    <tr align="center" bgcolor="white" height="40">
      ...
    </tr>
  `;
}

function renderProductAddList($productAdd) {
  const $listContainer = document.createElement('div');
  const products = JSON.parse(localStorage.getItem(PRODUCTS_STORAGE_KEY))
    ?.map((product) => productListTemplate(product))
    .join('');

  $listContainer.innerHTML = `
    <h3>${PRODUCT_LIST_TITLE}</h3>
    <table id="${PRODUCT_LIST_TABLE_ID}" bgcolor="black" border="1" style="border-collapse:collapse;">
      ${productListHeaderTemplate()}
      ${products || ''}
    </table>
  `;
  $productAdd.append($listContainer);
}
}
```
위 코드를 보면 `localStorage`에서 데이터를 가져와서 하나의 문자열로 만들어 `products`라는 변수에 저장한다.
그 다음에 그것을 그냥 **template literal 안에 추가**하는 것으로 의도를 정확하게 구현할 수 있었다.

### 2-6. 탭 구현에 대한 고찰
탭을 구현하기 위해서 이런 방법을 생각했다. 처음에 모든 DOM 을 생성하여 렌더링한다. 그런데 여기서 기본값으로는 상품 추가 탭 화면만 먼저 보여주고 잔돈 추가, 상품 구매 뷰는 `hidden` 속성을 이용해서 가려두는 것이다. 이렇게 하면 탭을 누를때마다 그 탭에 해당하는 전체 뷰를 다 지우고 다시 생성해서 보여줄 필요가 없기 때문이다.

간략하게 설명하자면, 
1. 상품 구매 뷰를 보여준다.
2. 잔돈 추가 뷰를 보여준다.
3. 상품 구매 뷰를 보여준다.
4. 세 개의 뷰를 `hidden` 속성으로 가린다.
5. 현재 탭에 대한 뷰만 `hidden` 속성을 지워 보여준다.

이렇게 하는 것이 탭을 누를때마다 새로운 뷰를 생성해서 보여주는 것보다 더 효율적이고 좋을 것이라고 생각했다.

그러면 업데이트되는 뷰들은 어떻게 해야 할까? 예를 들어 상품 추가 탭에서 상품을 추가하면 표에 추가한 상품이 추가돼야 한다. 처음 뷰를 보여줄때 모든 DOM 을 생성했기 때문에 그 DOM에 추가해서 업데이트할 DOM만 데이터를 반영해서 업데이트 해주면 된다.

이렇게 DOM이 업데이트 되는 부분들의 시작은 무엇일까? 바로 **이벤트리스너**이다. 이벤트가 발생했을 때 해당 탭의 DOM을 업데이트 해주면 되는 것이다. 그런데 한 가지 예외 상황을 맞닥뜨리면서 이런 뷰 설계에 문제가 있다는 것을 깨달았다.

**상품 추가 탭에서 상품을 추가하면 상품 추가 탭의 상품 현황 DOM만 업데이트 되는 것이 아니라 상품 구매 탭의 구매할 수 있는 상품 현황 DOM도 업데이트가 돼야 하는 것**이다.

그러면 상품 추가 탭에서 상품을 추가하는 이벤트가 발생했을 때 상품 구매 탭의 표를 업데이트 해줘야 하는 상황이다.

나는 이 상황이 마음에 들지 않았다. 상품 추가하는 이벤트를 처리하는 함수에서 상품 구매의 뷰를 업데이트해줘야 하기 때문에 해당 함수가 할 일을 벗어나는 느낌이다. 해당 함수의 의도된 역할을 넘어서서 다른 일까지 하게 되므로 이 설계에는 문제가 있다고 생각했다.

리액트를 생각해보면 각 탭에서 보여주는 뷰들을 하나의 컴포넌트로 볼 수 있다. 각 컴포넌트는 고유의 state값을 갖고 해당 state값들을 백엔드에서 불러와서 최신화된 데이터들을 뷰에 보여주는 방식으로 구현이 가능하다. 이렇게 하기 위해서는 이 프로그램에서는 객체가 갖고 있는 정보를 보여주는 것이 가장 적절하다.

이런 생각들을 기반으로 해서 처음에 했던 방식을 바꿨다. 처음부터 전부 다 뷰를 렌더하는 것이 아니라 현재 **탭 정보만 누를때마다 뷰를 새로 업데이트해서 렌더하는 것**이다. 이렇게 한다면 해당 뷰에서 다룰 데이터는 해당 뷰에서만 다룰 수 있다. 그러고 객체에 저장된 데이터를 각 뷰에서 접근하기 때문에 데이터의 일관성도 잘 유지된다.

탭을 눌러서 다른 뷰를 보려고 하면 현재 보이는 DOM을 삭제해버린다. 그리고나서 새롭게 보여질 뷰를 만들어서 보여준다. 리액트도 이런 식으로 컴포넌트를 보여주기 때문에 이런 방식으로 구현했다.

## 3. Javascript

### 3-1. Localstorage
`localStorage`를 사용하면 웹 브라우저에 데이터를 저장하고 사용할 수 있기 때문에 페이지가 새로고침이 되더라도 값을 유지할 수 있는 장점이 있다. 그런데 localStorage를 사용할 때 주의할 점이 있다.

`localStorage`는 값을 저장할 때 전부 `string`으로 변환해서 저장한다. 값을 불러올 때도 저장된 값이 `string`이니까 당연히 문자열로 데이터를 가져온다. 그런데 **데이터가 만약에 객체라면 어떻게 사용해야 할까?**

```js
> localStorage.setItem('obj', {a: 1, b: 2})
undefined
> localStorage.getItem('obj')
"[object Object]"
```

위 예시를 보면 객체를 그대로 `localStorage`에 저장하려고 하면 객체를 문자열로 변환해서 저장하게 된다.

```js
> String({a: 1, b: 2})
"[object Object]"
```
이렇게 변환되기 때문에 객체에 담은 데이터를 전혀 쓸 수 없게 되는 버그를 만들게 된다. 이러한 문제를 해결하기 위해서 **JSON.parse 와 JSON.stringify** 함수를 사용해야 한다.

```js
> localStorage.setItem('json', JSON.stringify({a: 1, b: 2}))
undefined
> JSON.parse(localStorage.getItem('json'))
{a: 1, b: 2}
```
`JSON.stringify` 함수를 이용해서 객체의 정보를 그대로 담은 문자열로 변환(직렬화)을 해서 저장하고 값을 불러올 때는 `JSON.parse`를 통해서 문자열로 된 객체를 실제 객체로 변환(역직렬화)해서 사용하면 객체가 갖고 있는 데이터에 의도한대로 접근할 수 있다.

## 4. Git

### 4-1. 직전 커밋 수정하기
README 를 수정할 일이 있어서 수정을 한 후에 `commit`을 한 상황이다. 그런데 파일에 오타가 있다는 것을 발견하고 방금 commit한 내용에 대해서 수정하고 싶을 수 있다.

이런 상황에서 아주 간단하게 적용할 수 있는 명령어가 있다.

이미 알고 있는 명령어였지만 보통은 직전의 커밋 메세지의 내용을 수정할 때 쓰곤 했다.

`git commit --amend` 를 사용해서 명령어를 입력하면 직전의 커밋로그 작성하던 텍스트 편집기가 화면에 보인다. 이제 커밋로그를 수정한 다음에 저장하면 commit message가 수정돼서 로그에 저장된다.

그런데 만약에 코드를 바꾼다거나 어떤 수정이 필요할 떄는 어떻게 할까?

아주 간단하다. **바로 직전의 커밋을 수정하는 상황**이라면 현재 상태에서 수정하고 싶은 파일을 수정한다. 그리고 나서 `git add <수정한 파일>` 을 해주고 `git commit --amend`를 해주면 똑같이 커밋로그를 작성하는 텍스트 편집기가 화면에 보인다. 이제 메세지를 수정할거면 수정하고 아니면 그대로 저장하면 커밋에 수정사항이 반영돼서 수정된다. 

## References

- 모던 자바스크립트 Deep Dive (이웅모 저)
- [클린코드 Javascript](https://github.com/qkraudghgh/clean-code-javascript-ko)
- [모던 자바스크립트 튜토리얼](https://ko.javascript.info/)
- [LocalStorage](https://www.daleseo.com/js-web-storage/)
- [NAVER D2 - 브라우저는 어떻게 동작하는가?](https://d2.naver.com/helloworld/59361)
- [git commit 수정하기](https://mizzo-dev.tistory.com/entry/git-commit-edit)
