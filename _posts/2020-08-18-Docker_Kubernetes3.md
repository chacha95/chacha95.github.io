---
layout: post
title: 딥러닝을 위한 kubernetes2 - 기초 구성
tags: [Backend]
use_math: true
---

# 쿠버네티스 구성

쿠버네티스 클러스터는 노드(Node) 컴포넌트와 컨트롤 플레인(Master였으나 이름이 변경됨) 컴포넌트로 구성됩니다. 쿠버네티스를 배포하면 클러스터를 얻게 됩니다. 쿠버네티스 클러스터는 컨테이너화된 애플리케이션을 실행하는 노드라고 하는 워커 머신의 집합입니다. 모든 클러스터는 최소 한개의 워커 노드를 가집니다. 워커 노드는 애플리케이션의 구성요소인 pod을 호스트합니다. 컨트롤 플레인은 워커 노드와 클러스터 내 pod들을 관리합니다.

> k8s 구성

![](https://user-images.githubusercontent.com/31475037/90489252-fe2de580-e177-11ea-8c1d-9848ecebd805.png)

## K8S Object

Object란 k8s에 올라가는 모든 컴포넌트들을 object라 칭합니다. 이 object들을 정의하는 yaml 포맷의 manifest 파일을 정의함으로서 사용합니다.

대표적인 object들은 다음과 같습니다.

> k8s의 object들

![](https://user-images.githubusercontent.com/31475037/90489250-fd954f00-e177-11ea-961f-c5bf3793e495.png)

<br>

## Basic Object

### Pod

워커 노드에서 실행되는 컨테이너들의 집합입니다. 하나의 pod에서 한 개 이상의 서비스가 지정됩니다. 각각의 pod은 고유 ip가 할당 됩니다.(내부 ip) 

하나의 pod 내에서는 PID namespace, network, hostname 공유합니다. 즉, 하나의 pod 내에서는 서로 다른 컨테이너의 PID가 보이며 같은 network, 같은 hostname을 사용합니다.

> pod의 구조

<center><img src="https://user-images.githubusercontent.com/31475037/90489248-fd954f00-e177-11ea-9e01-098a4ac132c5.png" width=80%></center>



### Service

Pod은 같은 pod끼리는 호출이 되지만 이를 외부에선 접속이 불가능합니다. 이를 해결하기 위해서 service 사용합니다.  Label selector로 선택하여 하나의 endpoint로 노출되는 pod의 집합입니다.

### IP 노출

외부에 IP를 노출 시키는 기능으로는 Ingress를 이용한 방법과 NodePort 두가지 방법이 존재합니다.

**Ingress**

k8s pod의 컨테이너들을 외부에서 접속 할 수 있도록 일종의 proxy 서버를 이용해 접근하는 방식이며, 이 proxy 서버를 ingress 서버라하며, Nginx를 비롯한 여타의 웹 서버로 구성 가능합니다.

> Ingress

![](https://user-images.githubusercontent.com/31475037/90489247-fcfcb880-e177-11ea-9823-f778ba444407.png)

**NodePort**

k8s pod의 컨테이너들을 외부에서 접속할 수 있도록 IP를 노출합니다.

> NodePort

![](https://user-images.githubusercontent.com/31475037/90489245-fc642200-e177-11ea-8f4b-8a1c0ff897df.png)

### Secret

컨테이너에서 읽혀지거나 사용되는 소량의 민감 정보들입니다. 특수한 volume이 자동으로 연결되어 container에서 사용 가능하며, 수동으로 secret을 생성, 연결이 가능합니다. Base64로 encoding 될 뿐 암호화 되지는 않습니다.

<br>

## Controler Object

### Deployment Controller

pod의 배포 및 관리에 사용됩니다. ReplicaSet을 자동으로 생성하며, pod에 대한 rolling-update 관리합니다.

### ReplicaSet

Replication controller의 새로운 버전입니다. 가용성과 확장성 보장하며, 사용자의 요청에 따른 pod의 숫자를 유지 및 관리합니다. 각각의 pod이 필요로 하는 정보 명세가 있는 template 이용해 정의합니다.

> ReplicaSet의 예시

![](https://user-images.githubusercontent.com/31475037/90489243-fbcb8b80-e177-11ea-9bba-b6c77065af51.png)



### Job

특정 task 실행을 위해 하나 혹은 이상의 pod을 생성, 실행 pod이 실행 완료하는 것을 보장(실패시 재시도, deadline 설정 가능)합니다. Job 실행시 pod의 순차 실행 또는 동시 실행이 가능합니다. 모든 pod 실행이 완료됬을시 job의 완료로 인식, 사용한 pod을 제거합니다.

<br>

## Storages

### Volume(임시적인 데이터 저장)

pod에 연결되어 디렉토리 형태로 데이터를 저장 할 수 있도록 제공합니다. Pod의 container들 끼리 공유가능하며, pod의 life-cycle과 일치하여 pod 삭제시 같이 삭제됩니다.

### Persistence Volume(영구적 데이터 저장)

k8s 클러스터 관리자에 의해 제공된 저장소의 일부입니다. volume과 유사하지만 pod과는 독립적인 life-cycle을 가집니다. 사용자가 용량, 모드 필요한 정보와 함께 PVC(PersistenceVolumeClaim)를 생성하면 이에 대응하는 Persistence Volume이 생성됩니다.

<br>

**참고 강의**

[k8s](https://kubernetes.io/ko/docs/concepts/overview/what-is-kubernetes/#:~:text=%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4%EB%8A%94%20%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88%ED%99%94,%EC%9E%90%EB%8F%99%ED%99%94%EB%A5%BC%20%EB%AA%A8%EB%91%90%20%EC%A7%80%EC%9B%90%ED%95%9C%EB%8B%A4.)