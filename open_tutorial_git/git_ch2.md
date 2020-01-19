Chapter 2 : GIT CLI branch & conflict
---------------------------------

---

### branch<br>

```C
git branch  //이 명령어를 실행하는 시점부터 branch가 하나 생성되면서 분기가 생긴다.
git checkout <branch name>  //해당 branch로 작업을 시작한다.
```
- 동일한 소스코드로 다수의 사람이 작업을 각각 독립적으로 할 떄 필요한 것이 branch이다.<br><br>
- 참고: terminal 에서 `git init <생성할 폴더명>`을 하면 현재 폴더 위치에서 새로운 폴더를 만듦과 동시에 git repository로 만들어준다. <br><br>

### merge<br>

```C
A(base) -----A---A-------->
        |        |(master A 로 merge)
        B--- B----
```
- A라는 점에서 B로 분기가 되는 브랜치가 생성되었을 때 A는 부모노드가 되고 계속 A인 줄기와 A 이후로 생긴 B라는 줄기(branch)가 생기게 된다. 여기서 A를 base 라고 한다.<br><br>
- 이 상황에서 A 줄기와 B줄기를 다시 합치는 것을 merge라고 한다. <br><br>
- 참고: `git commit --amend` 명령어는 가장 최근에 commit 한 commit message를 수정할 수 있다.<br><br>
- 분기된 B branch를 A(master)로 다시 합치고 싶을 때는 일단 master branch로 checkout 한 이후에 `git merge <branch name>` 명령어를 쳐서 합치면 된다.<br><br>
- merge를 할 떄 A와 B branch에서 각각 독립적으로 같은 파일을 수정했다고 가정하자. A는 파일의 첫째줄을 수정하고 B는 두번째 줄을 수정한 이후에 B를 A(master)로 merge했을 때 그 파일의 상태는 A가 수정한 첫째줄과 B가 수정한 두번째 줄의 변경사항을 모두 적용하고 있다. 즉 git은 같은 파일의 서로 다른 위치를 수정하고 merge를 하면 각각 수정된 변경사항을 합쳐준다.<br><br>
- merge를 할 때 A와 B가 같은 파일의 같은 위치를 수정하고 merge를 하면 conflict가 발생한다. 이 때 merge하려는 master 브랜치에서 해당 파일을 열면 편집기가 각각의 브랜치가 같은 장소에 각각 어떻게 변경을 했는지를 보여준다. 편집기에서 이 부분을 원하는대로 수정을 해주고 다시 commit을 하면 merge가 된다. 즉, git은 서로 다른 branch가 같은 파일의 같은 위치를 각각 수정하면 어떤 부분으로 수정해줘야 할지 정할 수 없기 때문에 사용자에게 이 부분을 수정해달라고 요청하는 것이고 이를 사용자가 결정해주면 그제서야 commit이 가능하게 해준다.<br><br>

### 3 way merge<br>

```C
left(branch)  base  right(branch)
A             A       R //right 브랜치만 수정한 경우
L             B       B //left 브랜치만 수정한 경우
C             C       C //아무데서도 수정하지 않은 경우
L             D       R //left, right 브랜치에서 각각 수정한 경우
```
- 3 way merge는 base의 내용을 참고한다.<br><br>
- 첫 번째와 두 번째 경우처럼 한 쪽 브랜치에서만 수정이 있었으면 그 수정한 파일로 수정된다.<br><br>
- 세 번째 경우처럼 아무 수정이 없었다면 당연히 그대로 있는다.<br><br>
- 네 번째 경우처럼 양 쪽 branch에서 같은 파일을 수정했으면 사용자가 직접 어떤 것으로 적용할지 수정해줘야 한다.<br><br>
- 3 way merge 툴을 사용하는 방법도 있다.<br><br>

### checkout & reset

- checkout은 head가 어디를 가리키는지를 정하는 명령어이다.<br><br>
- head는 branch를 가리키는 것이 일반적이지만 `checkout e213b3c423d3` 이런 식으로 commit id 를 가리킬 수도 있다. 이렇게 commit id를 가리키는 상태를 브랜치로부터 떨어져있다고 해서 detached 상태에 있다고 한다.<br><br>
- reset은 head가 branch를 가리키고 있는 동안에는 branch를 관리한다. 즉, master branch와 B라는 branch가 있을 때 reset master 라고 하면 master가 가리키고 있는 버전을 B branch도 가리킨다는 것을 의미한다.<br><br>
- reset은 일반적으로 branch에 사용하지 않고 commit id에 사용한다. commit id를 통해 버전을 이전 으로 가리키게 되면 그 이후에 수정된 버전들은 더이상 branch에 연결돼있지 않기 때문에 삭제된 것 같은 효과를 낸다.<br><br>

### Reference<br>

-	생활코딩 GIT CLI branch & conflict (egoing)
- URL : https://opentutorials.org/course/3840

---
