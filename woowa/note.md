### 비즈니스 로직과 UI 를 분리하라
1,2주차 과제를 진행하면서 **MVC 패턴**을 유지하려고 노력했다. 특히 디렉토리 구조를 명확하게 나눠서 input, view 디렉토리를 두고 UI를 모두 view 디렉토리에 넣고 진행했다. 그런데 2주차 피드백을 보고 이렇게 하는 것이 답은 아니라는 것을 느꼈다.

![image](./feedback.png)

피드백 내용을 살펴보면 위 사진과 같다. 내가 지금까지 했던 방식과는 다르게 `Car` 클래스 내부에서 UI 로직을 갖고 있는 것을 볼 수 있다. 이 피드백을 받고 지금까지 했던 방식과는 다르게 해보려고 생각했다. 우선 `Car`와 밀접하게 관련된 UI 를 보여주는 함수가 `Car` 클래스 내부에 선언돼 있는 것이 합리적이다. 내가 기존에 했던 방식으로 했을 때는 **View 클래스에 서로 관련 없는 UI 로직이 다 뭉쳐져 있었다**. 이런 부분은 확실히 그 해당 클래스에 역할이 무엇인지를 명확하게 밝힐 수 없는 구조인 것 같다.

### innerHTML vs insertAdjscentHTML
(https://oniondev.tistory.com/17)
(https://developer.mozilla.org/ko/docs/Web/API/Element/insertAdjacentHTML)

### Localstorage
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

출처: (https://www.daleseo.com/js-web-storage/)