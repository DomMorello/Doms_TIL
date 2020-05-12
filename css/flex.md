# flex
`display:flex`를 부모 element에 적용하면 그 안에 있는 자식 element들이 전부 자동으로 inline-block처럼 블락 안에 들어오게 된다. <br>
이 상태에서 부모 element에 `justify-content:center` 라고 하면 자식 element들이 전부 가운데(수평 기준으로)로 위치한다. <br>
`justify-content:space-between`을 주면 알아서 자식 element 간격이 조정되면서 아이템들 사이사이에 공간이 생긴다. <br>
`justify-content:space-around`를 주게 되면 아이템들 사이 공간뿐 아니라 주변 양 끝과의 거리도 자동으로 계산돼서 벌려진다. <br>
`align-items:center`라고 하면 위치가 가운데로(수직 기준으로) 정렬된다. <br>
`align-items:flex-start, flex-end` 등을 선택해서 수직으로 맨 위인지 아래인지 위치를 부모 element를 기준으로 조정할 수 있다. <br>

여기서 주의할 점은 `flex-direction`이 디폴트값으로 `row`로 돼있기 때문에 `justify-content`가 수평으로 조정되고 `align-items`가 수직으로 조정되는 것이다. <br>
그런데 `flex-direction`가 `column`으로 설정돼있으면 수평, 수직이 서로 바뀌어서 적용된다. 그리고 자식 element들이 가로가 아니라 세로로 정렬된다. <br>

`flex-wrap:no-wrap` 이 디폴트 값인데 이럴 경우에는 자식 element들이 계속 추가 돼도 부모 element 안에서 계속 크기가 작아지면서 그 블락 안에서 자동으로 크기가 조정되면서 위치하게 된다. <br>
그런데 창이 작아지거나 할 때 창이 작아지면서 자식 box들도 같이 작아지는 것이 아니라 어느 정도 작아졌을 때 폭이 좁아지는게 아니라 아래로 떨어져서 보이게 하고 싶을 수도 있다. <br>
이럴 때는 `flex-wrap:wrap`으로 바꿔주면 창이 작아질 때 알아서 자식 box가 아래로 떨어져서 보이게 된다. <br>
