---
layout: post
title: 딥러닝을 위한 kubeflow 1 - 소개
tags: [Backend, Full Stack Deep Learning]
use_math: true
---

# Kubeflow란?

Kubeflow란 kubernetes 환경에서 간단하게 ML 워크플로우를 배포할 수 있게 도와주는 시스템입니다. 다양한 환경에서 실행하는 ML 워크플로우를 단일 환경으로 만들어 손쉽고 직관적으로 배포 합니다.

<br>

## 왜 kubeflow를 쓰는가?

실제 ML 모델을 만들어 서비스에 적용시키는 일은, 모델을 만드는 시간보다 데이터 수집과 분석 그리고 모델 튜닝에 더 많은 시간이 소모되며, 시스템이 복잡해 졌습니다. 그래서 이런 과정을 묶어 파이프라인으로 구축하게 됩니다.  그런데 이러한 파이프라인이 늘어나고, 시스템이 복잡해지자 유지보수가 힘들어지기 시작하자 kubeflow와 같은 '**ML 플랫폼**'이 등장하였습니다. 

> production level의 ML project에서 순수 ML 모델 개발은 5%도 채 안됨

![](https://user-images.githubusercontent.com/31475037/94651765-f1cea800-0333-11eb-8d34-761ee240d7d8.png)

파이프라인을 구성하기 위해 kubeflow는 새로운 툴들을 만들기보단 kubernetes 오픈소스 생태계에 있는 툴을 가져와 쓸수 있게 만들었습니다. 또한 cloud나 On-Premise 환경에 상관 없이 동일한 환경에서 개발할 수 있기 때문에 특정 벤더나 플랫폼에 종속되지 않습니다. 예를들어 모델 학습은 On-Premise 서버에서 학습을하고, 학습된 모델은 cloud에 올려 deployment하는 것입니다. 

요약하자면, kubernetes 위에서 돌아가는 오픈 소스들을 가져다 붙여 쓸수 있는 확장성 있는 **ML 플랫폼** 입니다.

> kubeflow 구조

![](https://www.kubeflow.org/docs/images/kubeflow-overview-platform-diagram.svg)



<br>

## Kubeflow 특징

Kubeflow의 특징은 다음과 같습니다. 

- 다양한 인프라에 쉽고 반복 가능한 배포 (예 : local pc에서 실험한 다음 클라우드에 배포)
- 요구에 따라 가능한 스케일링(auto scailing)
- 전체 ML 개발 과정 통합 관리, On-premise/cloud 제한 없이 배포 지원 
- ML 워크플로우를 제공하며, 여기서 ML 워크플로우라는 오픈소스는 ML project의 라이프사이클을 관리하는 오픈소스이며, kubeflow는 kubernetes 환경에서 ML 워크플로우를 관리, 배포, 확장하게 도와주는 ML 플랫폼임
- kubernetes 위에서 돌기에 kubernetes 생태계에 있는 오픈소스를 가져와 사용 가능(Argo, Prometheus...)
- kubernetes가 업계 de facto(표준)가 된 지금, 강력한 확장성을 지님

<br>

**참조 강의**

[Kubeflow 101](https://www.youtube.com/playlist?list=PLIivdWyY5sqLS4lN75RPDEyBgTro_YX7x)

[Kubeflow doc](https://www.kubeflow.org/docs/)