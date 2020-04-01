# docker commands
##### 컨테이너 목록 보기
```bash
docker ps //(= docker container ls)실행중인 컨테이너 목록을 출력
docker ps -a //(= docker ps --all)종료된 컨데이터까지 함께 보여줌
```
##### 컨테이너 중단
```bash
docker stop [options] CONTAINER //컨테이너 중단
```
컨테이너를 중단할 때 컨테이너 ID를 입력하는데 이 때 ID가 다른 컨테이너와 구분되는 부분까지만 적어줘도 해당 컨테이너를 찾아서 중단시킨다.
##### 컨테이너 삭제
```bash
docker rm [options] CONTAINER //컨테이너 삭제
docker images [options] [repository] //이미지 목록 출력
```
##### 이미지 다운
```bash
docker pull [options] NAME  //이미지 다운받기
```
run명령어를 입력하면 이미지가 없을 때 자동으로 다운받으니 pull명령어를 언제 쓰는지 궁금할 수 있는데 pull은 최신버전으로 다시 다운 받음. 같은 태그지만 이미지가 업데이트 된 경우는 pull명령어를 통해 새로 다운받을 수 있음.
##### 이미지 삭제
```bash
docker rmi [OPTIONS] IMAGE [IMAGE...] //이미지 삭제하기
```
컨테이너가 실행중인 이미지는 삭제되지 않음.
##### 컨테이너 로그 보기
```bash
docker logs [options] CONTAINER //컨테이너 로그 보기
docker logs --tail 10 CONTAINER //마지막 10줄만 로그 출력
docker logs -f CONTAINER //로그 실시간으로 보기, 중지하려면 Ctrl + C
```
##### exec
```bash
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]  //실행중인 컨테이너에 명령 전달

docker exec -it mysql /bin/bash //쉘을 시작할 수 있고
docker exec -it mysql mysql -uroot  //바로 명령어를 넣어도 된다.
```
exec 명령어를 사용하면 host OS에 mysql 이 설치돼있지 않아도 컨테이너 내부에서 명령어를 사용할 수 있다.
