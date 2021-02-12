---
layout: post
title: Rancher란?
tags: [MLOps]
use_math: true
---

# Rancher?

kubernetes는 이제 컨테이너 management tool에서 de facto(업계 표준)가 되었습니다. 이런 상황에서 Rancher는 kubernetes를 위한 컨테이너 관리 플랫폼으로 시작되었습니다. 

<br>

# Rancher 2.0

Rancher는 **"Deliver kubernetes-as-a-Service"**라는 슬로건으로 표현 가능합니다.

Rancher 사용자는 Rancher kubernetes Engine (RKE) 또는 클라우드 kubernetes 서비스(GKE, AKS, EKS)를 사용하여 kubernetes 클러스터를 만들 수 있습니다. Rancher 사용자는 kubernetes 기존 kubernetes 클러스터를 가져와 관리 할 수 있습니다. 하나의 Rancher 서버는 동일한 UI에서 수천 개의 kubernetes 클러스터와 수천 개의 노드를 관리 할 수 있습니다. 특히 사용자는 Rancher 사용을 위해 kubernetes에 대한 깊은 지식이 필요치 않습니다.

Rancher 2.0 구조는 아래와 같습니다.

> Rancher2.0 Architecture

![](https://user-images.githubusercontent.com/31475037/95277257-55069000-0888-11eb-9bdd-a5c9051f5cb9.png)



<br>

## Rancher for Dev

### Rancher

Rancher를 사용하면 kubernetes를 더 쉽게 관리 할 수 있습니다. 이제 컨테이너, Istio 서비스 메시에 대한 보안을 완벽하게 지원하는 Rancher는 개발자가 더 빠르게 개발 하도록 돕습니다.

### RKE

RKE(Rancher Kubernetes Engine)는 컨테이너 내에서 실행되며 간단히 설치됩니다. RKE를 사용하면 kubernetes 운영이 쉽게 자동화되며, 실행중인 OS 및 플랫폼과 완전히 독립적입니다.

### K3S

K3s는 IoT 또는 네트워크 edge에서 워크로드를 실행하기 위한 경량 kubernetes 입니다. K3s는 x86 및 Arm 프로세서 둘다 40MB 미만의 단일 바이너리로 패키징되어 있으며, Raspberry Pi는 물론이고 AWS a1.4xlarge 32GiB 서버와 같은 대형 서버에서도 잘 동작합니다.

### LONGHORN

LONGHORN은 kubernetes 용으로 구축된 오픈소스 분산 스토리지입니다. 

<br>

## 주요 기능

### 통합 클러스터 관리

GKE, EKS, AKS와 같은 클라우드 kubernetes 클러스터는 물론 On-premise로 설치된 kubernetes 클러스터까지 통합 관리 해줍니다.

### kubernetes 클러스터 프로비저닝

Rancher API 서버는 기존 노드에서 kubernetes를 프로비저닝하거나 kubernetes 업그레이드를 수행 할 수 있습니다.

### 보안 및 계정 관리

**인증**

Rancher API 서버는 액세스 제어 및 보안 정책을 관리합니다.

**사용자 관리**

Rancher API 서버는 로컬 사용자뿐 아니라 GitHub와 같은 외부 인증도 가능합니다.

### 앱 카탈로그

Rancher 카탈로그에는 **보안**, **모니터링**, **스토리지**, **네트워킹 드라이버** 등 Helm을 이용해 다양한 클라우드 네이티브 패키지들을 제공합니다. 

### 파이프 라인

파이프 라인을 설정하면 개발자가 효율적으로 소프트웨어를 CI / CD 할 수 있습니다. 

### 모니터링

프로메테우스, 그라파나 기반 모니터링을 Helm을 통해 설치해서 사용 가능합니다.  

### Alert(경고)

클러스터와 애플리케이션이 정상 인지 체크하는 alert 기능을 제공합니다.

### 로깅

자체 로깅 시스템은 없고, 외부 로깅 시스템과 연동합니다. 주로 Kafka를 이용해 로깅을 합니다.



<br>

## 언제 써야 할까?

- 특정 클라우드 벤더에 종속적이고 싶지 않을 때

- On-premise 환경에서 kubernetes를 쓰거나 On-premise와 여러 Cloud 벤더를 섞어서 써야할 때
- 단일 On-premise 환경에서 사용할 때(k3s 기반 간편히 설치 및 관리 가능)



<br>

**용어 설명**

*프로비저닝*

*사용자의 요구에 맞게 시스템 자원을 할당, 배치, 배포해 두었다가 필요 시 시스템을 즉시 사용할 수 있는 상태로 미리 준비해 두는 것을 말합니다.*

*워크로드*

*워크로드란 고객 대면 애플리케이션이나 백엔드 프로세스 같이 비즈니스 가치를 창출하는 리소스 및 코드 모음을 말합니다.*

<br>

**참조 강의**

[AWSKRUG 컨테이너 소모임 Rancher 기본 입문](https://www.slideshare.net/HyunminKim5/awskrug-rancher)

[Rancher doc](https://rancher.com/products/)