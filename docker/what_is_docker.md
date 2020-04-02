# what is docker?
- 컨테이너 기반의 오픈소스 가상화 플랫폼

Docker기술은 Linux 커널과 함께 Cgroups 및 네임스페이스와 같은 커널의 기능을 사용하여 프로세스를 분리함으로써 독립적으로 실행될 수 있도록 한다. 이러한 독립성은 컨테이너의 본래 목적이다. 다시 말해서, 여러 프로세스와 애플리케이션을 서로 개별적으로 실행하여 인프라를 더 효과적으로 활용하고 개별 시스템을 사용할 때와 동일한 보안을 유지할 수 있다.
![image](https://user-images.githubusercontent.com/51396282/78120960-bbc47000-7445-11ea-8539-6781efef9592.png)


가상머신은 운영체제 위에 하드웨어를 에뮬레이션하고 그 위에 운영체제를 올리고 프로세스를 실행하는 반면에, 도커 컨테이너는 하드웨어 에뮬레이션 없이 리눅스 커널을 공유해서 바로 프로세스를 실행한다.

## container
Containers are an abstraction at the app layer that packages code and dependencies together. Multiple containers can run on the same machine and share the OS kernel with other containers, each running as isolated processes in user space. Containers take up less space than VMs (container images are typically tens of MBs in size), can handle more applications and require fewer VMs and Operating systems.

## image
A Docker image is a file, comprised of multiple layers, that is used to execute code in a Docker container. An image is essentially built from the instructions for a complete and executable version of an application, which relies on the host OS kernel. When the Docker user runs an image, it can become one or multiple instances of that container.

## tablerminology
- Layers - A Docker image is built up from a series of layers. Each layer represents an instruction in the image’s Dockerfile. Each layer except the last one is read-only.
- Dockerfile - A text file that contains all the commands, in order, needed to build a given image. The Dockerfile reference page lists the various commands and format details for Dockerfiles.
- Volumes - A special Docker container layer that allows data to persist and be shared separately from the container itself. Think of volumes as a way to abstract and manage your persistent data separately from the application itself.
