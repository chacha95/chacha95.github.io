---
layout: post
title: 딥러닝을 위한 kubernetes4 - 기능
tags: [Backend]
use_math: true
---

# 쿠버네티스에서 애플리케이션 실행

쿠버네티스에서 애플리케이션을 실행하려면 먼저 애플리케이션을 하나 이상의 컨테이너 이미지로 패키징하고 해당 이미지를 이미지 레지스트리로 푸시한 다음 컨트롤 플레인에 있는 API서버에 yaml로 작성된 애플리케이션 디스크립션을 올려야함

API서버가 앱 디스크립션을 읽고난뒤, 스케쥴러는 각 컨테이너에 필요한 리소스를 계산한다. 스케쥴러는 사용 가능한 노드에 pod을 할당한다. 그런 다음 해당 노드의 kubelet은 컨테이너 런타임에 필요한 컨테이너 이미지를 docker hub로부터 가져와 컨테이너를 실행한다.

<br>

## 백그라운드에서 일어난 동작 이해하기

유저가 컨테이너를 직접 생성, 동작하지 않습니다.

유저가 kubectl을 이용해 run 명령을 내리면, 레플리케이션컨트롤러를 생성, 레플리케이션 컨트롤러가 실제 pod을 생성합니다. 또한 클러스터 외부에서 pod에 접근케 하기 위해 쿠버네티스에게 레플리케이션컨트롤러에 의해 관리되는 모든 pod을 service로 외부에 노출되게 합니다. 

> 전체 파이프라인

![](https://user-images.githubusercontent.com/31475037/93834207-fd77fa00-fcb5-11ea-9da0-ed58a14c8979.png)

### 1. YAML, JSON 디스크립터로 pod 생성(앱 디스크립터)

YAML파일에 모든 쿠버네티스 오브젝트를 정의하면 버전 관리 시스템에 넣는 것이 가능함

### 2. 이미지를 docker hub에 push

사용자가 원하는 docker image를 도커 허브에 push함

### 3. kubectl에게 명령

kubectl 명령어를 싱행하면 쿠버네티스의 API 서버로 REST API로 요청

### 4. kubectl이 kube-apiserver 호출

### 5.pod과 노드 생성  스케쥴링

클러스터에 새로운 레플리케이션 컨트롤러 오브젝트 생성

레플리케이션 컨트롤러는 새 pod을 생성하고 스케쥴러에 의해 워커 노드 중 하나에 스케쥴링이 됨

### 6. kube-apiserver가 노드의 kubelet에게 명령

해당 워커 노드의 kubelet은 pod이 스케쥴링 됐다는 것을 확인

### 7. kubelet은 도커에 이미지를 실행하라 명령

kubelet이 도커에게 이미지를 실행하라 명령

### 8. 노드에 도커 이미지가 없다면 도커 허브에서 pull 해옴

이미지가 로컬에 없기 때문에 도커에게 레지스트리에서 특정 이미즈를 pull해 옴

이미지를 다운한 후 도커는 컨테이너를 생성, 실행

<br>

## 쿠버네티스 오브젝트 기술하기

쿠버네티스에서 오브젝트를 생성할 때, 오브젝트에 대한 기본적인 정보와 더불어, 의도한 상태를 기술한 오브젝트 spec을 제시해 줘야만 한다. 

**대부분의 경우 정보를 .yaml 파일로 kubectl에 제공한다.**

**kubectl**은 API 요청이 이루어질 때, JSON 형식으로 정보를 변환시켜 kube-apiserver에 요청합니다.

```yaml
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

<br>

**Metadata**: 이름, 네임스페이스, 레이블, 파드에 대한 정보

**Spec**: pod 컨테이너, 볼륨, 기타 데이터 등 파드 자체에 관한 실제 명세

**Status**: pod 상태, 각 컨테이너 설명과 상태, pod 내부 IP, 기타 기본 정보 등 현재 실행 중인 pod에 관한 현재 정보 

예를 들어, 쿠버네티스 디플로이먼트는 클러스터에서 동작하는 애플리케이션을 표현해줄 수 있는 오브젝트이다. 디플로이먼트를 생성할 때, 디플로이먼트 spec에 3개의 애플리케이션 레플리카가 동작되도록 설정할 수 있다. 쿠버네티스 시스템은 그 디플로이먼트 spec을 읽어 spec에 일치되도록 상태를 업데이트하여 3개의 의도한 애플리케이션 인스턴스를 구동시킨다. 만약, 그 인스턴스들 중 어느 하나가 어떤 문제로 인해 멈춘다면(상태 변화 발생), 쿠버네티스 시스템은 보정(이 경우에는 대체 인스턴스를 시작하여)을 통해 spec과 status간의 차이에 대응한다.

<br>

## Pod 수평 확장

실행중인 앱은 레플리케이션컨트롤러에 의해 모니터링되고, 실행되며 서비스를 통해 외부에 노출됨

쿠버네티스의 주요 이점 중 하나는 간단하게 배포를 확장 가능, 즉 pod 개수를 간단히 늘림

pod의 레플리카 수를 늘리려면 레플리카 컨트롤러에서 의도하는 레플리카 수를 변경해야 함

<br>

**참고 자료**

[쿠버네티스란 무엇인가?](https://kubernetes.io/ko/docs/concepts/overview/what-is-kubernetes/)

