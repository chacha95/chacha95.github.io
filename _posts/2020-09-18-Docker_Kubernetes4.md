---
layout: post
title: 딥러닝을 위한 kubernetes3 - 구성 요소
tags: [Backend]
use_math: true
---

쿠버네티스의 구성요소는 크게 컴포넌트와 object가 있습니다.

# 쿠버네티스 컴포넌트

쿠버네티스 컴포넌트는 크게 컨트롤 플레인과 노드로 이루어져 있습니다.

<br>

## 컨트롤 플레인

컨트롤 플레인은 클러스터에 대한 유지 및 제어를 하지만, 애플리케이션을 실행하진 않습니다. 컨트롤 플레인은 클러스터 내 어떤 머신에서든 동작 가능하지만, 보통 동일 머신 상에 모든 컨트롤 플레인을 구동시킵니다.

컨트롤 플레인은 다음 요소들로 구성됩니다.

> 컨트롤 플레인 (컴포넌트) 구성 요소

![](https://user-images.githubusercontent.com/31475037/93835893-bdb41100-fcbb-11ea-8e44-9ddc76875db0.png)

### kube-apiserver

kube-apiserver는 쿠버네티스 API를 노출합니다. API 서버는 쿠버네티스 컨트롤 플레인의 프론트 엔드로서 사용자와 컨트롤 플레인 내의 요소들과 통신합니다. 또한 수평적(scale out)으로 확장가능케 설계되었습니다. 여러 kube-apiserver 인스턴스를 실행하고, 인스턴스간 트래픽을 조절합니다.

### etcd

모든 클러스터 데이터를 저장하는 분산 데이터 저장소(storage)로서 고가용성 key-value 저장소입니다.

### kube-scheduler

새로 생성된 pod에게 노드를 배정합니다. 이를 통해 pod이 애플리케이션을 실행할 수 있도록 배포합니다.

### kube-controller-manager

컨트롤러에는 ReplicaSet, Deployment, StatefulSet, DaemonSet 등이 있습니다.

**ReplicaSet**

Pod들이 늘어날 때는 어떻게 관리해야 되는가?

같은 역할을 하는 Pod들을 한 번에 ReplicaSet으로 관리한다.

**Deployment**

Pod의 개수는 ReplicaSet으로 줄였다 늘렸다 하는데, 이 ReplicaSet을 관리하는 것이 Deployment

Deployment를 만들었을 때 특정 버전에 대한 ReplicaSet이 나오고, 새 버전이 나오면 ReplicaSet을 새로 만든다.

다른 ReplicaSet이지만, 같은 Deployment로 관리한다.

Deployment를 만들게 되면, Deployment가 ReplicaSet을 만들고, Pod을 만들게 된다.

**StatefulSet**

Pod에 Identity(name, network id 등)를 고정시킨다. 죽었다 띄워도 똑같이 띄운다.

**DaemonSet**

모든 노드에 pod이 1개씩 실행

<br>

## 노드

노드는 동작 중인 파드를 유지, 컨테이너화된 애플리케이션을 실행합니다.

> 노드 (컴포넌트)의 구성 요소

![](https://user-images.githubusercontent.com/31475037/93835878-b2f97c00-fcbb-11ea-8e47-98c5a1ab0ed6.png)

### kubelet

각각의 노드에서 실행되며, 컨트롤 플레인의 kube-apiserver와 통신하고 파드에서 컨테이너가 동작하도록 관리합니다.

Kubelet은 다양한 앱 디스크립터를 통해 만들어진 PodSpec을 받아 컨테이너가 해당 pod 스펙에 따라 동작하도록 합니다.

### kube-proxy

애플리케이션 구성 요소간 네트워크 트래픽을 로드밸런싱 해줍니다. 즉, **pod에 요청이 들어왔을 때 어떤 Pod한테 요청을 전달할 것인가?**를 결정합니다.

### container runtime

컨테이너 실행을 담당하는 소프트웨어 입니다. 보통 docker를 씁니다.

<br>

# 쿠버네티스 오브젝트

오브젝트는 사용자가 쿠버네티스에 바라는 상태(desired state)를 의미하고 컨트롤러는 객체가 원래 설정된 상태를 유지 및 관리하는 역할을 합니다.  

오브젝트에는 pod, service, volume, namespace 등이 있습니다.

쿠버네티스 클러스터에 객체나 컨트롤러가 어떤 상태여야 하는지를 제출할때는 yaml 파일형식의 템플릿을 사용합니다. 

오브젝트를 중복 없이 식별하기 위해 쿠버네티스 시스템은 **UID**라는 문자열을 생성합니다. 쿠버네티스 클러스터가 구동되는 전체 시간에 걸쳐 생성되는 모든 오브젝트는 서로 구분되는 UID를 갖습니다.

<br>

## Pod

쿠버네티스 오브젝트 중에서 pod이 가장 중요합니다.

쿠버네티스는 개별 컨테이너들을 직접 다루지 않고, 함께 배치된 다수의 컨테이너를 pod이란 곳에 묶어 관리합니다. 각 **pod은 자체 IP, host name, process**이 있는 논리적으로 분리된 머신입니다. Pod은 프로세스를 실행하는 하나 이상의 컨테이너를 가지고 있습니다.

pod은 일시적이며 언제든 사라질 수 있습니다.

<br>

## 레이블

레이블은 쿠버네티스 오브젝트를 계층화합니다.

키-값 쌍으로 이뤄진 레이블은 레이블 셀렉터를 사용해 리소스를 선택할 때 활용됩니다. 일반적으로 리소스를 생성할 때  레이블을 붙입니다. 특히, 레이블은 오브젝트의 특성을 식별하는 데 사용되어 사용자에게 중요하지만, 코어 시스템에 직접적인 의미는 없습니다. 오브젝트마다 키와 값으로 레이블을 정의할 수 있으며 오브젝트의 키는 고유한 값이어야 합니다.

```yaml
"metadata": {
  "labels": {
    "key1" : "value1",
    "key2" : "value2"
  }
}
```

### 레이블 셀렉터

일반적으로 우리는 많은 오브젝트에 중복되는 이름을 붙입니다. 레이블 셀렉터를 통해 클라이언트와 사용자는 오브젝트를 식별할 수 있습니다.

<br>

## Service

우리가 pod을 만들게 되면, pod끼리는 내부에서 서로 접속이 가능하지만 외부에선 접속이 불가능합니다. 내부/외부에서 둘다 접속하고자 할 때는 service를 사용합니다. Pod에 Label을 붙이고, Label Selector로 선택하여 하나의 endpoint로 노출합니다. 

Service를 사용하면 크게 두가지 이점이 있습니다.

### IP, Port 관리

pod이 죽었을 때, pod은 레플리케이션컨트롤러에 의해 새로운 pod으로 대체됩니다. 새로운 pod은 새로운 IP와 port를 부여받습니다. 

### 단일 endpoint

Service가 생성되면 static ip를 할당받고 service가 존재하는 동안 변경되지 않습니다. pod에 직접 연결하는 대신 클라이언트는 service의 IP주소와 port를 통해 연결됩니다.

> Service 예시

![](https://user-images.githubusercontent.com/31475037/93834302-58a9ec80-fcb6-11ea-8b0c-12b058570027.png)

<br>

## Storages

### Volume(임시적인 데이터 저장)

컨테이너 내 디스크에 있는 파일은 임시적이며, 컨테이너에서 실행될 때 애플리케이션에 문제가 발생한다. 

첫째, 컨테이너가 충돌되면, kubelet은 컨테이너를 재시작시키지만, 컨테이너는 이전의 상태로 시작되기 때문에 기존 파일이 유실된다. 

둘째, pod에서 컨테이너를 여러개 실행할 때 컨테이너 사이에 파일을 공유해야 하는 경우가 자주 발생한다.

pod에 연결되어 디렉토리 형태로 데이터를 저장 할 수 있도록 제공합니다.

 Pod의 container들 끼리 공유가능하며, pod의 life-cycle과 일치하여 pod 삭제시 같이 삭제됩니다.

### Persistent Volume(영구적 데이터 저장)

k8s 클러스터 관리자에 의해 제공된 저장소의 일부입니다. volume과 유사하지만 pod과는 독립적인 life-cycle을 가집니다. 사용자가 용량, 모드 필요한 정보와 함께 PVC(PersistenceVolumeClaim)를 생성하면 이에 대응하는 Persistence Volume이 생성됩니다.

k8s 클러스터 관리자가 관리

Volume하고 비슷한데 Pod하고 독립적

영구적으로 유지되어야 하는 데이터는 PV로 저장하게 된다.

PVC(PersistentVolumeClaim), PVC를 갖다 쓸거야 라고 Claim을 해서 사용할 수 있다.

<br>

**참고 자료**

[쿠버네티스란 무엇인가?](https://kubernetes.io/ko/docs/concepts/overview/what-is-kubernetes/)

[k8s](https://kubernetes.io/ko/docs/concepts/overview/components/)

