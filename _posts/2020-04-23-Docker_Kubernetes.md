---
layout: post
title: 딥러닝을 위한 Docker
tags: [Backend]
use_math: true
---

# Docker란?

Docker는 리눅스 컨테이너에 여러 기능을 추가함으로써 애플리케이션을 컨테이너로 분리해 사용할 수 있게 만들어진 오픈소스 프로젝트입니다. docker는 Go 언어로 작성되었으며 VM(가상머신)에 비해 성능 손실이 거의 없어 가상화 솔루션으로 많이 사용됩니다. 일반적으로 docker라하면 docker 엔진 혹은 docker와 관련된 프로젝트를 의미합니다.

> Docker

<center><img src="https://raw.githubusercontent.com/docker-library/docs/c350af05d3fac7b5c3f6327ac82fe4d990d8729c/docker/logo.png"></center>

기존 VM 기술은 hipervisor를 이용해 여러개의 OS를 하나의 호스트에서 생성해 사용했습니다. 이러한 방식은 hipervisor에의해 Guest OS가 생성되 독립적으로 가상화시키는 방식이었습니다. 그에반해 docker는 Hipervisor와 Guest OS 없이 작동되며, 이 두 부분이 Docker engine으로 대체되었습니다. 이러한 적은 오버헤드 덕분에 성능 손실 없이도 다양한 환경에서 프로그램을 구동 시킬 수 있게 됬습니다.

아래는 VM과 docker의 구조적 차이를 나타내는 그림입니다. 

> docker(containers) vs VM

<center><img src="https://wiki.aquasec.com/download/attachments/2854029/docker-birthday-3-intro-to-docker-slides-18-638.jpg?version=1&modificationDate=1515522843003&api=v2"></center>

### docker의 등장

기존 시스템에선 여러개의 어플리케이션을 구동하기 위해 다양한 패키지, 환경설정 등을 해야했고, 이런 과정속에서 패키지 끼리의 충돌이나 환경변수 충돌과 같은 문제가 많이 발생되어 시스템을 운영(operation)만하기도 굉장히 힘들었습니다. 이 때 어플리케이션별로 환경변수와 패키지를 분리해서 가상화시켜 구동하는 VM이 나왔고 이를 통해 좀 더 편하게 운영이 가능하게 되었습니다. 그렇지만 이러한 VM은 높은 오버헤드로인해 많은 문제가 발생하였습니다. 이러한 문제를 해결하기위해 나온게 docker이며, 이를 통해 컨테이너를 통한 가상화 솔루션을 제안했습니다. docker와 클라우드의 등장으로 인해 시스템의 operation이 보다 쉬워졌고, 이러한 흐름속에서 development와 operation이 합쳐진 devops가 등장했습니다. 기존엔 분리되어 있던 시스템이 합쳐지면서 개발 과정에선 문제가 되지 않았지만 운영과정에서 문제가 되었던 부분을 개발팀에 빠르게 피드백을 주어 문제를 해결하는 방법론이 등장하였고, 대세가 되었습니다. 또한 docker로 애플리케이션을 컨테이너 별로 가상화 시키다보니, 마이크로서비스 구조로 각 애플리케이션을 모듈화하는 방법이 떠오르고 있습니다.

<br>

# Docker image와 container

docker 엔진에서 기본으로 사용하는 단위는 이미지와 컨테이너입니다. 이미지는 컨테이너를 생성하기위한 필수 요소이며, docker 이미지를 docker hub로부터 run해 docker 컨테이너를 만들어냅니다. 즉, 이미지에서 컨테이너를 찍어냅니다.

예를 들어, 웹 서버 docker 이미지로부터 여러개의 컨테이너를 생성하면 생성된 컨테이너의 개수만큼 웹 서버가 생성됩니다.

> docker 허브에서 docker 이미지를 pull해올 수 있음

<center><img src="https://www.itzgeek.com/wp-content/uploads/2016/07/Building-Docker-Images.png"></center>

docker 이미지로부터 docker 컨테이너를 생성하는 키워드는 **docker run**입니다. **docker run** 명령어를 통해 이미지를 읽어 컨테이너를 생성하고 실행합니다.  컨테이너 생성시 이름을 지어줘야 사용하기 편리하며, 이름을 짓지않는다면 랜덤으로 생성됩니다.

```shell
docker run <options> <이미지 이름> <command><args>

options:
	-h: 컨테이너 호스트 이름(컨테이너의 유저) 설정
	-t: tty 모드
	-i: interactive라는 뜻, 컨테이너와 상호적으로 주고 받음
	-d: detached 모드(docker cli 화면으로 들어가지 않음)
	-e: 컨테이너 내부의 환경변수 설정
	-v: docker 볼륨 공유 옵션
	-p: 컨테이너 포트 개방, 호스트 포트와 연결, -p [연결할 호스트의 포트번호]:[컨테이너 포트번호]
	-expose: 컨테이너 포트만 개방
    
	--link: 내부 IP 알 필요 없이 별명으로 접근
	--name: 컨테이너 이름 설정
	--privileged: privileged 모드로 실행
	
example:
	docker run -it --name container_name image_name
```

**docker start** 키워드를 이용하면 꺼져있는 컨테이너를 실행시킵니다.

```shell
docker start <컨테이너 이름>
```

**docker attach** 키워드를 이용하면 해당 컨테이너에 다시 접속합니다. docker 컨테이너가 실행 중인 상태여야 합니다. docker 컨테이너에 접속중인 상태에서 **exit**을 치면 해당 컨테이너를 종료하고 나옵니다. **Ctrl+q+p** 버튼을 누르면 docker 컨테이너 밖으로 이동되며, 해당 컨테이너는 계속 실행 상태입니다.

```shell
docker attach <컨테이너 이름>
```

**docker ps** 키워드를 이용하면 컨테이너 목록을 보여줍니다. 아무런 옵션이 없다면 현재 실행중인 컨테이너 목록을 보여줍니다.

```shell
docker ps <options>

options:
	-a: 전체 컨테이너 목록을 보여줍니다.
```

**docker images** 키워드를 이용하면 docker 이미지 목록을 보여줍니다.

```shell
docker images
```

**docker rm** 키워드를 이용해 컨테이너를 삭제 할 수 있습니다.

```
docker rm <컨테이너 이름>
```



### docker privileged 모드

docker 컨테이너는 기본적으로 unprivileged 모드로 실행이 됩니다. 이 모드에서는 시스템 주요 자원에 접근이 불가능 합니다. 컨테이너를 privileged 모드로 실행하면 시스템의 모든 장치에 접근가능하며, 커널의 기능을 대부분 사용 가능합니다.



### docker 작동 원리

우리가 컴퓨터에서 docker를 실행 시키면 docker는 다음과 같이 작동합니다. docker CLI(클라이언트)에서 docker 데몬(server)에게 API를 통해 명령을 보내면, docker 데몬은 요청에 대한 응답을 보냅니다. 

> 실제 docker의 작동

![](https://nickjanetakis.com/assets/blog/dockers-architecture-6c296cdac053f794eabed5ddda5c04ba7110c746687a0e8b88ba6df919415175.jpg)

docker 데몬은 호스트 운영 체제에서 실행되는 서비스입니다. 클라이언트로 가장 널리 사용되는 도구는 docker CLI입니다. docker CLI는 docker 데몬과 통신 할 수있는 명령 도구입니다. docker를 설치하면 docker 데몬과 docker CLI 도구가 함께 제공됩니다.

아래는 실제 docker CLI에서 명령어를 보내 docker 데몬을 거쳐 이미지를 컨테이너로 생성해내는 과정을 나타냅니다. 만약 docker 데몬 이미지목록에 요청 이미지가 없다면, docker hub나 사설 registry로 부터 이미지를 다운로드 받아옵니다.

> docker는 server-clinet 구조

![](https://nickjanetakis.com/assets/blog/docker-client-host-registry-fc0858b5191e042ce19437fd6b52ba214d1e429bf646374f633598ebaac1a4ab.jpg)



### dockerfile

docker 이미지를 만드는 방법은 docker 허브로 pull 받아오는 방법 말고도 있습니다.

바로 docker 파일을 만든뒤, 빌드해 docker 이미지를 만드는 방법입니다. dockerfile은 docker 이미지 설정 파일입니다. dockerfile에 설정된 내용대로 이미지를 생성합니다.해당 방법을 이용하면 자기가 원하는 방법대로 환경을 커스텀할 수 있습니다. 

> docker 파일을 빌드해 image를 만들수도 있음

<center><img src="https://miro.medium.com/max/2520/1*p8k1b2DZTQEW_yf0hYniXw.png"></center>

docker 파일 안에 들어가는 명령어는 다음과 같습니다

```dockerfile
FROM : 어떤 이미지를 기반으로 이미지를 생성할지 설정
MAINTAINER : 이미지를 생성한 사람의 정보 설정, 보통 이름, 이메일
RUN : FROM에서 설정한 이미지 위에서 명령을 실행합니다.
CMD : 컨테이너가 시작되었을 때 명령을 실행
EXPOSE : 호스트와 연결할 포트 번호 설정
WORKDIR : 실행 파일이 실행될 디렉토리
ENV : 환경변수 설정
ADD : 파일을 이미지에 추가
VOLUME : 디렉토리 내용을 컨테이너에 저장하지 않고, 호스트에 저장
USER : 명령을 실행할 사용자 계정 설정
```

이렇게 만들어진 dockerfile을 빌드하면 docker 이미지가 생성됩니다.

<br>

# NVIDIA-Docker

NVIDIA-docker을 사용하면 GPU 가속 docker 컨테이너를 빌드하고 실행할 수 있습니다. 컨테이너 런타임 라이브러리 및 NVIDIA GPU를 활용하도록 컨테이너를 자동으로 구성하는 유틸리티가 포함되어 있습니다. 

NVIDIA-docker를 사용하기 위해선 컴퓨터에 NVIDIA Graphic driver가 설치되어 있어야 합니다. 즉, NVIDIA-docker는 CUDA를 컨테이너 별로 만들 수 있는거지, driver를 버전별로 설치하진 못합니다. 다행히도 NVIDIA graphic driver를 최신버전으로 깔면 하위 버전은 호환이 되서 큰 문제는 없습니다. 

이렇게 깔리 NVIDIA-docker 위에 cuda 혹은 tensorflow나 pytorch를 버전별로 설치해 환경을 관리 할 수 있습니다.

> NVIDIA-docker

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2Fc3NLSP%2Fbtqve5rtE1z%2FjlYxw033xN6QknkhXee5F0%2Fimg.png)

[NVIDIA-docker](https://github.com/NVIDIA/nvidia-docker) 에서 자세한 설명을 해줍니다.

<br>

**참고 강의**

[시작하세요! docker/쿠버네티스](http://www.yes24.com/Product/Goods/84927385)

[서버 운영 시스템의 변화 과정](https://www.youtube.com/watch?v=WxzWXqTNdlw)

[What is docker](https://nickjanetakis.com/blog/understanding-how-the-docker-daemon-and-docker-cli-work-together)