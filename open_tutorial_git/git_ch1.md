Chapter 1 : GIT CLI 버전 관리
---------------------------------

---

### GIT의 목적<br>

-	GIT은 버전관리, 백업, 협업을 위해 존재한다.<br><br>
- MAC 사용자는 terminal에서 git 명령어를 쳐서 설치가 돼있는지 확인하자.<br><br>

### 기존에 알던 명령어들<br>

```C
git log //log를 보여줘라
ls -al //폴더에 있는 파일들을 자세히 보여줘라
git add //버전관리를 할 거니까 staging area에 올린다
git commit //버전을 생성해 staging area에 있던 파일이 repository로 간다
git log --stat //state 즉, log와 함께 각 commit에 어떤 파일들이 연루되어있는지 보여준다.
git add . //현재 디렉토리에 있는 모든 파일을 add한다.
git add <폴더명> //폴더 아래 파일들을 전부 add 한다.
git commit -am "message"  //add와 commit을 동시에 해준다.
```
- git commit -am "": 실수로 원치 않는 파일을 commit 하지 않도록 unracked 파일이 있으면 그 파일은 commit 하지 않는다. 즉, 최초에 한 번은 add가 된 상태여야 -am 명령어를 할 수 있다. 예를 들어 최초 1회 add를 한 상태인 파일을 수정하고 새로운 파일을 만들고(최초 add를 하지 않은 파일) git commit -am 을 하면 최초 1회 add를 한 파일(track되고 있는 파일)은 commit이 되지만 untracked(새로 만든 파일, 최초 1회 add하지 않은 파일)은 commit이 되지 않는다.

### 버전관리 명령어<br>

```C
cat hello1.txt  //hello1.txt 파일에 있는 내용을 출력한다.
git diff  //파일의 변경사항을 보여준다. 차이점 difference
git log -p //파일의 변경사항을 세부적으로 보여준다.
git checkout e234e534e352e42c3234c4d243a4a54  //checkout 뒤에 있는 주소로 header를 이동시킨다
git config --global core.editor "vim" //이 컴퓨터의 모든 폴더에서 기본 에디터를 vim으로 설정한다.
git reset --hard e2e23423e234a3v2345  //주소로 header를 옮기고 수정중인 파일과 옮긴 header 이후의 버전들을 다 삭제한다.
git revert e2324a534b234c234  //주소로 돌아간다.
```
- git diff: unstaged된 상태(Add가 안된 상태)에서 변경점이 확인 됩니다. staging agre에 넣지 않은 상태 즉, add를 하지 않은 상태에서 명령어를 쳐야 차이점을 확인할 수 있다.<br><br>
- git checkout: 시간여행과 같다. log 에서 확인할 수 있는 commit 시점의 주소를 복사해서 checkout 하면 그 시점의 상태로 파일들이 돌아간다. 하지만 돌아갔다고 해서 그 돌아간 시점 이후에 변경된 사항들이 삭제되는 것은 아니다. git checkout master를 통해서 다시 최신의 상태로 돌아올 수 있다.<br><br>
- git reset : 협업을 할 때는 사용하면 엉키게 되는 문제가 발생하니 주의해야 한다.<br><br>
- git revert : 4개의 log가 있고 각각의 commit message가 1 2 3 4 일 때 4를 commit 하고 나서 잘못을 깨달은 이후 3으로 돌아가고 싶을 때 git revert '4의 주소' 를 하면 4에서 변경된 사항을 취소하여 3의 내용과 같은 버전이 commit된다. 여기서 주의할 점은 revert를 했을 때 4의 commit 내용이 사라지는 것이 아니라 4 이후에 새롭게 log가 쌓이는 형태가 된다. 그래서 4의 변경사항도 log속에 유지할 수 있다. 만약에 4 3 2를 다 취소하고 1로 돌아가고 싶을 때 revert 2 를 하면 될 것 같지만 이렇게 하면 2의 변경사항만 취소가 되므로 3 4 의 변경사항을 유실할 수 있기 때문에 에러메세지를 보낸다. 즉 1로 돌아가고 싶으면 역순으로 4 3 2를 각각 revert를 해줘서 돌아가야 한다.<br><br>
- .gitignore : 파일을 만든 후 버전관리를 하고 싶지 않는 파일들의 이름을 넣으면 버전관리를 하지 않는다.<br><br>
- 위의 예제에서 e234v324a2334c324 등의 commit id 는 보기 복잡하기 떄문에 이 id값을 직접 설정하고 싶다면 tag 라는 기능을 사용하면 된다.<br><br>

### Reference<br>

-	생활코딩 GIT CLI 버전관리 (egoing)
- URL : https://opentutorials.org/course/3839

---
