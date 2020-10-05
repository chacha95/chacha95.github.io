---
layout: post
title: 딥러닝을 위한 kubeflow 2 - 구성 요소
tags: [Backend, Full Stack Deep Learning]
use_math: true
---

## Kubeflow component

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

학습이 완료된 모델은 TFX 패키지의 일부인 Tensorflow Serving, KFServing 등등이 존재합니다. 

### Kubeflow’s UI – Central Dashboard

특정 작업에 대한 바로가기, 최근 노트북 목록 및 파이프 라인 목록 한번에 확인 가능

파이프라인, 노트북, katib 등 클러스터에서 실행중인 컴포넌트 목록 확인 가능

대시보드 컴포넌트 목록 

### Metadata

Kubeflow 사용자가 머신 러닝 워크 플로에서 생성하는 메타 데이터를 추적하고 관리함으로써, 머신 러닝 워크 플로를 이해하고 관리 할 수 있도록 도와 줍니다.

### Frameworks for Training

Kubeflow에서 머신 러닝 모델을 학습할 수 있도록 도와줍니다.

Kubeflow에서 제공하는 학습 프레임워크는 다음과 같습니다.

. Chainer Training

. MPI Training

. MXNet Training

. PyTorch Training

. TensorFlow Training (TFJob)

### Hyperparameter Tuning : Katib

Katib는 머신 러닝 모델의 하이퍼 파라미터 및 뉴럴 아키텍처(Neural Architecture)를 자동으로 튜닝할 수 있는 기능을 제공합니다. Katib는 TensorFlow, PyTorch, Apache MXNet, XGBoost 등 다양한 머신 러닝 프레임 워크를 지원합니다.

### Pipelines(제일 중요하게 다루기)

Kubeflow 파이프라인은 컨테이너를 기반으로 확장 가능한 ent-to end 머신 러닝 워크 플로를 구축하기 위한 플랫폼입니다.

머신 러닝 파이프라인을 관리하는 기능을 제공하여 ent-to end 오케스트레이션을 지원합니다. 그리고 수 많은 아이디어와 기술을 시도할 수 있도록 시험(trials)과 실험(experiments)을 관리할 수 있는 기능도 제공합니다.

<br>

**참조 강의**

[Kubeflow 101](https://www.youtube.com/playlist?list=PLIivdWyY5sqLS4lN75RPDEyBgTro_YX7x)

[Kubeflow doc](https://www.kubeflow.org/docs/)

