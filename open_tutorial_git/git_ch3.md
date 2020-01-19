Chapter 3 : GIT CLI backup & 협업
---------------------------------

---

### git hub 원격 저장소와 local 저장소 연결<br>

```C
git add origin remote <원격저장소 주소>
git push --set-upstream origin master
```
- origin 은 사용자가 원하는 이름으로 바꿔도 되지만 관습적으로 origin을 사용한다. <br><br>
- `git push --set-upstream origin master` 명령어는 앞으로 이 저장소에서 git push만 하면 origin remote 저장소로 push 하겠다는 것을 알려주는 것이다. 최초 1회만 명령하면 그 이후부터는 `git push`만 하면 해당 remote 저장소로 push된다. <br><br>

### 원격저장소를 local 저장소로 복제<br>

```C
git clone <원격저장소 주소> <생성할 폴더이름> //폴더이름을 생략하면 my-repo라는 이름으로 생성된다.
```
- 이렇게 복제를 하면 알아서 원격저장소에 있는 파일들이 pull된다.<br><br>
- `git pull`은 원격저장소에 있는 파일을 local 저장소로 가져올 때 사용하는 명령어이다.<br><br>


### 협업<br>
- github에서 동료와 협업을 할 때는 repository setting 에서 colaborators에 지정을 해줘야 한다.<br><br>
- 협업을 할 때 동시에 같은 파일을 수정하고 push를 하면 conflict가 발생하기 때문에 자주 pull하고 자주 push해서 서로 충돌이 일어나지 않도록 하는 것이 좋은 습관이다.<br><br>

### pull & fetch
```C
git pull  //git fetch를 하고 git merge origin/master(원격 저장소 branch) 한 것과 같다.
git fetch
```

- `git fetch`는 remote branch만 가져온다. 그러므로 현재 local 저장소에서는 변경사항이 적용되지 않는다. 즉 fetch를 하고 나서 merge를 해줘야 git pull을 한 효과를 낼 수 있는 것이다.<br><br>

### patch
```C
git format-patch e23b234c234d234  
git am -3 -i *.patch //3 way merge를 이용해서 patch파일을 전부 적용해라,
```
- 어떤 프로젝트에 자신의 코드를 push 하고 싶지만 push를 할 권한이 없을 때, push를 할 수 있는 권한이 있는 사람한테 patch를 이용해서 생성한 파일을 보냄으로써 프로젝트에 자신의 코드를 넣을 수 있는 방법이 있다.<br><br>
- remote 저장소에서 가져온(pull) 가장 최신화된 버전에서 자신이 push하고 싶은 변경사항을 넣은 버전을 만든 후에 pull한 시점의 commit id(내가 수정을 하기 직전의 파일)를 `git format-patch <commit id>`을 통해 명령하면 patch 파일들이 생성된다. 이를 다른 사람에게 이메일 등으로 보내줄 수 있다. <br><br>  
- git am 에서 -i 옵션은 interaction 으로 정말 적용할지를 물어보게 하는 옵션이다. <br><br>

### pull request
- 내가 저장한 내용을 pull 해주십시오라고 요청하는 것이다.<br><br>
- github 에서 fork 라는 기능이 있는데 다른 저장소를 fork 하면 그 저장소를 그대로 복사해서 내 저장소로 만들 수 있다. 그러면 복사된 그 저장소는 이제 나의 저장소기 때문에 내가 마음대로 push pull 할 수 있다.<br><br>
- github 에서 compare라는 기능이 있는데 이는 원래 저장소와 fork를 통해 복사해온 저장소를 비교해 주는 것이다. 보통 pull request를 하기 전에 시행하며 fork로 복사해와서 자신이 수정한 이후에 원래 original 저장소와 복사한 저장소의 차이를 볼 수 있다.<br><br>
- 이후에 github에 있는 pull request버튼을 누르면 원래 저장소의 주인에게 정보가 전달되고 원래 저장소의 주인은 그 메세지를 확인하고 merge pull request를 할 수 있다. <br><br>

### Reference<br>

-	생활코딩 GIT CLI backup , GIT CLI 협업(egoing)
- URL : https://opentutorials.org/course/3841, https://opentutorials.org/course/3842

---
