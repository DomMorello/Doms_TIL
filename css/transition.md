# trandision
```
.box{
        background-color: green;
        color: white;
        transition: all 0.5s ease-in-out;
}
.box:hover{
        background-color: red;
        color:blue;
}
```
위 코드와 같이 했을 때 `transition: color 3s ease-int-out` 이런 식으로 all 대신에 다른 property를 지정하면 그 property에만 transition 효과가 작용한다. <br>
저 박스에 마우스를 hover (갖다 대면)하면 0.5초 동안 모든 property가 hover style로 서서히 바뀐다. <br>
