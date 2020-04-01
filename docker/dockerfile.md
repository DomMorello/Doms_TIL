# dockerfile
##### FROM
```bash
FROM <image>:<tag>
FROM ubuntu:16.04
```
베이스 이미지를 지정. 반드시 지정해야 하며 어떤 이미지도 베이스 이미지가 될 수 있다. tag 는 latest보다는 지정하는 것이 좋다.
##### MAINTAINER
```bash
MAINTAINER <name>
MAINTAINER zuzudnf@gmail.com
```
Dockerfile을 관리하는 사람의 이름 또는 이메일 정보를 적는다. 빌드에 딱히 영향을 주지는 않는다.
##### COPY
```bash
COPY <src>... <dest>
COPY . /usr/src/app
```
파일이나 디렉토리를 이미지로 복사. 일반적으로 소스를 복사하는 데 사용. target디렉토리가 없다면 자동으로 생성.
##### ADD
```bash
ADD <src>... <dest>
ADD . /usr/src/app
```
`COPY`명령어와 매우 유사하나 몇가지 추가 기능이 있다. src에 파일 대신 URL을 입력할 수 있고 src에 압축 파일을 입력하는 경우 자동으로 압축을 해제하면서 복사됨.
##### RUN
```bash
RUN <command>
RUN ["executable", "param1", "param2"]
RUN bundle install
```
명령어를 그대로 실행. 내부적으로 /bin/sh -c 뒤에 명령어를 실행하는 방식.
##### CMD
```bash
CMD ["executable","param1","param2"]
CMD command param1 param2
CMD bundle exec ruby app.rb
```
도커 컨테이너가 실행되었을 때 실행되는 명령어를 정의. 빌드할 때는 실행되지 않으며 여러 개의 CMD가 존재할 경우 가장 마지막 CMD만 실행됨. 한꺼번에 여러 개의 프로그램을 실행하고 싶은 경우에는 run.sh파일을 작성하여 데몬으로 실행하거나 supervisord나 forego와 같은 여러 개의 프로그램을 실행하는 프로그램을 사용.
##### WORKDIR
```bash
WORKDIR /path/to/workdir
```
RUN, CMD, ADD, COPY등이 이루어질 기본 디렉토리를 설정. 각 명령어의 현재 디렉토리는 한 줄 한 줄마다 초기화되기 때문에 RUN cd /path를 하더라도 다음 명령어에선 다시 위치가 초기화 됨. 같은 디렉토리에서 계속 작업하기 위해서 WORKDIR을 사용.
##### EXPOSE
```bash
EXPOSE <port> [<port>...]
EXPOSE 4567
```
도커 컨테이너가 실행되었을 때 요청을 기다리고 있는(Listen) 포트를 지정. 여러개의 포트를 지정할 수 있음.
##### VOLUME
```bash
VOLUME ["/data"]
```
컨테이너 외부에 파일시스템을 마운트 할 때 사용.
##### ENV
```bash
ENV <key> <value>
ENV <key>=<value> ...
ENV DB_URL mysql
```
컨테이너에서 사용할 환경변수를 지정. 컨테이너를 실행할 때 -e옵션을 사용하면 기존 값을 오버라이딩 하게 됨.
