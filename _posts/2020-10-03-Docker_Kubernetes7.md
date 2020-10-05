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

실제 ML모델을 만들어서 서비스에 적용시키는 일은, 모델을 만드는 시간보다 데이터 수집과 분석 그리고 모델 튜닝에 더 많은 시간이 소모됩니다. 

그래서 일련의 과정을 묶어 파이프라인으로 구축하게 됩니다. 하지만, 서비스가 많아지고 파이프라인이 많아졌습니다. 이로인해 시스템이 복잡해지고 유지보수가 힘들어지자 '**ML 플랫폼**'이 등장하였습니다. 

> 프로덕션 환경에서의 ML 워크플로우

![](https://user-images.githubusercontent.com/31475037/94651765-f1cea800-0333-11eb-8d34-761ee240d7d8.png)

위와 같은 파이프라인을 구성하기 위해, kubeflow는 새로운 툴들을 만들기보단, kubernetes 오픈소스 생태계에 있는 여러 툴들을 가져와 쓸수 있게 만들었습니다. 또한 클라우드나 On-Prem, 개인 개발 환경에 상관 없이 동일한 머신러닝 플랫폼을 손쉽게 만들 수 있기 때문에 특정 벤더나 플랫폼에 종속되지 않습니다. 

요약하자면, kubernetes 위에서 돌아가는 모든 오픈 소스들을 가져다 붙여 쓸수 있는 확장성 있는 ML 플랫폼 입니다.

> kubeflow 구조

![](https://www.kubeflow.org/docs/images/kubeflow-overview-platform-diagram.svg)



<br>

## Kubeflow 특징

Kubeflow의 특징은 다음과 같습니다. 

- 다양한 인프라에 쉽고 반복 가능한 배포 (예 : local pc에서 실험한 다음 클라우드에 배포)
- 요구에 따라 가능한 스케일링(auto scailing)
- 전체 ML 개발 과정 통합 관리, On-prem/클라우드 제한 없이 배포 지원 
- ML 워크 플로우 제공, 여기서 ML 워크 플로우라는 오픈소스는 ML project의 라이프사이클을 관리하는 오픈소스이며, kubeflow는 kubernetes 환경에서 ML 워크플로우를 관리, 배포, 확장하게 도와주는 ML 플랫폼임
- kubernetes 위에서 돌기에 kubernetes 생태계에 있는 오픈소스를 가져와 사용 가능(Argo, Prometheus...)
- kubernetes가 업계 de facto(표준)가 된 지금, 강력한 확장성을 지님

<br>

**참조 강의**

[Kubeflow 101](https://www.youtube.com/playlist?list=PLIivdWyY5sqLS4lN75RPDEyBgTro_YX7x)

[Kubeflow doc](https://www.kubeflow.org/docs/)

[조대협 블로그](https://bcho.tistory.com/1301)