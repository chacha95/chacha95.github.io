---
layout: post
title: MLOps Zero to Hero 7 - Service Mesh와 Knative
tags: [MLOps]
use_math: true
---

# Service Mesh

Service Mesh를 이해하기 위해선 MSA(Micro Service Architecture)의 장단점에 대해 알아야 합니다.

MSA는 장점도 많지만, 단점도 많습니다. 

MSA는 기능을 서비스 단위로 잘게 나누다 보니, 시스템이 커지면 커질수록 서비스간 관리가 힘들어집니다. 특히, 서비스간 전체적 연결 구조 파악이 어려워, 이로 인한 장애가 났을때 어느 서비스에서 장애가 났는지 추적이 어려워집니다.

이런 MSA의 단점을 개선하기 위해 service mesh 기술이 나왔습니다. Service mesh는 소프트웨어 계층이 아니라 인프라 측면에서 MSA의 문제를 해결하기 위해 나왔습니다.

Service mesh를 이해하기 위해선 다음 개념들을 알아야 합니다.



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

Service mesh는 소프트웨어 계층이 아니라 인프라 측면에서 MSA의 문제를 해결하기 위해 나왔습니다.

기존 대다수 Proxy 기술들은 서비스 트래픽을 네트워크 단(L3, L4 layer)에서 통제했습니다. 그러나 네트워크 기반(L3, L4 layer)의 proxy로는 MSA 구조에선 기능이 부족했습니다. 특히나 MSA가 커졌을 때, 서비스들은 네트워크를 통해 더 많은 통신이 필요 했고, 이를 load balancing 해줄 기능이 필요했습니다.

이렇게 커져가는 MSA 패러다임 속에서 **L7 layer proxy** 기능이 추가된 envoy proxy가 나왔습니다.

> 커져가는 MSA 

![](https://www.abhishek-tiwari.com/assets/images/Network-of-Microservices.jpg)

<br>

## Envoy proxy

Envoy proxy는 MSA를 위해 설계된 고성능 분산 c++ 프록시입니다. 기존의 **L3/L4 layer proxy**를 **L7 layer proxy** 까지 확장시켰습니다. 

다음 기능들을 지원합니다.

### 고성능 및 독립적

Envoy proxy는 c++로 구현된 메모리 사용량이 적은 고성능 서버입니다.

특정 플랫폼에 종속되지 않습니다. 이로인해 모든 프로그래밍 언어, 프레임워크와 함께 실행될 수 있으며 네트워크 인프라를 추상화 시킵니다.

### L7 layer proxy

L3/L4/L7 layer proxy를 사용합니다. L7 layer에서 HTTP,  gRPC, WebSocket 및 TCP 트래픽에 대한 라우팅 규칙을 사용하여 보다 세분화된 트래픽 관리를 합니다.

### 보안 및 인증

보안 정책을 시행하고 구성 API를 통해 정의된 액세스 제어 및 속도를 제한합니다.

그 외에도 **Dynamic service discovery**, **L7 layer Load balancing**, **HTTP/2, gRPC**를 지원합니다.

<br>

# Istio(이스티오)

Service mesh의 기본 아키텍처는 서비스끼리 직접통신하는 것이 아닌, 사이드카 패턴의 프록시를 배치하여 통신합니다. 이런 service mesh 기술의 대표가 **Istio**입니다. 

그러나 프록시를 사용해 트래픽을 통제하는 것은 장점이자 단점입니다. 서비스 수가 증가함에 따라 **프록시 수도 증가**하기 때문입니다. 이 문제를 해결하기 위해 각 프록시에 대한 설정정보를 **중앙 집중화된 컨트롤러**가 통제할 수 있게 설계되었습니다.

<br>

## Istio Architecture

Istio의 구조는 크게 두가지 components로 이뤄져 있습니다. **Data plane**으로 envoy proxy, **control plane**으로 Istiod를 사용합니다.

### Istiod

Istiod는 service discovery, configuration(설정), certificatie(인증)를 관리 합니다. 

Istiod는 다음 components로 구성됩니다.

**Pilot**

- envoy 설정 및 관리
- service discovery 기능 제공
- Pilot은 service discovery mechanism을 추상화

**Mixer**

- Service Mesh 액세스 제어 및 정책 관리
- 각종 모니터링 지표 수집

**Citadel**

- 보안 모듈
- 서비스를 사용하기 위한 인증
- TLS 암호화, 인증서 관리

**Galley**

- Istio configuration의 유효성 검사

> Istio 구조

![img](https://istio.io/latest/docs/ops/deployment/architecture/arch.svg)    

<br>

# Knative

Knative는 Kubernetes를 확장하여 컨테이너화된 애플리케이션을 빌드하는데 필수적인 미들웨어를 제공하는 오픈소스 입니다. 먼저, Knative가 무엇인지 알아보기 전에, **serverless**라는 단어를 짚고 넘어갈 필요가 있습니다.

<br>

## Serverless?

Serverless란 단어 의미대로 실제 서버가 존재하지 않는다는 것은 아니고, 개발자가 **서버인프라에 대해 신경쓰지 않아도 된다는** 뜻입니다. 

이와 관련되어 클라우드의 등장 배경과 발전과정에 대해 간략히 봅시다.

### On-Premise

시스템에 필요한 모든 인프라부터 software 개발까지 직접 관리하는 것을 의미합니다. 공간, 하드웨어, 네트워크, 운영체제, 모두 직접 관리합니다.

### IaaS (Infrastructure as a Service)

AWS, Azure, GCP 등의 서비스가 등장했습니다. 더 이상 서버자원, 네트워크 같은 인프라를 직접 구축 할 필요 없어졌습니다.

### PaaS (Platform as a Service)

IaaS에서 한단계 더 진보했으며, 사용자는 애플리케이션만 배포하면 바로 구동시킬 수 있습니다. 대표적으로 AWS Elastic Beanstalk, Azure App Services 등이 있습니다. 이를 사용하면 Auto Scaling 및 Load Balancing 도 손쉽게 적용 할 수 있습니다.

### Serverless

#### BaaS(Backend as a Service)

서버 개발시 백엔드 서버까지 제공하는 서비스입니다. 대표적으로 Firebase가 있습니다. 

Firebase를 기준으로 말하자면, 앱 개발에 있어 필요한 다양한 기능들(데이터베이스, 소셜서비스 연동)을 API로 제공해 줌으로서, 개발자들이 백엔드 개발을 하지 않아도 필요한 기능을 쉽고 빠르게 구현케 해줍니다. 그러나 클라이언트 위주의 코드, 이용자 수에 따라 기하급수적으로 늘어나는 가격, 복잡한 쿼리가 불가한 문제 등이 있어 소규모 프로젝트 이외에는 잘 쓰이지 않습니다.

#### FaaS(Function as a Service)

FaaS는 애플리케이션을 기능별로 여러 함수로 쪼갠뒤 매우 거대하고 분산된 컴퓨팅 자원에 등록해, 이 함수들이 실행되는 **횟수 x 시간**만큼 비용을 내는 방식을 말합니다. 

이벤트가 발생했을 때만 컴퓨팅 자원을 이용하기에, PaaS 방식보다 비용 절약이 됩니다. 또한 함수를 호출하는 형태이기에 확장성 또한 좋습니다. 

그러나 사용 할 수 있는 한번에 사용 가능한 자원에 제한이 있습니다. 하나의 함수가 한번 호출 될 때, AWS의 경우 최대 1500MB의 메모리까지 사용 가능하며, 처리시간은 최대 300초 까지만 사용 가능합니다. 안타깝게도 GPU 자원은 사용 불가능하며, 현재는 CPU와 Memory 자원만 사용 가능합니다.

<br>

## Knative란?

현재 쿠버네티스가 de fecto(업계 표준)가 되는 와중에 쿠버네티스 자체는 안정되어가고 있지만, 이를 현업에 적용하기에는 까다로운 부분들이 많이 존재합니다. 예를들어 stateless(무상태) 서비스를 하나 구축한다 하더라도 Deployment, Ingress, Service 등의 쿠버네티스 리소스를 정의해서 배포해야 합니다. 그런데 이런 설정을 일일이 다 하기에는 일반 개발자들에게 부담이 됩니다.

그래서 서버에 복잡한 설정 없이 stateless 서비스나 간단한 event 서비스 등을 구축하는 방법으로 위에서 설명한 serverless 서비스가 나타났습니다(FaaS). AWS의 람다(Lambda)나, GCP의 Function이 대표적입니다. 그런데 이런 serverless 서비스들은 특정 클라우드 플랫폼에 의존성을 가지고 있는데, 이런 문제를 해결하기 위해 나온 오픈소스 serverless 솔루션이 Knative 입니다.   

<br>

## Knative components

Knative의 components는 두 가지로 구성됩니다.

### Eventing

Eventingh은 비동기 메세지 처리를 위한 component 입니다. Kafka와 같은 message queue와 비슷한 구조를 지니며, Cron과 같은 타이머에서 이벤트가 발생하면 이를 받아서 처리합니다. 

- **Broker**: 메시지를 받아 저장하는 버킷역할, broker는 속성별 event bucket을 제공

- **Trigger**: 특정 메시지 패턴만 service로 보낼 수 있게 하는 조건역할
- **subscriber**: 이벤트를 수신하여 처리하는 객체
- **filter**: evnet 속성에 대한 필터링해주는 기능

> Event 구성 요소

 ![image](https://knative.dev/docs/eventing/images/broker-trigger-overview.svg)

### Serving

Knative Serving은 serverless application이나 function들을 배포하고 서빙하는 역할을 합니다. 서비스를 배포하며 서비스 네트워크 설정을 알아서 해줍니다. 

- Serverless 컨테이너의 빠른 배포
- 자동 scale up & down
- Istio기반 라우팅 & 네트워크 프로그래밍
- 배포된 코드와 config의 스냅샷기능

Serving 은 쿠버네티스 CRD (Custom Resource Definition)으로 정의된 4개의 오브젝트로 구성되 있습니다.

- **Service** : Configuration과 Route 리소스의 추상화된 집합체입니다. 워크로드의 lifecycle을 관리하는 역할을 합니다.
- **Configuration** : Serving으로 배포되는 서비스를 정의합니다. 컨테이너 경로, 환경변수 등의 설정을 정의할 수 있으며 컨테이너의 경로를 지정할 때 빌드 버전을 지정할 수 있습니다.
- **Revision** : code와 config의 스냅샷입니다. Configuration을 생성할때마다 새로운 revision이 생기며 이전 revision으로 롤백하거나 각각의 버전으로 트래픽을 분할하여 서빙할 수 있습니다.
- **Route** : User Service의 네트워크 엔드포인트를 제공합니다. 하나 이상의 Revision을 가지며 서비스로 들어오는 트래픽을 Revision으로 라우팅할 수 있습니다. 

> Serving

 <center><img src="https://github.com/knative/serving/raw/master/docs/spec/images/object_model.png"></center>

<br>

**참고 자료**

[2018 Cloud Hackathon Tech Session - Kubernetes](https://www.youtube.com/watch?v=rdyUAduXi48&feature=emb_title)

[Istio](https://istio.io/latest/)

[Knative](https://knative.dev/docs/)

[조대협 블로그](https://bcho.tistory.com/1323?category=731548)