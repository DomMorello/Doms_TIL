# transformation
```
.box{
    width: 300px;
    height: 300px;
    background: red;
    transition: transform 3s ease-in-out;
}
.box:hover{
    transform: rotate(1turn) scale(.5, .5);
}
```
transform 은 형태를 변화시킬 수 있다. `transform:rotate(20deg)` 같은 경우에는 어떤 모양이 20도 기울어진 형태로 보이게 할 수 있다. <br>
이런 특성을 transition과 함께 쓰면 애니메이션 효과를 낼 수 있다. <br>
예를 들어 위 코드와 같이 hover했을 때 transform을 하고 hover가 아닌 상태에서 transition을 주면 <br>
마우스를 갖다 댔을 때 서서히 rotate하고 scale이 작용하면서 크기가 반으로 작아질 것이다. <br>
