---
layout: post
title: 딥러닝을 위한 Rancher 1 - overview
tags: [Backend, Full Stack Deep Learning, Kubeflow]
use_math: true
---

# Rancher?

Kubernetes는 이제 컨테이너 management tool에서 de facto(업계 표준)가 되었습니다. 이런 상황에서 Rancher는 Kubernetes를 위한 컨테이너 관리 플랫폼으로 시작되었습니다.

<br>

# Rancher 2.0

Rancher는 **"Deliver Kubernetes-as-a-Service"**라는 슬로건으로 표현 가능합니다.

Rancher 사용자는 Rancher Kubernetes Engine (RKE) 또는 클라우드 Kubernetes 서비스(GKE, AKS, EKS)를 사용하여 Kubernetes 클러스터를 만들 수 있습니다. 

Rancher 사용자는 Kubernetes 배포 또는 설치 프로그램을 사용하여 생성 된 기존 Kubernetes 클러스터를 가져오고 관리 할 수도 있습니다.

하나의 Rancher 서버 설치는 동일한 UI에서 수천 개의 Kubernetes 클러스터와 수천 개의 노드를 관리 할 수 있습니다.

Rancher 2.0 구조는 아래와 같습니다.

> Rancher2.0 Architecture

![](https://user-images.githubusercontent.com/31475037/95277257-55069000-0888-11eb-9bdd-a5c9051f5cb9.png)

<br>

## 언제 써야 할까?

개발 환경에서 웹-인터넷 접속이 느리고 방화벽으로 막혀있는환경

규정상 특정 클라우드에 종속적이지않고싶다

On-premise 환경에서 Kubernetes를 쓰거나 On-premise와 여러 Cloud 벤더를 섞어서 써야할 때

<br>

## Rancher for Dev

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

Rancher는 제어되는 모든 Kubernetes 클러스터에 대해 RBAC 기반 중앙 집중식 인증, 액세스 제어, 모니터링, 로깅을 지원합니다.

Rancher는 DevOps 엔지니어가 애플리케이션 워크로드를 관리 할 수있는 직관적인 UI를 제공합니다.

사용자는 Rancher 사용을 위해 Kubernetes에 대한 깊은 지식이 필요치 않습니다.

Rancher 카탈로그에는 보안, 모니터링, 컨테이너, 스토리지, 네트워킹 드라이버 등 Helm을 이용해 다양한 클라우드 네이티브 에코 시스템 툴들을 제공합니다.

### Rancher Kubernetes Engine (RKE)

On-Premise 서버에 Kubernetes 클러스터를 설치하는 가장 쉬운 방법

### 통합 클러스터 관리

GKE, EKS, AKS와 같은 클라우드 Kubernetes 클러스터는 물론 On-premise로 설치된 Kubernetes 클러스터까지 통합 관리 해줍니다.

### Kubernetes 클러스터 프로비저닝

Rancher API 서버는 기존 노드에서 Kubernetes를 프로비저닝하거나 Kubernetes 업그레이드를 수행 할 수 있습니다.

### 보안 및 계정 관리

**사용자 관리**

Rancher API 서버는 로컬 사용자뿐 아니라 Active Directory 또는 GitHub와 같은 외부 인증 공급자에 해당하는 사용자 ID를 관리합니다.

**인증**

Rancher API 서버는 액세스 제어 및 보안 정책을 관리합니다.

### 앱 카탈로그

앱 카탈로그를 Helm 연동을 통해서 지원

Rancher는 반복적으로 애플리케이션을 쉽게 배포 할 수 있도록 Helm 차트 카탈로그 기능을 제공합니다.

### 프로젝트 관리

프로젝트는 클러스터 내 여러 네임 스페이스 및 액세스 제어 정책의 그룹입니다. 프로젝트는 Kubernetes 개념이 아닌 Rancher 개념으로, 여러 네임 스페이스를 그룹으로 관리하고 그 안에서 Kubernetes 작업을 수행 할 수 있습니다. 

### 파이프 라인

파이프 라인을 설정하면 개발자가 가능한 빠르고 효율적으로 새로운 소프트웨어를 제공 할 수 있습니다. Rancher 내에서 각 Rancher 프로젝트에 대한 파이프 라인을 구성 할 수 있습니다.(외부 CI / CD 툴을 쓸 수도 있음)

### Istio

Istio와의 통합은 Rancher 운영자가 Istio를 개발자에게 제공합니다.

### 모니터링

프로메테우스, 그라파나 기반 모니터링 제공

Prometheus와의 통합을 통해 클러스터 노드, Kubernetes 구성 요소 및 소프트웨어 배포의 상태와 프로세스를 모니터링 할 수 있습니다. 

### Alert(경고)

지표에 따른 알람 기능도 제공한다.

클러스터와 애플리케이션을 정상 상태로 유지하는 alert 기능을 제공합니다.

### 로깅

자체 로깅 시스템은 없고, 외부 로깅 시스템과 연동

ELK, Splunk를 지원하며, Kafka를 통해 로그를 빼내거나 Syslog/fluentd를 이용해서 로그 서버에 전송

Rancher는 Kubernetes 클러스터 외부에 존재하는 다양한 로깅 도구를 사용 가능합니다

<br>

#### 용어 설명

**프로비저닝**

사용자의 요구에 맞게 시스템 자원을 할당, 배치, 배포해 두었다가 필요 시 시스템을 즉시 사용할 수 있는 상태로 미리 준비해 두는 것을 말한다.

**워크로드**

워크로드란 고객 대면 애플리케이션이나 백엔드 프로세스 같이 비즈니스 가치를 창출하는 리소스 및 코드 모음을 말합니다.

<br>

**참조 강의**

[AWSKRUG 컨테이너 소모임 Rancher 기본 입문](https://www.slideshare.net/HyunminKim5/awskrug-rancher)

[Rancher doc](https://rancher.com/products/)