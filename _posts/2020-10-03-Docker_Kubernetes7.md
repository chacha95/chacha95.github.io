---
layout: post
title: 딥러닝을 위한 kubeflow 1 - 소개
tags: [Backend, Full Stack Deep Learning]
use_math: true
---

# Kubeflow란?

Kubeflow는 쿠버네티스에서 TF(TensorFlow)를 확장한 TFX(TensorFlow eXtend) 프로젝트로 시작했습니다. 

Kubeflow란 Kubernetes 환경에서 간단하게 ML 워크플로우를 배포할 수 있게 도와주는 시스템입니다. 다양한 환경에서 실행하는 ML 워크플로우를 단일 환경에서 손쉽고 직관적으로 배포하게 하는 것이 Kubeflow의 목적입니다.

실제 ML모델을 만들어서 서비스에 적용시키는 일은, 모델을 만드는 시간보다 데이터 수집과 분석 그리고 모델 튜닝에 더 많은 시간이 소모됩니다. 

그래서 이러한 일련의 과정을 묶어 파이프라인으로 구축하게 됩니다. 하지만, 서비스가 많아지고 파이프라인이 많아지면서 시스템이 복잡해지고 유지보수가 힘들어지자 'ML 플랫폼'이 필요하게 되었다. 

> 프로덕션 환경에서의 ML workflow

![](https://user-images.githubusercontent.com/31475037/94651765-f1cea800-0333-11eb-8d34-761ee240d7d8.png)

위와 같은 워크플로우를 구성하기 위해서, kubeflow는 새로운 툴들을 만든것이 아닌, kubernetes 오픈소스 생태계에 있는 여러 툴들을 가져와 쓸수 있습니다. 또한 클라우드나 On-Prem, 개인 개발 환경에 상관 없이 동일한 머신러닝 플랫폼을 손쉽게 만들 수 있기 때문에 특정 벤더나 플랫폼에 종속되지 않습니다. 또한 컨테이너 특성상 패키징이 쉬워 배포가 간단합니다.

<br>

## Kubeflow 특징

Kubeflow의 목표는 다음과 같은 업무를 수행 할 수 있도록하여, ML(머신 러닝) 모델을 확장하고 프로덕션에 최대한 간단하게 배포하는 것입니다.

- 다양한 인프라에 쉽고 반복 가능하며 이식 가능한 배포 (예 : local pc에서 실험한 다음 클라우드에 배포)
- 느슨하게 결합 된 마이크로 서비스 배포 및 관리
- 다양한 인프라 환경에서 간단히 재사용 가능하고 포터블한 배포 가능
- 요구에 따라 가능한 스케일링(auto scailing)
- 전체 ML 개발 과정 통합 관리, On-prem/클라우드 제한 없이 배포 지원 
- Kubernetes + ML flow
- ML 플로우는 ML의 라이프사이클을 관리하는 오픈소스 ML 플랫폼. 즉, kubeflow는 쿠버네티스 환경에서 ML 워크플로우를 관리하고 제작한 학습모델을 환경제한 없이 배포 및 확장할 수 있도록 돕기 위해 개발된 툴킷
- 쿠버네티스 위에서 돌기에 쿠버네티스 생태계에서 쓰이는 오픈소스를 레고 조립 하듯 조립해 쓸 수 있음(Argo, Prometheus...). 즉 쿠버네티스가 de facto가 된 지금, 강력한 확장성을 지님



<br>

**참조 강의**

[Kubeflow 101](https://www.youtube.com/playlist?list=PLIivdWyY5sqLS4lN75RPDEyBgTro_YX7x)

[Kubeflow doc](https://www.kubeflow.org/docs/)

[조대협 블로그](https://bcho.tistory.com/1301)

[Full Stack Deep Learning](https://course.fullstackdeeplearning.com/)