---
layout: post
title: 딥러닝을 위한 Rancher 1 - overview
tags: [Backend, Full Stack Deep Learning, Kubeflow]
use_math: true
---

# Rancher?

Kubernetes는 컨테이너 오케스트레이션 기술에서 de facto가 되었습니다. 이런 환경 속에서 Rancher는 Kubernetes를 위한 컨테이너 관리 플랫폼입니다.

Rancher 사용자는 Rancher Kubernetes Engine (RKE) 또는 클라우드 Kubernetes 서비스(GKE, AKS, EKS)를 사용하여 Kubernetes 클러스터를 만들 수 있습니다. 

Rancher 사용자는 Kubernetes 배포 또는 설치 프로그램을 사용하여 생성 된 기존 Kubernetes 클러스터를 가져오고 관리 할 수도 있습니다.

하나의 Rancher 서버 설치는 동일한 UI에서 수천 개의 Kubernetes 클러스터와 수천 개의 노드를 관리 할 수 있습니다.

<br>

### 언제 써야 할까?

개발 환경에서 웹-인터넷 접속이 느리고 방화벽으로 막혀있는환경

규정상 특정 클라우드에 종속적이지않고싶다

On-premise 환경에서 Kubernetes를 쓰거나 On-premise와 여러 Cloud 벤더를 섞어서 써야할 때

<br>

# Rancher 2.0

**"Deliver Kubernetes-as-a-Service"**라는 슬로건!

Rancher Labs는 기업이 모든 인프라에서 Kubernetes-as-a-Service를 제공하는 데 도움이되는 소프트웨어를 구축합니다.

### Rancher Kubernetes Engine (RKE)

베어메탈 서버 및 클라우드에 Kubernetes 클러스터를 설치하는 가장 쉬운 방법

### 통합 클러스터 관리

GKE, EKS 및AKS와 같은 클라우드 관리형 Kubernetes 클러스터는 물론 On-premise로 설치된기존 Kubernetes 클러스터 통합 관리

### 응용 프로그램 워크 로드 관리

Rancher 2.0은 조직에서 Kubernetes의 채택을 촉진하는 직관적인 UI 제공합니다. 다른 기능으로는 Helm 3.0 지원, Helm을 이용한 카탈로그 기능을 지원(The package manager for Kubernetes) 쉽게 kubernetes tool package 설치 가능, CI/CD 통합, Alert 기능 및 중앙 집중식 logging.

> Rancher2.0 Architecture

![](https://user-images.githubusercontent.com/31475037/95277257-55069000-0888-11eb-9bdd-a5c9051f5cb9.png)

<br>

## Rancher for Dev Overview

### Rancher

Rancher를 사용하면 Kubernetes를 쉽게 관리 할 수 있습니다. 이제 컨테이너, Istio 서비스 메시 및 클라우드 네이티브 워크로드에 대한 향상된 보안을 완벽하게 지원하는 Rancher는, 개발자가 더 빠르고 확실하게 혁신 할 수 있도록 지원합니다.

### RKE

RKE는 전적으로 컨테이너 내에서 실행되고 복잡한 설치에 대한 불만을 해결해줍니다. RKE를 사용하면 Kubernetes 운영이 쉽게 자동화되고 실행중인 운영 체제 및 플랫폼과 완전히 독립적입니다.

### K3S

K3s는 IoT 내부 또는 네트워크 edge에서 production 워크로드를 실행하기 위해 구축 된 경량 Kubernetes 입니다. K3s는 x86 및 Arm 프로세서 모두에 대해 40MB 미만 단일 바이너리로 패키징되어 있으며, Raspberry Pi는 물론이고, AWS a1.4xlarge 32GiB 서버와 같이 큰 서버에서도 잘 동작합니다.

### LONGHORN

Longhorn은 Kubernetes 용으로 구축된 오픈소스 분산 블록 스토리지입니다. Rancher Labs에서 시작했지만 Longhorn은 이제 CNCF가 관리하는 프로젝트입니다. 데이터를 관리하고 환경을 운영하는 데 필요한 리소스를 줄여줍니다.

<br>

## 주요 기능

### 계정 관리

일반 오픈소스 버전의 쿠버네티스는 계정 관리 시스템이 없고, 이를 외부 계정 관리 시스템과 연동해서 쓰도록 되어 있는데, Rancher를 사용할 경우 일반 계정 시스템을 자체적으로 가지고 있고(로컬 데이타 베이스에 저장한다.) 그리고 필요하면 외부 계정시스템 (AD, GitHub,LDAP)을 사용하거나 SAML/SSO 인증등을 지원한다.

### 앱 카달로그

앱 카달로그를 Helm 연동을 통해서 지원

### 모니터링

프로메테우스, 그라파나 기반 모니터링 제공

### Alert

지표에 따른 알람 기능도 제공한다.

### Notifier

Alert 을 어디로 보낼까 했더니, Notification을 등록하는 기능이 있다.

Notifier 등록화면을 보니 이메일 뿐만 아니라 Pager Duty, Slack 그리고 Webhook 도 지원한다.

### 로깅

자체 로깅 시스템은 없고, 외부 로깅 시스템과 연동

ELK, Splunk를 지원하며, Kafka를 통해 로그를 빼내거나 Syslog/fluentd를 이용해서 로그 서버에 전송

<br>

**참조 강의**

[AWSKRUG 컨테이너 소모임 Rancher 기본 입문](https://www.slideshare.net/HyunminKim5/awskrug-rancher)

[Products](https://rancher.com/products/)