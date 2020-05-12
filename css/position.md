# position
`position: fixed` 속성을 주면 스크롤을 내려도 그 위치에 고정된다. <br>
`position: static` 은 기본값이다. 위치를 정해놓으면 그 위치에 있는다.<br>
`position: absolute`속성을 주면 html에서 해당 element와 관계가 있는 부모 element를 찾아서 이에 상응하는 포지션에 위치한다. <br>
만약에 부모 element에 `position: relative`를 넣게 되면 그 부모 element 안에서 absolute 속성을 설정한 자식 element가 위치하게 된다. <br>
즉 부모 element에 먼저 relative 속성이 설정된 후에 자식 element에 absolute이 있어야 해당 부모 element 안에서 자식 element가 위치할 것이다. <br>

