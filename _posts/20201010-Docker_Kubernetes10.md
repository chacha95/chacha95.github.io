---
layout: post
title: 딥러닝을 위한 Kubeflow 2 - 구성 요소
tags: [Backend, Full Stack Deep Learning, Kubeflow]
use_math: true
---

이번 포스트에선 kubeflow의 components들에 대해 다루겠습니다.

# Dashboard

Kubeflow 배포에는 클러스터에 배포 된 Kubeflow 구성 요소에 대한 빠른 액세스를 제공하는 중앙 대시 보드가 포함됩니다. 대시 보드에는 다음 기능이 포함됩니다.

- 특정 작업에 대한 바로 가기, 최근 파이프 라인 및 노트북 목록, 지표를 통해 작업 및 클러스터에 대한 개요를 하나의보기로 제공
- Pipeline, Katib, Jupyter notebook 등을 포함하여 클러스터에서 실행되는 구성 요소의 UI

### Kubeflow UI 개요

Kubeflow UI에는 다음이 포함됩니다.

- **Home:** Kubeflow 구성 요소 간 탐색을위한 중앙 대시 보드
- **Pipeline:** Kubeflow Pipelines
- **Notebook Servers**
- **Katib:** Hyperparameter tuning
- **Artifact Store:** artifact metadata tracking
- **Manage Contributors:** Kubeflow 배포의 네임 스페이스에서 사용자 액세스 공유를 위해 contributor 관리

중앙 대시 보드는 다음과 같습니다.

![](https://user-images.githubusercontent.com/31475037/96206784-a0195500-0fa4-11eb-94b1-edf8778def81.png)

# Metadata

Kubeflow에서 ML workflow metadata 추적 및 관리

Meatadata 프로젝트의 목표는 ML workflow가 생성하는 metadata를 추적, 관리하여 유저가 ML workflow를 관리하도록 함

Metadata는 모델, dataset 및 기타 artifact에 대한 정보를 의미

Artifact는 ML workflow에서 구성 요소의 입출력을 형성하는 파일입니다.

# Fairing

Build, train, and deploy your ML training jobs remotely

Kubeflow Fairing은 하이브리드 클라우드 환경에서 ML train job을 구축, train and deployment하는 프로세스를 간소화합니다.

Kubeflow Fairing을 사용하고 몇 줄의 코드를 추가하면 Python 코드 또는 Jupyter 노트북에서 직접 로컬 또는 클라우드에서 ML train job을 실행할 수 있습니다. Train job이 완료된 후 Kubeflow Fairing을 사용하여 trained 모델을 end point로 deployment 할 수 있습니다.

Kubeflow Fairing은 Jupyter 노트북,  Python 파일을 Docker 이미지로 패키징 한 다음 Kubeflow 또는 AI Platform에서 training job or deployemnt. 훈련 작업이 완료된 후 Kubeflow Fairing을 사용하여 훈련 된 모델을 Kubeflow에서 예측 엔드 포인트로 배포 할 수 있습니다.

- **손쉬운 ML 학습 작업 패키징:** ML 실무자가 ML 모델 학습 코드와 해당 코드의 종속성을 Docker 이미지로 쉽게 패키징
- **하이브리드 클라우드 환경에서 손쉽게 ML 모델 학습:** 인프라를 추상화 시켜, ML 모델 train을 위한 API를 제공
- **trained 모델 배포 프로세스 간소화:** On-premise 서버에서 학습시킨 모델을 클라우드 환경에 쉽게 deployment 할 수 있습니다.

# Kubeflow Pipelines

Kubeflow Pipelines는 Docker 컨테이너를 기반으로하는 확장 가능한 ML workflow를 구축하고 배포하기위한 플랫폼입니다.

Kubeflow Pipelines 플랫폼은 다음으로 구성됩니다.

Experiment, Job 및 Run을 관리하고 추적하기위한 사용자 인터페이스 (UI).

ML workflow를 위한 엔진입니다.

Pipelines 및 구성 요소를 정의하고 조작하기위한 SDK

SDK를 사용하여 시스템과 상호 작용하기위한 Jupyter notebook

## Kubeflow Pipelines의 목표

**end to end orchestration:** 기계 학습 pipeline의 오케스트레이션을 단순화합니다.

**Experiment:** 다양한 아이디어와 기술을 쉽게 시도하고 다양한 시도 / 실험을 관리 할 수 있습니다.

**Reuse:** 구성 요소와 파이프 라인을 재사용하여 매번 재 구축 할 필요없이 엔드 투 엔드 솔루션을 빠르게 생성 할 수 있습니다.

# Katib

**beta version**, Hyperparameter tunning and NAS tool

모델 튜닝 툴

# Multi-Tenancy in Kubeflow

## Multi-user Isolation

프로덕션 환경에서는 서로 다른 팀과 사용자간에 동일한 리소스 풀을 공유해야하는 경우가 많습니다. 이러한 다양한 사용자는 실수로 서로의 리소스를 보거나 변경하지 않고 자신의 리소스를 격리하고 보호 할 수있는 안정적인 방법이 필요합니다.

Kubeflow v1.1.0은 배포에서 네임 스페이스 및 사용자 생성 리소스에 대한 액세스 제어를 적용하는 다중 사용자 격리를 지원합니다. 이 기능은 사용자에게 노트북 검색, 교육 작업, 배포 및 기타 리소스 제공의 편리함을 제공합니다. 격리 메커니즘은 또한 배포에서 다른 사용자의 리소스를 실수로 삭제 / 수정하는 것을 방지합니다.

### 주요 개념

**Manger:** Manager는 Kubeflow 클러스터를 만들고 유지 관리하는 사람입니다. 이 사람은 다른 사람에게 액세스 권한을 부여 할 권한이 있습니다.

**User:** User는 클러스터의 일부 리소스 집합에 액세스 할 수있는 사람입니다. User는 관리자로부터 액세스 권한을 부여 받아야합니다.

**Profile:** Profile은 사용자가 소유 한 모든 Kubernetes 클러스터의 그룹입니다.

# Tools for Serving

## KFServing

**Beta version, KFServing은 Kubeflow 1.1에서 작동 가능**

KFServing은 Kubernetes에서 serverless inference를 지원하고 TensorFlow, XGBoost, scikit-learn, PyTorch, ONNX와 같은 일반적인 머신 러닝 (ML) 프레임 워크에 대해 고성능의 높은 추상화 인터페이스를 제공, 프로덕션 모델을 해결합니다.

### 주요 기능

임의의 프레임 워크에서 ML 모델을 제공하기위한 Kubernetes 커스텀 리소스 정의를 제공합니다.

auto scailing, networking, status check, 서버 구성의 복잡성을 캡슐화하여 GPU 자동 확장, 0으로 확장, 카나리아 롤아웃과 같은 최첨단 서비스 기능을 ML 배포에 제공합니다.

예측, 사전 처리, 사후 처리 및 설명 가능성을 즉시 제공하여 프로덕션 ML 추론 서버에 대한 간단하고 플러그 가능하며 완전한 스토리를 지원합니다.

> KFServing

![](https://user-images.githubusercontent.com/31475037/96206778-9e4f9180-0fa4-11eb-883f-8cfeb02d2969.png)

<br>

**참조 강의**

[Kubeflow 101](https://www.youtube.com/playlist?list=PLIivdWyY5sqLS4lN75RPDEyBgTro_YX7x)

[Kubeflow doc](https://www.kubeflow.org/docs/)

