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
transform �� ���¸� ��ȭ��ų �� �ִ�. `transform:rotate(20deg)` ���� ��쿡�� � ����� 20�� ������ ���·� ���̰� �� �� �ִ�. <br>
�̷� Ư���� transition�� �Բ� ���� �ִϸ��̼� ȿ���� �� �� �ִ�. <br>
���� ��� �� �ڵ�� ���� hover���� �� transform�� �ϰ� hover�� �ƴ� ���¿��� transition�� �ָ� <br>
���콺�� ���� ���� �� ������ rotate�ϰ� scale�� �ۿ��ϸ鼭 ũ�Ⱑ ������ �۾��� ���̴�. <br>
