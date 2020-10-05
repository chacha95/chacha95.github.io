---
layout: post
title: 딥러닝을 위한 kubernetes 5 - Service Mesh와 Knative
tags: [Backend, Full Stack Deep Learning]
use_math: true
---

# Service Mesh

Service Mesh를 이해하기 위해선 MSA의 장단점에 대해 알아야 합니다.

MSA(Micro Service Architecture)는 여러 장점을 가졌지만, 많은 단점도 가졌습니다. MSA는 기능을 서비스 단위로 잘게 나누다 보니, 시스템이 커지면 커질수록 서비스간 관리가 힘들어집니다. 특히, 서비스간 전체적인 연결 구조를 파악하기 어려워, 이로 인해 장애가 났을때, 어느 서비스에서 장애가 났는지 추적이 어려워집니다.

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

Proxy 기술은 서비스의 트래픽을 네트워크 단(L3, L4 layer)에서 통제할 수 있게 하고, Client의 요구에 따라 proxy단에서 라우팅서비스도 가능케 할 수 있습니다. 그러나 TCP기반(L3, L4 layer)의 proxy로는 한계가 있었습니다. 특히나 MSA가 커지면서 서비스들은 네트워크를 통해 더 많은 통신이 필요했습니다.

이렇게 커져가는 MSA에서 L3, L4기반의 프록시들만으론 모든 요청을 처리하기 어려워졌고, **L7 proxy**가 나왔습니다.

> 커져가는 MSA 

![](https://www.abhishek-tiwari.com/assets/images/Network-of-Microservices.jpg)

<br>

## Envoy proxy

여러 L7 layer proxy 중에서 Istio에서 쓰이는 envoy proxy를 소개 드립니다. Envoy proxy는 MSA의 Application과 Service를 위해 설계된 고성능 분산 c++ 프록시입니다. 

### 주요 기능

**Out of process architecture**

Envoy proxy는 c++로 구현된 메모리 사용량이 적은 고성능의 서버입니다.

특정 플랫폼에 종속되지 않습니다. 이로인해 모든 프로그래밍 언어, 프레임워크와 함께 실행될 수 있으며 네트워크 인프라를 추상화 시킵니다.

**L3/L4 Architecture**

Envoy의 주요 기능은 L7이지만  근본은 L3/L4 네트워크 프록시 입니다.

TCP프록시, HTTP프록시, TLS인증과 같은 다양한 작업을 지원합니다.

기존의 L3/L4 proxy를 L7 까지 확장시킨 proxy입니다.

**L7 Architecture**

버퍼링, 속도제한, 라우팅/전달 등과 같은 다양한 작업을 수행할 수 있게 합니다.

<br>

## Istio(이스티오)

Service mesh의 기본 아키텍처는 서비스끼리 직접통신하는 것이 아닌, 사이드카 패턴의 프록시를 배치하여 통신합니다. 이런 service mesh 기술의 대표가 **Istio**입니다. 

그러나 프록시를 사용해 트래픽을 통제하는 것은 장점이나, 서비스 수가 증가함에 따라 **프록시 수도 증가**하게 됩니다. 이런 문제를 해결하기 위해 각 프록시에 대한 설정정보를 **중앙 집중화된 컨트롤러**가 통제할 수 있게 설계되었습니다.

### Istio 구조

Data plane의 main proxy로 envoy, control plane으로 Istio를 사용합니다.

**Pilot**

- envoy에 대한 설정관리
- service discovery 기능 제공

**Mixer**

- Service Mesh 전체에서 액세스 제어 및 정책 관리
- 각종 모니터링 지표의 수집
- 플랫폼 독립적, Istio가 다양한 호스트환경 & 백엔드와 인터페이스할 수 있는 이유

**Citadel**

- 보안 모듈
- 서비스를 사용하기 위한 인증
- TLS(SSL)암호화, 인증서 관리

**Galley**

- Istio configuration의 유효성 검사

> Istio 구조

![img](https://istio.io/latest/docs/ops/deployment/architecture/arch.svg)    

### Istio 작동 방식

**그림으로 정리해서 올리기**

pilot을 통해서 읽어옴

ssl을 이용해 보안 암호화 -> Istio Auth를 사용

프록시가 service B를 호출한(pilot이 호출함)

그랬을 때, service B의 호출을 mixer가 호출을 받아 리스폰스를 받음

mixer에는 log가 남음

control plane을 이용해 서비스를 컨트롤 가능함

traffic steering을 이용해 트래픽 컨트롤 가능

service discovery 기능 좋음

Istio는 믹서를 통해 서비스 간 호출 관계, 서비스 응답시간, 처리량 등의 네트워크 트래픽 지표를 수집하고 모니터링합니다. 

모든 서비스는 호출될 때 앞단의 Envoy를 거치기 때문에 각 서비스의 호출 시 또는 주기적으로 믹서에 정보를 전달하고 이를 로깅할 수 있습니다. 

mixer는 손쉽게 플러그인(Plugin)이 가능한 어댑터(Adapter) 구조로 여러 모니터링 시스템에 연결됩니다.

<br>

# Knative

오픈소스 서버리스 솔루션인 Knative에 대해서 알아보겠습니다.

<br>

## Serverless?

Knative가 무엇인지 알아보기 전에, **Serverless**라는 단어를 짚고 넘어갈 필요가 있습니다.

Serverless Architecture란 단어그대로 서버가 필요없는 구조를 뜻합니다. 하지만 실제 서버가 존재하지 않는다는 것은 아니고, 개발자나 관리자가 **서버인프라에 대해 신경쓰지 않아도 된다는** 뜻입니다. 

이와 관련된 배경지식을 알면 이해가 편합니다. 클라우드의 등장으로 여러 종류의 클라우드 서비스가 등장했습니다.

### On-Premise

시스템에서 필요한 모든 인프라를 직접 관리하는 것을 의미합니다. 공간, 하드웨어, 네트워크, 운영체제, 모두 직접 관리를 해줍니다. 이 방식에서의 가장 큰 문제점은 시스템이 엄청 커지면 전산실을 유지 할 관리자가 필요 할 것이고, 이 인력에 대한 비용이 나간다는 점입니다.

### IaaS (Infrastructure as a Service)

AWS, Azure, GCP 등의 서비스가 등장했습니다. 더 이상 서버자원, 네트워크, 전력 등의 인프라를 모두 직접 구축 할 필요 없어졌습니다.

### PaaS (Platform as a Service)

IaaS에서 한단계 더 진보했으며, 사용자는 애플리케이션만 배포하면 바로 구동시킬 수 있습니다. 대표적으로 AWS Elastic Beanstalk, Azure App Services 등이 있습니다. 이를 사용하면 Auto Scaling 및 Load Balancing 도 손쉽게 적용 할 수 있습니다.

### Serverless

#### BaaS(Backend as a Service)

서버 개발시 백엔드 단까지 제공하는 서비스입니다. 대표적으로 Firebase가 있습니다. 

Firebase를 기준으로 말하자면, 앱 개발에 있어 필요한 다양한 기능들(데이터베이스, 소셜서비스 연동)을 API로 제공해 줌으로서, 개발자들이 백엔드 개발을 하지 않아도 필요한 기능을 쉽고 빠르게 구현케 해줍니다. 그러나 클라이언트 위주의 코드, 이용자 수에 따라 기하급수적으로 늘어나는 가격, 복잡한 쿼리가 불가능한 성능 문제 등이 있어 소규모 프로젝트 이외에는 잘 쓰이지 않습니다.

#### FaaS(Function as a Service)

FaaS는 애플리케이션을 기능별로 여러개의 함수로 쪼개, 매우 거대하고 분산된 컴퓨팅 자원에 등록해, 이 함수들이 실행되는 **횟수 x 시간**만큼 비용을 내는 방식을 말합니다. 

장점으론 이벤트가 발생했을 때만 컴퓨팅 자원을 대여하기에, PaaS 방식보다 비용 절약이 됩니다. 또한 함수를 호출하는 형태이기에 확장성 또한 좋습니다. 그냥 함수를 여러번 호출하는 것과 동일하니까요.

단점으론 사용 할 수 있는 자원에 제한이 있습니다. 하나의 함수가 한번 호출 될 때, AWS 에서는 최대 1500MB 의 메모리까지 사용 가능하며, 처리시간은 최대 300 초 까지 사용 가능합니다. 때문에, 웹소켓 같이 계속 켜놔야 하는것은 사용하지 못합니다. 안타깝게도 GPU 자원은 사용 불가능하며, 현재는 CPU와 Memory 자원만 지원합니다.

이러한 구조 하에서는 **요청이 있을때만 필요한 코드를 실행**하고, 요청이 많을 경우에는 그에 비례하는 자원을 할당하여 동시에 처리하는 방법으로 **사용률과 확장성을 극대화**할 수 있습니다.

<br>

## Knative의 특징

continer만 던지면 알아서 deployment 하게 해줌

이걸 이용해 배포하게 되면 굉장히 쉬워짐

dockerfile을 만들어 던져주고, service.yaml을 정의해 던져주면

위에 모든걸 다 해줌(kubernetes도 귀찮고 istio도 귀찮음 -> 다 알아서 해줌)

위 FaaS들은 특정 클라우드 플랫폼에 **의존성**을 가지고 있습니다. 이를 해결하기 위해 나온 **오픈소스 서버리스 솔루션**이 Knative입니다.

- **Kubernetes** 위에서 기동
- 특정 클라우드 종속성이 없음
- On-Premise에서 사용 가능

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

- **broker** : 이벤트 소스로부터 메시지를 받아 저장하는 버킷역할
- **trigger** : 특정 메시지 패턴만 서비스로 보낼 수 있게 하는 조건역할
- **subscriber** : 이벤트를 수신하여 처리하는 객체

<br>

**참고 자료**

[2018 Cloud Hackathon Tech Session - Kubernetes](https://www.youtube.com/watch?v=rdyUAduXi48&feature=emb_title)

[lstio](https://istio.io/latest/docs/ops/deployment/architecture/)

[호롤리 블로그](https://gruuuuu.github.io/cloud/service-mesh-istio/#)