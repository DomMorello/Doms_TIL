# docker commands
domMorello/centos 처럼 앞에 사용자 이름이 있으면 공식 이미지가 아니다.
##### 컨테이너 목록 보기
```bash
docker ps //(= docker container ls)실행중인 컨테이너 목록을 출력
docker ps -a //(= docker ps --all)종료된 컨데이터까지 함께 보여줌
docker container ls -a //이것도 됨
```
##### 컨테이너 로그 보기
```bash
docker logs [options] CONTAINER //컨테이너 로그 보기
docker logs --tail 10 CONTAINER //마지막 10줄만 로그 출력
docker logs -f CONTAINER //로그 실시간으로 보기, 중지하려면 Ctrl + C
```
##### 컨테이너 중단
```bash
docker stop [options] CONTAINER //컨테이너 중단
```
컨테이너를 중단할 때 컨테이너 ID를 입력하는데 이 때 ID가 다른 컨테이너와 구분되는 부분까지만 적어줘도 해당 컨테이너를 찾아서 중단시킨다.
##### 컨테이너 삭제
```bash
docker rm [options] CONTAINER //컨테이너 삭제
```
##### 컨테이너 접속
```bash
docker attach CONTAINER
docekr attach mycontainer //start를 한 후에 이 명령어를 입력하면 쉘 환경으로 바뀌면서 컨테이너에 접속이 됨.
```
##### 이미지 검색
```bash
docker search IMAGE
docker search centos
```
##### 이미지 목록 출력
```bash
docker images [options] [repository] //이미지 목록 출력
docker image ls //이것도 가능
```
##### 이미지 다운
```bash
docker pull [options] NAME  //이미지 다운받기
```
run명령어를 입력하면 이미지가 없을 때 자동으로 다운받으니 pull명령어를 언제 쓰는지 궁금할 수 있는데 pull은 최신버전으로 다시 다운 받음. 같은 태그지만 이미지가 업데이트 된 경우는 pull명령어를 통해 새로 다운받을 수 있음.
##### 이미지 삭제
```bash
docker rmi [OPTIONS] IMAGE [IMAGE...] //이미지 삭제하기
docker rmi $(docker images -f “dangling=true” -q)
```
`docker images -f "dangling=true" -q` 를 하면 non-tagged 즉 <none> 으로 이름이 돼있는 이미지들은 전부 출력한다. 이를 rmi 명령어와 함께 쓰면 태그되지 않은 이름없는 이미지들을 전부 삭제할 수 있다.
- docker imgae 옵션
`--filter , -f`	:	Filter output based on conditions provided
`--quiet , -q`	:	Only show numeric IDs<br>

컨테이너가 실행중인 이미지는 삭제되지 않음.
##### 이미지 태그하기
```bash
docker image tag <IMAGE_ID> <TAG>
docker image tag 85ab32e... domsimage
docker image build -t hello:v0.1 . //이런 식으로도 사용 가능(hello -> REPOSITORY, v0.1 -> TAG)
```
위 명령어 이후 `docker image ls`를 실행하면 REPOSITORY column에 domsimage라고 변경돼서 출력된다.
##### 이미지 검사하기
```bash
docker image inspect <IMAGE>
docker image inspect --format "{{ json .RootFS.Layers }}" alpine
```
`inspect`를 하면 다음을 포함한 다양한 정보들을 확인할 수 있다.
- the layers the image is composed of
- the driver used to store the layers
- the architecture / OS it has been created for
- metadata of the image
...
##### exec
```bash
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]  //실행중인 컨테이너에 명령 전달

docker exec -it mysql /bin/bash //쉘을 시작할 수 있고
docker exec -it mysql mysql -uroot  //바로 명령어를 넣어도 된다.
```
exec 명령어를 사용하면 host OS에 mysql 이 설치돼있지 않아도 컨테이너 내부에서 명령어를 사용할 수 있다.
###### docker 빌드
```bash
docker build <옵션> <Dockerfile 경로>
docker build -t domMorello/hello:v01.2 . //현재 위치에 Dockerfile이 있고 저장소이름,이미지이름,태그 를 달겠다.
```
##### [출처]
- (https://docs.docker.com/engine/reference/commandline/images/)
