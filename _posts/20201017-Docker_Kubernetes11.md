---
layout: post
title: 딥러닝을 위한 Kubeflow 3 - Pipelines란?
tags: [Backend, Full Stack Deep Learning, Kubeflow]
use_math: true
---

# Kubeflow Pipelines?

Kubeflow Pipelines 플랫폼은 다음으로 구성됩니다.

Experiment, Job 및 Run을 관리하고 추적하기위한 사용자 인터페이스 (UI).

ML workflow를 위한 엔진입니다.

Pipelines 및 구성 요소를 정의하고 조작하기위한 SDK

SDK를 사용하여 시스템과 상호 작용하기위한 Jupyter notebook

**End to end orchestration:** 기계 학습 pipeline의 오케스트레이션을 단순화합니다.

**Experiment:** 다양한 아이디어와 기술을 쉽게 시도하고 다양한 시도 / 실험을 관리 할 수 있습니다.

**Reuse:** 구성 요소와 파이프 라인을 재사용하여 매번 재 구축 할 필요없이 엔드 투 엔드 솔루션을 빠르게 생성 할 수 있습니다.

<br>

## Pipelines?

Pipelines는 workflow의 모든 구성 요소와 이러한 구성 요소가 그래프 형태로 결합되는 방식을 포함하는 ML workflow에 대한 설명입니다. Pipelines에는 Pipelines을 실행하는데 필요한 입력의 정의와 각 구성 요소의 입출력이 포함됩니다.

Pipelines  구성 요소는 파이프 라인에서 한 단계를 수행하는 Docker 이미지로 패키징 된 자체 포함 된 사용자 코드 집합입니다. 예를 들어 구성 요소는 데이터 전처리, 데이터 변환, 모델 학습 등을 담당 할 수 있습니다.

### 파이프 라인의 런타임 실행 그래프

> Kubeflow Pipelines UI에있는 파이프 라인의 런타임 실행 그래프

![](https://user-images.githubusercontent.com/31475037/96213849-e5de1980-0fb4-11eb-8871-1a5da0779862.png)

<br>

## Architecture overview

**Python SDK:** Kubeflow Pipelines 도메인 별 언어 (DSL)를 사용하여 구성 요소를 생성하거나 Pipelines을 지정합니다.

**DSL 컴파일러:** DSL 컴파일러는 Pipelines의 Python 코드를 정적 구성 (YAML)으로 변환합니다.

**파이프 라인 서비스:** Pipelines 서비스를 호출하여 정적 구성에서 실행되는 Pipelines을 만듭니다.

**Kubernetes 리소스:** Pipelines 서비스는 Kubernetes API 서버를 호출하여 Pipelines을 실행하는 데 필요한 Kubernetes 리소스 (CRD)를 만듭니다.

**오케스트레이션 컨트롤러:** 일련의 오케스트레이션 컨트롤러는 Pipelines을 완료하는 데 필요한 컨테이너를 실행합니다. 컨테이너는 가상 머신의 Kubernetes 포드 내에서 실행됩니다.

**Metadata:** Experimentation, Job, Pipelines 실행 및 모델 Evaluation.

**Artifact:**  Kubeflow Pipelines는 Minio 서버 또는 Cloud Storage와 같은 Artifact 저장소에 Artifact를 저장합니다.

**Agent 및 ML Metadata:** Pipelines Agent는 Pipelines 서비스에서 생성 된 Kubernetes 리소스를 감시하고 ML Meatadata 서비스에서 이러한 리소스의 상태를 유지합니다. Pipeline Persistence Agent는 실행 된 컨테이너 세트와 입력 및 출력을 기록합니다. 입력 / 출력은 컨테이너 매개 변수 또는 데이터 아티팩트 URI로 구성됩니다.

**Pipelines 웹 서버:** Pipelines 웹 서버는 다양한 서비스에서 데이터를 수집하여 현재 실행중인 Pipelines 목록, Pipelines 실행 기록, 데이터 아티팩트 목록, 개별 Pipelines 실행에 대한 디버깅 정보, 개별 Pipelines 실행에 대한 실행 상태를 표시합니다.

> Architecture

![](https://user-images.githubusercontent.com/31475037/96213846-e4acec80-0fb4-11eb-8676-9f60603bf277.png)

<br>

**참조 강의**

[Kubeflow 101](https://www.youtube.com/playlist?list=PLIivdWyY5sqLS4lN75RPDEyBgTro_YX7x)

[Kubeflow doc](https://www.kubeflow.org/docs/)

