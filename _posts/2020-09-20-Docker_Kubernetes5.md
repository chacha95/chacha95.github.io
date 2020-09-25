---
layout: post
title: 딥러닝을 위한 kubernetes4 - 어떻게 동작할까?
tags: [Backend]
use_math: true
---

# 백그라운드에서 일어난 동작 이해하기

이전 포스트들에서 쿠버네티스의 개념과 구성 요소들에 대해 알아보았는데요, 그렇다면 쿠버네티스는 어떻게 동작할까요?

<br>

## 쿠버네티스의 작동방식

쿠버네티스에서 가장 중요한 것은 **desired state** 라는 개념입니다. Desired state란 개발자가 원하는 상태를 의미하며, 구체적으론 애플리케이션의 종류, 스펙, 몇 번 포트로 서비스하기를 원하는지 등을 의미합니다.

쿠버네티스는 **current state**를 모니터링하면서 관리자가 설정한 **desired state**를 유지하려 합니다. 그렇기에 개발자가 서버를 배포할 때 직접적인 명령어를 보내지 않고, 상태를 기술하는 방식을 사용합니다. 대표적으론 yaml 파일로 기술된 앱 디스크립션 파일이 있습니다.

![](https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/desired-state.png)



## 쿠버네티스 기술하기

가장 개발자가 해야할 것은 앱 디스크립터를 기술하는 것입니다. 왜냐면 쿠버네티스가 오브젝트를 생성할 때, 오브젝트에 대한 기본 정보와 더불어, desired state를 기술한 오브젝트 spec이 필요합니다.

다음은 가장 간단한 예제입니다.

```yaml
apiVersion: v1              # 쿠버네티스 버전
kind: Pod                   # 오브젝트 종류
metadata:
    name: chacha-simple     # pod 이름
spec:
    containers:
    - image: chacha/exp     # 컨테이너 이미지
      name: cha             # 생성될 컨테이너 이름
      ports:
      - containerPort: 8080 # 컨테이너 포트
        protocol: TCP
status:
    conditions:
    - status: "True"
      type: Ready
    containerStatuses:
    - containerID: docker://f03234
      image: chacha/exp
      imageID: docker://43cfsc32
      name: cha
      ready: True
      restartCount: 0
      state:
          running: 
                  startedAt: 2020-09-18T12:46:05z
    hostIP: 10.132.0.4
    phase: Running
    podIP: 10.0.2.3
    startTime: 2020-09-18-T12:46:05z
```

**Metadata**: 이름, 네임스페이스, 레이블, 파드에 대한 정보를 기술

**Spec**: pod, 컨테이너, 볼륨, 기타 데이터에 대해 기술(**desired status**)

**Status**: pod 상태, 컨테이너에대한 설명과 상태, pod 내부 IP 등 현재 실행 중인 pod에 관한 현재 정보(**current state**) 

쿠버네티스 deployment는 클러스터에서 동작하는 애플리케이션을 표현해줄 수 있는 오브젝트입니다. Deployment를 생성할 때, spec에 3개의 레플리카가 동작되도록 설정할 수 있습니다. 쿠버네티스 시스템은 spec을 읽어, spec에 일치되도록 상태를 업데이트하여 3개의 의도한 애플리케이션 인스턴스를 구동합니다. 

만약, 그 인스턴스들 중 어느 하나가 문제로 인해 멈춘다면(상태 변화 발생), 쿠버네티스 시스템은 새 pod 생성을 통해 desired status(spec)와 current status(status)간의 차이에 대응합니다.

<br>

## 전체 파이프라인

쿠버네티스의 전체 파이프라인은 다음과 같습니다.

> 전체 파이프라인

![](https://user-images.githubusercontent.com/31475037/94216967-c571f080-ff1b-11ea-910e-8ed403b6db30.png)

### 1. YAML, JSON 디스크립터로 pod 생성(앱 디스크립터)

YAML파일에 쿠버네티스 오브젝트들의 Spec을 정의합니다.

### 2. 이미지를 docker hub에 push

쿠버네티스에서 애플리케이션을 실행하려면, 애플리케이션을 docker image로 만들어 도커 허브에 push해야 합니다.

### 3. kubectl에게 명령

Kubectl 명령어를 실행하면 kube-apiserver에 REST API로 yaml로 작성된 애플리케이션 디스크립션이 전송됩니다. kubectl이 kube-apiserver에 yaml 파일 전송시, JSON 형식으로 정보를 변환시켜 전송합니다.

### 4. kubectl이 kube-apiserver 호출

Kube-apiserver가 앱 디스크립션을 읽고난뒤, kube-apiserver는 kube-scheduler에게 각 컨테이너에 필요한 리소스를 계산하라고 명령을 내립니다. 

### 5. pod과 노드 생성  스케쥴링

Kube-scheduler는 사용 가능한 노드에 pod을 할당합니다. 만약 사용가능한 pod이 없다면, 클러스터에 새로운 레플리카셋컨트롤러 오브젝트 생성, 레플리카셋컨트롤러는 새 pod을 생성합니다.

### 6. kube-apiserver가 노드의 kubelet에게 명령

해당 노드의 kubelet은 pod이 스케쥴링 됐다는 것을 전달받습니다.

### 7. kubelet은 container runtime에 이미지를 실행하라 명령

kubelet이 container runtime(도커)에게 이미지를 실행하라 명령합니다.

### 8. 노드에 도커 이미지가 없다면 도커 허브에서 pull 해옴(optional)

이미지가 로컬에 없다면, 도커에게 도커 허브에서 이미지를 pull해 옵니다. 이미지를 다운한 후 도커는 컨테이너를 생성 후 실행시킵니다.

<br>

위의 모든 과정에서 유저가 컨테이너 및 pod을 직접 생성하지 않습니다. 단지 앱 디스크립터에 기술할 뿐이죠. 유저가 kubectl을 이용해 run 명령을 내리면, 레플리케이션컨트롤러를 생성, 레플리케이션컨트롤러가 실제 pod을 생성합니다. 즉, 앱 디스크립터를 잘 기술하고, 도커 허브에 이미지만 올바르게 업로드하면 쿠버네티스가 모든 작업을 처리해줍니다.



<br>

**참고 자료**

[쿠버네티스란 무엇인가?](https://kubernetes.io/ko/docs/concepts/overview/what-is-kubernetes/)

[subicura 블로그](https://subicura.com/2019/05/19/kubernetes-basic-1.html)