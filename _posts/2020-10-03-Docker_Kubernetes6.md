---
layout: post
title: 딥러닝을 위한 kubernetes 5 - MSA and Service Mesh
tags: [Backend, Full Stack Deep Learning]
use_math: true
---

# MSA와 Service Mesh

MSA(Micro Service Architecture)는 여러 장점을 가졌지만, 많은 단점도 가졌습니다. MSA는 기능을 서비스 단위로 잘게 나누다 보니, 시스템이 커지면 커질수록 서비스간의 관리가 힘들어집니다. 특히, 서비스간 전체적인 연결 구조를 파악하기 어려워, 이로 인해 장애가 났을때, 어느 서비스에서 장애가 났는지 추적이 어려워집니다.

이런 MSA의 단점을 개선하기 위해 service mesh 기술이 나왔습니다. Service mesh는 소프트웨어 계층이 아니라 인프라 측면에서 MSA의 문제를 해결하기 위해 나왔습니다.

 Service mesh 기술을 이해하기 위해선 다음 개념들을 알아야 합니다.

<br>

 ## 프록시 

프록시란 '**대리**'라는 의미로 네트워크 기술에서는 중요한 개념이며, 보통 서버의 대라자 역할을 하는 서버를 의미합니다.

예를들어 아래와 같이 서비스와 서비스간의 호출이 있을때

> service간 호출

<center><img src="https://user-images.githubusercontent.com/31475037/95031529-679a9100-06f1-11eb-8c8b-dc3d6d9ff5ef.png"></center>



서비스 A가 서비스 B를 직접 호출 하는 것이 아니라 서비스 마다 프록시를 넣어 간접적으로 호출하게 됩니다.

> service에 proxy를 추가

<center><img src="https://user-images.githubusercontent.com/31475037/95031528-66696400-06f1-11eb-8223-ec5937ba4d70.png"></center>



<br>

## Sidecar pattern

MSA의 등장으로 개발 패러다임이 바뀌는 가운데 여러 design pattern이 나왔습니다. 그 중 sidcar pattern은 **하나의 컨테이너에는 하나의 책임만 가지고 있어야 한다**의 원칙을 가지고 설계 되었습니다.

예를 들어 웹 서버와 로그 수집기가 같이 있는 container를 생각해봅시다. 웹 서버를 수정할 때, 로그 수집기의 종속성등을 고려해 수정을 해야 합니다. 

이런 종속성을 최대한 줄이고자, **서로 다른 역할을 하는 서비스는 각각의 컨테이너로 분리**를 해주어야 합니다. 또한 sidecar pattern은 pod안의 메인 컨테이너를 **확장**해줍니다. 

> Sidecar pattern

<center><img src="https://user-images.githubusercontent.com/31475037/95032005-cd881800-06f3-11eb-966a-49f57db479e8.png"></center>

<br>

## Service Mesh

Proxy 기술은 서비스의 트래픽을 네트워크단에서 통제할 수 있게 하고, Client의 요구에 따라 proxy단에서 라우팅서비스도 가능케 할 수 있습니다. 그러나 TCP기반(L4 layer)의 proxy로는 한계가 있습니다.

MSA시장이 커지면서 서비스들은 네트워크를 통해 서로 통신해야했고, 이러한 서비스에서 사용하는 핵심 네트워크 프로토콜은 HTTP 등의 L7 layer에서 동작하는 프로토콜입니다.

L3, L4기반의 프록시들로는 다양한 요건들을 처리하기 어려워졌고, 그에 따라 **L7기능을 갖춘 프록시**의 필요성이 부각되기 시작했습니다.

 그래서 **Service Mesh**에서의 통신은 사이드카로 배치된 **경량화된 L7 layer proxy**를 사용합니다.

그러나 프록시를 사용해서 트래픽을 통제할 수 있다는 것 까지는 좋은데, **서비스가 거대해짐에 따라 프록시 수도 증가**하게 됩니다. 이런 문제를 해결하기 위해서 각 프록시에 대한 설정정보를 **중앙집중화된 컨트롤러**가 통제할 수 있게 설계되었습니다.

> Service Mesh

![](https://user-images.githubusercontent.com/15958325/70860414-c3c6d580-1f64-11ea-85d9-fdf9b384a058.png)

<br>

# Istio

Data Plane의 메인 프록시로 Envoy proxy를 사용하며 이를 컨트롤 해주는 Control Plane의 오픈소스 솔루션이 **Istio**입니다.

Service mesh의 기본 아키텍처는 서비스 간 직접 호출 대신 앞에서 언급한 경량화된 사이드카 패턴의 프록시를 배치하여 통신합니다. 이런 service mesh 기술의 대표적인 구현체가 이스티오입니다. 무엇보다 쿠버네티스와 클라우드 기반에 적합하여 이미 Red Hat OpenShift, VMware Tanzu 등 PaaS 플랫폼의 Service mesh로 선택되기도 하였습니다. 

<br>

## Envoy proxy

C++로 개발된 고성능 프록시 사이드카.

dynamic service discovery, load balancing, TLS termination, circuit breaker..등등의 기능을 포함

대형 MSA의 단일 Application과 Service를 위해 설계된 고성능 분산 c++프록시입니다.

네트워크는 애플리케이션에 **투명**해야하며, 장애가 발생했을시 **어디에서 문제가 발생했는지 쉽게 파악**할 수 있어야 한다.

**디자인 목표**는 다음과 같습니다.

- 모듈화가 잘되어있으며 테스트하기 쉽게 쓰여짐
- 플랫폼에 구애받지 않는 방식으로 기능을 제공하여 네트워크를 추상화
- L7단이기때문에 L4보다는 성능 감소가 다소 존재하지만 가능한 최고 성능을 목표로 함

### 주요 기능

**Out of process architecture**

Envoy proxy는 그 자체로 메모리 사용량이 적은 고성능의 서버입니다.

모든 프로그래밍 언어, 프레임워크와 함께 실행될 수 있습니다.

이는 다양한 언어,프레임워크를 함께 사용하는 Architecture에 유용히 사용될 수 있습니다.								

**L3/L4 Architecture**

Envoy의 주요 기능은 L7이지만 핵심은 L3/L4 네트워크 프록시 입니다.

TCP프록시, HTTP프록시, TLS인증과 같은 다양한 작업을 지원합니다.

**L7 Architecture**

버퍼링, 속도제한, 라우팅/전달 등과 같은 다양한 작업을 수행할 수 있게 합니다.

**HTTP/2 , gRPC 지원**

HTTP/1.1은 물론 HTTP/2도 지원합니다. 이는 HTTP/1.1과 HTTP/2 클라이언트와 서버간 모든 조합을 연결할 수 있음을 의미합니다. 권장하는 구조는 모든 Envoy간에 HTTP/2를 사용하는 것입니다. 또한 gRPC를 지원하여 HTTP/2기능을 보완할 수 있습니다.

**Advanced Load Balancing**

자동 재시도, circuit break, 외부 속도 제한 서비스를 통한 글로벌 속도제한, 이상치 탐지 등의 기능을 제공합니다.

<br>

## Istio 구조



![img](https://istio.io/latest/docs/ops/deployment/architecture/arch.svg)    



### Pilot

- envoy에 대한 설정관리
- service discovery 기능 제공

**service discovery**

1. 새로운 서비스가 시작되고 pilot의 `platform adapter`에게 그 사실을 알림
2. `platform adapter`는 서비스 인스턴스를 `Abstract model`에 등록
3. **Pilot**은 트래픽 규칙과 구성을 `Envoy Proxy`에 배포

이러한 특징으로 Istio는 k8s, consul 등 여러 환경에서 **트래픽 관리를 위해 동일한 운영자 인터페이스를 유지**할 수 있습니다.

![](https://user-images.githubusercontent.com/15958325/70860715-7ba9b200-1f68-11ea-9fab-bbd3de995c30.png)

### Mixer

- Service Mesh 전체에서 액세스 제어 및 정책 관리
- 각종 모니터링 지표의 수집
- 플랫폼 독립적, Istio가 다양한 호스트환경 & 백엔드와 인터페이스할 수 있는 이유

### Citadel

- 보안 모듈
- 서비스를 사용하기 위한 인증
- TLS(SSL)암호화, 인증서 관리

### Galley

- Istio Configuration의 유효성 검사

<br>

## Istio 기능

구성요소를 살펴보았으니 Istio의 기능에 대해서 살펴보겠습니다.

### Traffic management

쉬운 규칙 구성과 트래픽 라우팅을 통해 서비스간의 트래픽 흐름과 API 호출을 제어 할 수 있습니다.

### Security

기본적으로 envoy를 통해 통신하는 모든 트래픽을 자동으로 TLS암호화를 합니다.

### Policies

서비스간 상호작용에 대해 access, role등의 정책을 설정하여 리소스가 각 서비스에게 공정하게 분배되도록 제어합니다.

### Observability

강력한 모니터링 및 로깅 기능을 제공하여 문제를 빠르고 효율적으로 감지할 수 있게 합니다.

Istio는 믹서를 통해 서비스 간 호출 관계, 서비스 응답시간, 처리량 등의 네트워크 트래픽 지표를 수집하고 모니터링합니다. 모든 서비스는 호출될 때 앞단의 Envoy를 거치기 때문에 각 서비스의 호출 시 또는 주기적으로 믹서에 정보를 전달하고 이를 로깅할 수 있습니다. 믹서는 손쉽게 플러그인(Plugin)이 가능한 어댑터(Adapter) 구조로 여러 모니터링 시스템에 연결됩니다. 예를 들면 쿠버네티스에서 많이 사용하는 Heapster, Prometheus, StackDriver, Datadog 등의 모니터링  도구들과 연계하여 시각화할 수 있으며 Grafana를 통해 각 서비스의 응답시간, 처리량 등을 확인할 수 있습니다. 또한  Jaeger를 이용하면 분산 트랜잭션 구간별로 응답 시간을 모니터링할 수도 있습니다.

<br>

# Knative

오픈소스 서버리스 솔루션인 Knative에 대해서 알아보겠습니다.

## Serverless? (FaaS)

IaaS Paas Faas Baas 리뷰

Knative가 무엇인지 알아보기 전에, **Serverless**라는 단어를 짚고 넘어갈 필요가 있습니다.

Serverless Architecture란 단어그대로 서버가 필요없는 구조를 뜻합니다.
하지만 실제로 서버가 존재하지 않는다는 것은 아니고, 개발자나 관리자가 **서버인프라에 대해 신경쓰지 않아도 된다는** 뜻입니다.

이러한 구조 하에서는 **요청이 있을때만 필요한 코드를 실행**하고, 요청이 많을 경우에는 그에 비례하는 자원을 할당하여 동시에 처리하는 방법으로 **사용률과 확장성을 극대화**할 수 있습니다.

### 장점

- 함수가 호출된 만큼만 비용이 청구, 비용절약
- 인프라 관리 필요없음

### 단점

- 각 함수에서 사용할 수 있는 

  자원이 제한됨

  - 계속 켜둬야 하는 웹소켓 같은 경우 사용불가

- 벤더에 강한 의존성을 가짐    

  - AWS Lambda
  - Azure Functions
  - Google Cloud Functions
  
- 로컬 storage 사용 불가능    

  - stateless
  - 클라우드 스토리지 사용해야함

<br>

## Knative의 특징

위 서버리스 솔루션들은 특정 클라우드 플랫폼에 **의존성**을 가지고 있습니다. 이를 해결하기 위해 나온 **오픈소스 서버리스 솔루션**이 Knative입니다.

- **Kubernetes** 위에서 기동
- 특정 클라우드 종속성이 없음
- On-Prem에서 사용 가능

<br>

## Knative 기능

![image](https://user-images.githubusercontent.com/15958325/71711867-42b57c80-2e46-11ea-8bb6-4aac375151fd.png)
stateless app뿐만아니라 이벤트를 받아 처리하는 이벤트 핸들링을 위한 모델제공, 컨테이너를 빌드할 수 있는 기능을 제공합니다.

크게 두가지 Component로 나뉘며 다음과 같습니다.

### Serving

서비스를 배포하며 서비스 네트워크 설정을 알아서 해줌 

Knative Serving은 serverless application이나 function들을 배포하고 서빙하는 역할을 합니다.

- Serverless 컨테이너의 빠른 배포
- 자동 scale up & down
- Istio기반 라우팅 & 네트워크 프로그래밍
- 배포된 코드와 config의 스냅샷기능

Serving 은 쿠버네티스 CRD (Custom Resource Definition)으로 정의된 4개의 오브젝트로 구성되어 있습니다.
 ![image](https://user-images.githubusercontent.com/15958325/71712412-742f4780-2e48-11ea-9882-4173d7ea9c59.png)

- **Service** : Configuration과 Route 리소스의 추상화된 집합체입니다. 워크로드의 lifecycle을 관리하는 역할을 합니다.
- **Configuration** : Serving으로 배포되는 서비스를 정의합니다. 컨테이너 경로, 환경변수 등의 설정을 정의할 수 있으며 컨테이너의 경로를 지정할 때 빌드버전도 지정할 수 있습니다.
- **Revision** : 코드와 config의 스냅샷입니다. Configuration을 생성할때마다 새로운 revision이 생기며 이전 revision으로 롤백하거나 각각의 버전으로 트래픽을 분할하여 서빙할 수 있습니다.
- **Route** : User Service의 네트워크 엔드포인트를 제공합니다. 하나 이상의 Revision을 가지며 서비스로 들어오는 트래픽을 Revision으로 라우팅할 수 있습니다. 단순히 최신 버전의 Revision으로 라우팅할수도 있지만 카날리 테스트와 같이 여러 Revision으로 라우팅할수도 있습니다.

### Eventing

Knative의 비동기 메세지 처리 모듈

또다른 Component인 Knative Eventing은 비동기 메시지 처리를 위한 모듈입니다.

메시지 큐와 같은 이벤트 발생 자원들은 Knative에 event source에 등록되고 해당 이벤트가 발생되면 지정된 knative 서비스로 이벤트 메시지를 전송하는 루틴으로 동작하게 됩니다.

 ![image](https://user-images.githubusercontent.com/15958325/71712448-9aed7e00-2e48-11ea-8809-db68f914d3a8.png)

- broker : 이벤트 소스로부터 메시지를 받아 저장하는 버킷역할
- trigger : 특정 메시지 패턴만 서비스로 보낼 수 있게 하는 조건역할
- subscriber : 이벤트를 수신하여 처리하는 객체

<br>

**참고 자료**

[2018 Cloud Hackathon Tech Session - Kubernetes](https://bcho.tistory.com/1281?category=731548)

[lstio](https://istio.io/latest/docs/ops/deployment/architecture/)

[호롤리 블로그](https://gruuuuu.github.io/cloud/service-mesh-istio/#)