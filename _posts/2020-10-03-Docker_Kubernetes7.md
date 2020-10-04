---
layout: post
title: 딥러닝을 위한 kubeflow 1 - 소개
tags: [Backend, Full Stack Deep Learning]
use_math: true
---

# Kubeflow란?

Kubeflow는 쿠버네티스에서 TF(TensorFlow)를 확장한 TFX(TensorFlow eXtend) 프로젝트로 시작했습니다. 왜냐하면 실제 ML 프로덕션 환경에선 모델연구 외 다른 일들이 훨씬 많기 때문입니다.

Kubeflow란 Kubernetes 환경에서 간단하게 머신 러닝 워크플로우를 배포할 수 있게 도와주는 시스템이다. 다양한 환경에서 실행하는 머신 러닝 워크플로우를 단일 환경에서 손쉽고 직관적으로 배포하게 하는 것이 Kubeflow의 목적이다.

실제로 ML모델을 만들어서 서비스에 적용시키는 일은, 모델을 만드는 시간보다 데이터 수집과 분석 그리고 모델을 튜닝하는 등의 반복적인 작업이 더 많이 소모된다. 

그래서 이러한 일련의 과정을 묶어 파이프라인으로 구축하게 된다. 하지만, 서비스가 많아지고 파이프라인이 많아지면서 시스템이 복잡해지고 유지보수가 힘들어지자 'ML 플랫폼'이 필요하게 되었다. 

> 프로덕션 환경에서의 ML workflow

![](https://user-images.githubusercontent.com/31475037/94651765-f1cea800-0333-11eb-8d34-761ee240d7d8.png)

위의 파이프라인 흐름을 시스템 아키텍쳐로 표현하면 다음과 같습니다.

> ML workflow pipeline

![](https://user-images.githubusercontent.com/31475037/90581688-d2a60c00-e206-11ea-9208-3badc5bbf034.png)

이런 상황에서 google에서 만든 Kuberflow는 쿠버네티스 플랫폼 위에서 작동하는 ML production을 위한 통합 플랫폼입니다.

위와 같은 아키텍쳐를 구성하기 위해서, kubeflow는 새로운 툴들을 만든것이 아닌, 쿠버네티스 오픈소스 생태계에 있는 여러 툴들을 통합하여 제공합니다. 이를 통해 쿠버네티스에 이해도가 엄청 높지 않아도 사용하기가 용이합니다. 

- 클라우드나 On-Prem, 개인 개발 환경에 상관 없이 동일한 머신러닝 플랫폼을 손쉽게 만들 수 있기 때문에 특정 벤더나 플랫폼에 종속되지 않음
- 컨테이너로 패키징이 되있어 배포가 간단함

### Kubeflow를 사용하려는 이유

\- 이미 쿠버네티스 기반 인프가가 있거나, 새로운 머신러닝 플랫폼을 만드려는 경우

\- 다양한 환경에서 머신러닝 모델을 학습하거나 서비스 하려는 경우

\- 자원(CPU 또는 GPU)를 할당하여 작업을 하려는 경우

\- jupyter 노트북을 사용하여 머신러닝 작업을 하려는 경우

Kubeflow의 목표는 다음과 같은 업무를 수행 할 수 있도록하여, ML(머신 러닝) 모델을 확장하고 프로덕션에 최대한 간단하게 배포하는 것입니다.

- 다양한 인프라에 쉽고 반복 가능하며 이식 가능한 배포 (예 : local pc에서 실험한 다음 클라우드에 배포)
- 느슨하게 결합 된 마이크로 서비스 배포 및 관리
- 수요에 따라 확장
- 다양한 인프라 환경에 쉽고 재사용 가능하고 포터블한 배포 가능
- loosely-coupled 마이크로서비스 배포 및 관리
- 요구에 따라 가능한 스케일링

궁극적으로 Kubernetes가 이미 실행중인 모든 곳에서 손쉽게 ML 스택을 사용할 수 있게 하는것입니다.

## 특징

전체 ML 개발 과정 통합 관리, 온프레미스/클라우드 제한 없이 배포 지원 

Kubernetes + ML 플로우

쿠버네티스는 컨테이너화된 워크로드와 서비스를 관리하기 위한 오픈소스 플랫폼

ML플로우는 ML의 라이프사이클을 관리하는 오픈소스 ML 플랫폼 

즉, 쿠브플로우는 쿠버네티스 환경에서 기계학습 워크플로우를 관리하고 제작한 학습모델을 환경제한 없이 배포 및 확장할 수 있도록 돕기 위해 개발된 툴킷

사용자는 주피터노트북을 사용해 학습모델 개발 후, 실제 업무에 적용하기 위해 그대로 배포하여 사용 

쿠버네티스 위에서 돌기에 쿠버네티스 생태계에서 쓰이는 오픈소스를 레고 조립 하듯 조립해 쓸 수 있음(Argo, Prometheus...). 즉 쿠버네티스가 de facto가 된 지금, 강력한 확장성을 지님

<br>

**참조 강의**

[Kubeflow 101](https://www.youtube.com/playlist?list=PLIivdWyY5sqLS4lN75RPDEyBgTro_YX7x)

[Kubeflow doc](https://www.kubeflow.org/docs/)

[조대협 블로그](https://bcho.tistory.com/1301)

[Full Stack Deep Learning](https://course.fullstackdeeplearning.com/)