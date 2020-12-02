---
layout: post
title: 딥러닝을 위한 Rancher 2 - Rancher 구조
tags: [Full Stack Deep Learning]
use_math: true
---

# Rancher Architecture

아래 그림은 Rancher 2.x의 아키텍처를 보여줍니다. 예시에선 두 개의 downstream kubernetes 클러스터를 관리하는 Rancher Server를 보여줍니다. 하나는 RKE에서 생성하고 다른 하나는 Amazon EKS (Elastic Kubernetes Service)에서 생성합니다.

> Rancher 2.0 Architecture

<center><img src="https://user-images.githubusercontent.com/31475037/95338344-ca587c00-08ed-11eb-9864-dc64b38d91be.png"></center>

<br>

## Downstream User Cluster와 통신

Rancher가 앱과 서비스를 실행하는 downstream user cluster를 프로비저닝하고 관리하는 방법을 보여줍니다. 

아래 그림은 cluster controller, cluster agent를 통해 Rancher Server가 downstream cluster를 제어하는 방법을 보여줍니다.

> 다운 스트림과의 통신

<center><img src="https://user-images.githubusercontent.com/31475037/95338349-cb89a900-08ed-11eb-833b-3e7460765b17.png"></center>



### Cluster Controller

Rancher server에 있으며, downstream cluster를 관리합니다.

- downstream cluster의 리소스 관리
- downstream cluster의 **current state**를 **desired state**로 만듬
- cluster에 대한 액세스 정책을 구성
- RKE나 클라우드 기반의 GKE와 같은 cluster를 프로비저닝함

### Cluster Agent

Rancher server가 downstream cluster와 통신 할 수 있도록 **cluster controller**는 **cluster agent**에 연결됩니다.

**cattle cluster agent**라고도하는 **cluster agent**는 downstream cluster에서 실행됩니다. 

다음 작업을 수행합니다.

- cluster controller가 만들라고 지시한 downstream cluster와 Rancher API Server에 연결
- 각 cluster 내 워크로드와  pod 생성 및 배포 관리
- 이벤트, 통계, 노드 정보에 대해 cluster와 Rancher API 서버와 통신

<br>

**참조 강의**

[Rancher doc](https://rancher.com/docs/rancher/v2.x/en/overview/architecture/)

