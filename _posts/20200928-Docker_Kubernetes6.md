---
layout: post
title: 딥러닝을 위한 kubeflow1 - 소개
tags: [Backend]
use_math: true
---

# Kubeflow란?

Kubeflow는 무엇입니까?
Kubeflow는 Kubernetes 용 기계 학습 도구 키트입니다. Kubeflow 사용 사례에 대해 알아 봅니다.

Kubeflow는 쿠버네티스에서 TF(TensorFlow)를 확장한 TFX(TensorFlow eXtend) 프로젝트로 시작했습니다. 왜냐하면 실제 ML 프로덕션 환경에선 모델연구 외 다른 일들이 훨씬 많기 때문입니다.

Kubeflow를 사용하기위한 기본 워크 플로는 다음과 같습니다.

Kubeflow 배포 바이너리를 다운로드하고 실행합니다.
결과 구성 파일을 사용자 정의하십시오.
지정된 스크립트를 실행하여 특정 환경에 컨테이너를 배포합니다.
구성을 조정하여 ML 워크 플로의 각 단계 (데이터 준비, 모델 학습, 예측 제공, 서비스 관리)에 사용할 플랫폼과 서비스를 선택할 수 있습니다.

Kubernetes 워크로드를 로컬, 온 프레미스 또는 클라우드 환경에 배포하도록 선택할 수 있습니다.

우리의 목표는 Kubernetes가 다음과 같은 장점을 수행 할 수 있도록하여 ML (머신 러닝) 모델을 확장하고 프로덕션에 최대한 간단하게 배포하는 것입니다.

다양한 인프라에 쉽고 반복 가능하며 이식 가능한 배포 (예 : 랩톱에서 실험 한 다음 온 프레미스 클러스터 또는 클라우드로 이동)
느슨하게 결합 된 마이크로 서비스 배포 및 관리
수요에 따라 확장
ML 실무자는 다양한 도구 세트를 사용하기 때문에 핵심 목표 중 하나는 사용자 요구 사항 (이유 내에서)에 따라 스택을 사용자 지정하고 시스템이 "지루한 작업"을 처리하도록하는 것입니다. 우리는 좁은 기술 세트로 시작했지만 추가 도구를 포함하기 위해 다양한 프로젝트를 진행하고 있습니다.

궁극적으로 Kubernetes가 이미 실행중인 모든 곳에서 손쉽게 ML 스택을 사용할 수 있고 배포 대상 클러스터를 기반으로 자체 구성 할 수있는 간단한 매니페스트 세트가 필요합니다.

Kubeflow란 Kubernetes 환경에서 간단하게 머신 러닝 워크플로우를 배포할 수 있게 도와주는 시스템이다. 다양한 환경에서 실행하는 머신 러닝 워크플로우를 단일 환경에서 손쉽고 직관적으로 배포하게 하는 것이 Kubeflow의 목적이다.

- 다양한 인프라 환경에 쉽고 재사용 가능하고 포터블한 배포 가능
- loosely-coupled 마이크로서비스 배포 및 관리
- 요구에 따라 가능한 스케일링

\- 복잡한 기계학습(ML) 모델 개발 및 배포를 효과적으로 지원하기 위한 오픈소스 머신러닝 툴킷 

\- 2020.03 쿠브플로우 1.0 버전 공개 

\- 전체 ML 개발 과정 통합 관리, 온프레미스/클라우드 제한 없이 배포 지원 

\- Kubernetes + ML플로우

\- 쿠버네티스는 컨테이너화된 워크로드와 서비스를 관리하기 위한 오픈소스 플랫폼

\- ML플로우는 ML의 라이프사이클을 관리하는 오픈소스 ML 플랫폼 

\- 즉, 쿠브플로우는 쿠버네티스 환경에서 기계학습 워크플로우를 관리하고 제작한 학습모델을 환경제한 없이 배포 및 확장할 수 있도록 돕기 위해 개발된 툴킷

\- 사용자는 주피터노트북을 사용해 학습모델 개발 후, 실제 업무에 적용하기 위해 그대로 배포하여 사용 

실제로 ML모델을 만들어서 서비스에 적용시키는 일은, 모델을 만드는 시간보다 데이터 수집과 분석 그리고 모델을 튜닝하는 등의 반복적인 작업이 더 많이 소모된다. 

그래서 이러한 일련의 과정을 묶어 파이프라인으로 구축하게 된다. 하지만, 서비스가 많아지고 파이프라인이 많아지면서 시스템이 복잡해지고 유지보수가 힘들어지자 'ML 플랫폼'이 필요하게 되었다. 



> 프로덕션 환경에서의 ML workflow

![](https://user-images.githubusercontent.com/31475037/90581600-9a063280-e206-11ea-918e-611979e51c00.png)

위의 파이프라인 흐름을 시스템 아키텍쳐로 표현하면 다음과 같습니다.

> ML workflow pipeline

![](https://user-images.githubusercontent.com/31475037/90581688-d2a60c00-e206-11ea-9208-3badc5bbf034.png)

이런 상황에서 google에서 만든 Kuberflow는 쿠버네티스 플랫폼 위에서 작동하는 ML production을 위한 통합 플랫폼입니다.

위와 같은 아키텍쳐를 구성하기 위해서, kubeflow는 새로운 툴들을 만든것이 아닌, 쿠버네티스 오픈소스 생태계에 있는 여러 툴들을 통합하여 제공합니다. 이를 통해 쿠버네티스에 이해도가 엄청 높지 않아도 사용하기가 용이합니다. 

- 클라우드나 On-Prem, 개인 개발 환경에 상관 없이 동일한 머신러닝 플랫폼을 손쉽게 만들 수 있기 때문에 특정 벤더나 플랫폼에 종속되지 않음
- 컨테이너로 패키징이 되있어 배포가 간단함





> Kubeflow를 사용하려는 이유

\- 이미 쿠버네티스 기반 인프가가 있거나, 새로운 머신러닝 플랫폼을 만드려는 경우

\- 다양한 환경에서 머신러닝 모델을 학습하거나 서비스 하려는 경우

\- 자원(CPU 또는 GPU)를 할당하여 작업을 하려는 경우

\- jupyter 노트북을 사용하여 머신러닝 작업을 하려는 경우

<br>

## Kubeflow 컴포넌트 구성

### IDE 환경

IDE 개발환경으로는 Jupyter 계열들을 지원한다. 또, 아래 그림과 같이 노트북 인스턴스의 하드웨어 스펙 (CPU, Memory, GPU)를 정의할 수 있습니다.

> 리소스 spec 정의

![](https://user-images.githubusercontent.com/31475037/90581594-983c6f00-e206-11ea-8ebe-20acd9fe1cd5.png)



### GPU 드라이버

쿠버네티스상에서 GPU를 사용할 수 있도록 GPU 드라이버를 미리 패키징 해놓아서 별도의 번거러운 작업이 필요 없습니다.

### 머신러닝 프레임웍

머신러닝 프레임웍으로는 현재 텐서플로우, 파이토치, MxNet등을 지원하는데, 플러그인 컴포넌트 형태로 제공됩니다.

### 파이프라이닝

kubeflow의 파이프라이닝은 Argo를 사용하며 이를 통해 간편하게 머신러닝 파이프라이닝을 구축할 수 있습니다.

### 하이퍼 파라미터 튜닝

학습된 모델에 대한 하이퍼 패레미터 튜닝은 Katib라는 컴포넌트를 이용해서 지원합니다.

### 모델 서빙

학습이 완료된 모델은 TFX 패키지의 일부인 Tensorflow Serving, KFServing 등등이 존재합니다..

> Kubeflow 대표 컴포넌트 

**Kubeflow’s UI – Central Dashboard**

\- 특정 작업에 대한 바로가기, 최근 노트북 목록 및 파이프 라인 목록 한번에 확인 가능

\- 파이프라인, 노트북, katib 등 클러스터에서 실행중인 컴포넌트 목록 확인 가능

\- 대시보드 컴포넌트 목록 

  . Home : Kubeflow 대시보드로 이동합니다.

  . Pipelines : Kubeflow 파이프라인 대시보드로 이동합니다.

  . Notebook Servers : 주피터 노트북 목록 화면으로 이동합니다.

  . Katib : 하이퍼파라메터 튜닝을 하는 Katb 화면으로 이동합니다.

  . Artifact Store : 아티펙트 저장소 호면으로 이동합니다.

  . Manage Contributors : 쿠버네티스 네임스페이스에 접근할 수 있는 사용자를 관리할 수 있는 화면으로 이동합니다.

****

**Jupyter Notebooks**

**-** 주피터 노트북을 생성하고 관리 할 수 있는 기능을 제공합니다.

****

**Metadata**

**-** Kubeflow 사용자가 머신 러닝 워크 플로에서 생성하는 메타 데이터를 추적하고 관리함으로써, 머신 러닝 워크 플로를 이해하고 관리 할 수 있도록 도와 줍니다.



**Frameworks for Training**

Kubeflow에서 머신 러닝 모델을 학습할 수 있도록 도와줍니다.

Kubeflow에서 제공하는 학습 프레임워크는 다음과 같습니다.

. Chainer Training

. MPI Training

. MXNet Training

. PyTorch Training

. TensorFlow Training (TFJob)



**Hyperparameter Tuning : Katib**

\- Katib는 머신 러닝 모델의 하이퍼 파라미터 및 뉴럴 아키텍처(Neural Architecture)를 자동으로 튜닝할 수 있는 기능을 제공합니다. Katib는 TensorFlow, PyTorch, Apache MXNet, XGBoost 등 다양한 머신 러닝 프레임 워크를 지원합니다.



**Pipelines**

\- Kubeflow 파이프라인은 컨테이너를 기반으로 확장 가능한 ent-to end 머신 러닝 워크 플로를 구축하기 위한 플랫폼입니다.

\- 머신 러닝 파이프라인을 관리하는 기능을 제공하여 ent-to end 오케스트레이션을 지원합니다. 그리고 수 많은 아이디어와 기술을 시도할 수 있도록 시험(trials)과 실험(experiments)을 관리할 수 있는 기능도 제공합니다.



**Tools for Serving**

\- Kubeflow는 두 가지 모델 서빙 시스템인 KFServing과 Seldon Core를 사용할 수 있습니다. KFServing과 Seldon Core는 다중 프레임워크 모델 서빙을 지원합니다. 그리고 TensorFlow Serving와 NVIDIA TensorRT Inference Server 같은 독립형 모델 제공 시스템을 사용할 수 있습니다.

. KFServing

. Seldon Serving

. NVIDIA TensorRT Inference Server

. TensorFlow Serving

<br>

**참조 강의**

[Kubeflow 101](https://www.youtube.com/playlist?list=PLIivdWyY5sqLS4lN75RPDEyBgTro_YX7x)

[Kubeflow doc](https://www.kubeflow.org/docs/)

[조대협 블로그](https://bcho.tistory.com/1301)

[Full Stack Deep Learning](https://course.fullstackdeeplearning.com/)