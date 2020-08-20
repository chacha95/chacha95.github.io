---
layout: post
title: 딥러닝을 위한 kubeflow1 - 소개
tags: [Backend]
use_math: true
---

# Kubeflow란?

Kubeflow는 쿠버네티스에서 TF(TensorFlow)를 확장한 TFX(TensorFlow eXtend) 프로젝트로 시작했습니다. 왜냐하면 실제 ML 프로덕션 환경에선 모델연구 외 다른 일들이 훨씬 많기 때문입니다.

> 프로덕션 환경에서의 ML workflow

![](https://user-images.githubusercontent.com/31475037/90581600-9a063280-e206-11ea-918e-611979e51c00.png)

위의 파이프라인 흐름을 시스템 아키텍쳐로 표현하면 다음과 같습니다.

> ML workflow pipeline

![](https://user-images.githubusercontent.com/31475037/90581688-d2a60c00-e206-11ea-9208-3badc5bbf034.png)

이런 상황에서 google에서 만든 Kuberflow는 쿠버네티스 플랫폼 위에서 작동하는 ML production을 위한 통합 플랫폼입니다.

위와 같은 아키텍쳐를 구성하기 위해서, kubeflow는 새로운 툴들을 만든것이 아닌, 쿠버네티스 오픈소스 생태계에 있는 여러 툴들을 통합하여 제공합니다. 이를 통해 쿠버네티스에 이해도가 엄청 높지 않아도 사용하기가 용이합니다. 

- 클라우드나 On-Prem, 개인 개발 환경에 상관 없이 동일한 머신러닝 플랫폼을 손쉽게 만들 수 있기 때문에 특정 벤더나 플랫폼에 종속되지 않음
- 컨테이너로 패키징이 되있어 배포가 간단함

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

<br>

**참조 강의**

[Kubeflow 101](https://www.youtube.com/playlist?list=PLIivdWyY5sqLS4lN75RPDEyBgTro_YX7x)

[Kubeflow: Machine Learning on Kubernetes](https://www.youtube.com/watch?v=HBxyLnEzyhw&t=17s)

[쿠버네티스 기반의 End2End 머신러닝 플랫폼 Kubeflow #1 - 소개](https://bcho.tistory.com/1301)



