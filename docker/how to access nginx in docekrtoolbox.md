# How to access nginx page in dockertoolbox
Docker Quickstart Terminal 접속하면 VirtualBox에서 리눅스 VM이 올라가고 그 위에서 Docker가 사용 가능한 상태로 세팅 될 것이다.

`docker pull nginx` 명령어로 nginx image를 pull 한다.
`docker run -d -p 80:80 -p 443:443 --name myserver nginx`명령어로 컨테이너를 실행한다.
`-d` 옵션은 detached 즉 백그라운데서 실행하는 옵션이고 `-p` 옵션은 port를 연결해주는 옵션이다. -p (HostPort):(ContainerPort).
![image](https://user-images.githubusercontent.com/51396282/78329199-04059e80-75bc-11ea-95db-6a8ba56595af.png)
docker terminal을 실행하면서 VirtualBox에 Default라는 이름으로 머신이 하나 생성돼있을 것이다. 그 머신을 클릭한 후에 설정 - 네트워크 - 고급 - 포트포워딩 - 추가버튼 을 통해서 80:80 과 443:443을 추가해준다.
이후에 윈도우로 돌아와서 브라우저에서 주소창에 127.0.0.1을 입력하면 nginx 초기화면이 보인다. 
