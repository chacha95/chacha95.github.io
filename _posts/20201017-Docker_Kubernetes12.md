---
layout: post
title: 딥러닝을 위한 Kubeflow 4 - 모델 서빙 비교
tags: [Backend, Full Stack Deep Learning, Kubeflow]
use_math: true
---

ML Model Serving이란 다른 소프트웨어에서 ML 모델을 사용할 수 있도록 하는 것입니다.

다양한 Serving tool들이 있지만, TensorFlow Serving, TorchServe, KFServing을 리뷰하겠습니다.

<br>

# TensorFlow Serving(TF Serving)

TensorFlow Serving은 프로덕션 환경을 위해 설계된 ML을 위한 유연한 고성능 production system입니다. TensorFlow Serving을 사용하면 동일한 서버 아키텍처와 API를 유지하면서 새로운 알고리즘과 실험을 쉽게 배포 할 수 있습니다.

<br>

## Key Concepts

### **Servables**

Servable은 TensorFlow Serving의 핵심 추상화입니다.

Servable은 클라이언트가 연산을 수행하는 데 사용하는 기본 개체입니다.

Servable은 모든 유형 및 인터페이스가 될 수 있으므로 다음과 같은 유연성 및 향후 개선이 가능합니다.

- streaming results
- experimental APIs
- asynchronous modes

### **Servable Version**

TensorFlow Serving은 단일 서버 인스턴스가 하나 이상의 서빙 가능 버전을 처리 할 수 있습니다. 이를 통해 새로운 알고리즘 구성, 가중치 및 기타 데이터를 시간에 따라 로드 할 수 있습니다. 버전을 사용하면 둘 이상의 서빙 가능 버전을 동시에로드 할 수 있으므로 점진적인 롤아웃 및 experimentation을 지원합니다. 제공 시간에 클라이언트는 특정 모델에 대해 최신 버전 또는 특정 버전 ID를 요청할 수 있습니다.

### **Servable Streams**

Servable stream은 연속된 versions of servable을 정렬해 제공합니다.

### Model

TensorFlow Serving은 모델을 하나 이상의 서빙 가능 항목으로 나타냅니다. ML 모델에는 하나 이상의 알고리즘 (학습 된 가중치 포함)과 조회 또는 임베딩 테이블이 포함될 수 있습니다.

### Loader

Loader는 서비스의 lifecycle을 관리합니다. Loader API는 관련된 특정 학습 알고리즘, 데이터 또는 제품 사용 사례와 독립적 인 공통 인프라를 지원합니다. 특히, 로더는 서비스 로드 및 언로드하기 위해 API를 표준화합니다.

### **Sources**

Sources는 서비스를 찾아 제공하는 플러그인 모듈입니다. 각 소스는 0개 이상의 제공 가능한 스트림을 제공합니다. 제공 가능한 각 스트림에 대해 소스는 로드 할 수있는 각 버전에 대해 하나의 로더 인스턴스를 제공합니다.

TensorFlow Serving의 Sources 인터페이스는 임의의 저장소 시스템에서 제공 가능한 항목을 검색 할 수 있습니다. TensorFlow Serving에는 일반적인 참조 소스 구현이 포함됩니다. 예를 들어 소스는 RPC와 같은 메커니즘에 액세스하고 파일 시스템을 폴링 할 수 있습니다.

소스는 여러 서비스 또는 버전에서 공유되는 상태를 유지할 수 있습니다. 이는 버전간에 델타 (diff) 업데이트 하는 시스템에 유용합니다.

### **Aspired Versions**

Aspired versions는 로드되고 준비되어야하는 제공 가능한 버전 집합을 나타냅니다. 소스는 한 번에 제공 가능한 단일 스트림에 대해이 제공 가능한 버전 세트를 전달합니다. 소스가 관리자에게 추천 버전의 새 목록을 제공하면 제공 가능한 스트림의 이전 목록을 대체합니다. Manager는 더 이상 목록에 나타나지 않는 이전에로드 된 버전을 언로드합니다.

### **Managers**

Manager는 다음을 포함하여 Servable의 전체 수명주기를 처리합니다.

- Servable 로드
- 서비스 제공
- Servable 언로드

Managers는 소스를 듣고 모든 버전을 추적합니다. 관리자는 소스의 요청을 이행하려고하지만 필요한 리소스를 사용할 수없는 경우 원하는 버전의 로드를 거부 할 수 있습니다. 관리자는 "언로드"를 연기 할 수도 있습니다. 예를 들어 관리자는 항상 하나 이상의 버전이로드되도록 보장하는 정책에 따라 최신 버전이 로드를 완료 할 때까지 언로드를 기다릴 수 있습니다.

요약하자면

1. Sources가 Servable versions를 위해 Loaders를 생성합니다.
2. Loaders가 Aspired Versions을 Manager에게 보내고, 이를 통해 client의 요청에 loads, serves 답변합니다.

> Life of a Servable

![](https://user-images.githubusercontent.com/31475037/96213587-58022e80-0fb4-11eb-8cca-a87499d5f009.png)

<br>

# TorchServe

PyTorch는 연구에서 많이 채택되었지만 사람들은 PyTorch 모델을 프로덕션에 얼마나 잘 적용 할 수 있는지에 대해 혼란 스러울 수 있습니다.

사람들이 모델을 '프로덕션'으로 가져가는 것에 대해 이야기 할 때, 일반적으론 inference를 수행하는 것을 의미하며 때로는 모델 evaluation 또는 inference 또는 deployment라고도합니다.

Facebook에서는 PyTorch를 사용하여 하루에 수백조 번 inference 작업을 수행하기 때문에 inference 가능한 한 효율적으로 실행되도록 많은 노력을 기울였습니다.

<br>

## Serving Strategies

하지만 inference에서 모델을 사용하는 방법에 대한 확대는 일반적으로 전체 이야기가 아닙니다. 실제 ML system에서는 Jupyter 노트북에서 단일 inference 작업을 실행하는 것 이상을 수행해야하는 경우가 많습니다. 일반적으로 다음 접근 방식 중 하나를 사용할 수 있습니다.

### Direct embedding

모바일과 같은 애플리케이션 설정에서는 종종 더 큰 프로그램의 일부로 모델을 직접 호출합니다. 이것은 앱만을위한 것이 아닙니다. 일반적으로 로봇 공학 및 전용 장치도 작동합니다. 코드 수준에서 모델에 대한 호출은 위에 표시된 추론에 대한 섹션에서 위에 표시된 것과 정확히 동일합니다. 주요 관심사는 종종 이러한 환경에 Python 인터프리터가 없다는 것입니다. 그렇기 때문에 PyTorch를 사용하면 Pytorch C ++를 사용합니다. 모바일을 사용하거나 로봇과 같은 임베디드 시스템에서 작업하는 경우 애플리케이션에 직접 임베딩하는 것이 올바른 선택 인 경우가 많습니다. 특히 모바일의 경우 사용 사례는 **ONNX** import 기능에 의해 제공 될 수 있습니다. 그러나 ONNX는 본질적으로 제한이 있으며, 더 큰 PyTorch 프로젝트에서 제공하는 모든 기능을 지원하지 않습니다. 로봇과 같은 다른 임베디드 시스템의 경우 C ++ API에서 PyTorch 모델에 대한 inference을 실행하는 것이 올바른 솔루션이 될 수 있습니다.

### Model microservices(MSA)

서버 모델을 사용하고 여러 모델을 관리하는 경우, 일반적으로 Docker와 같은 일종의 패키징 메커니즘을 사용하여 각 개별 모델 (또는 각 개별 모델 버전)을 별도의 서비스로 분할합니다. 해당 서비스는 HTTP를 통한 JSON 또는 gRPC와 같은 RPC 기술을 사용하여 서비스를 통해 네트워크에 액세스 할 수 있습니다.

주요 특징은 모델을 호출하는 단일 엔드 포인트로 서비스를 정의한다는 것입니다. 그런 다음 이미 서비스를 관리하는 데 사용하는 시스템 (예 : kubernetes)을 통해 모든 모델 관리를 수행합니다.

### Model servers

모델을 관리하고 제공하기 위해 빌드 된 애플리케이션입니다. 이를 통해 여러 모델을 업로드하고 각 모델에 대한 end-point를 설정합니다. 이러한 시스템에는 모델 관리 및 제공에 대한 전체 문제를 해결하는 데 도움이되는 다른 기능이 포함되어 있습니다. 여기에는 metric, visualization, data preprocessing 등이 포함되 있습니다. 또한 모델 롤 백 롤 아웃 기능 등을 지원합니다.

대표적인 예시로는 kubernetes cluster 위에서 kubeflow로 ML pipelines로 전체 학습 과정을 관리하고, data versioning 툴을 dolt를 쓰고, Prometheus와 Grafana로 visalization을 하는 것입니다.

<br>

## TorchServe Architecture

> TorchServe Architecture

![](https://user-images.githubusercontent.com/31475037/96213585-56386b00-0fb4-11eb-8a40-7e6ffddca6b1.png)

**Front-end** : TorchServe의 요청 / 응답 처리 구성 요소입니다. 서빙 컴포넌트의이 부분은 클라이언트로부터 오는 요청 / 응답을 모두 처리하고 모델의 수명주기를 관리합니다.

**Worker process:**이 작업자는 모델에 대한 실제 추론을 실행합니다. 이들은 모델의 실제 실행 인스턴스입니다.

**Model:** 모델은 script_module (JIT 저장 모델) 또는 eager_mode_models 일 수 있습니다. 이러한 모델은 state_dicts와 같은 다른 모델 아티팩트와 함께 데이터의 사용자 지정 사전 및 사후 처리를 제공 할 수 있습니다. 모델은 클라우드 스토리지 또는 로컬 호스트에서로드 할 수 있습니다.

**Plugin:** TorchServe에 드롭 할 수있는 사용자 지정 엔드 포인트 또는 authz / authn 또는 일괄 처리 알고리즘입니다.

**Model Storage:** 로드 가능한 모든 모델이 존재하는 디렉토리입니다.

<br>

# KF Serving

**Beta version**

KFServing은 Kubernetes에서 serverless inference를 지원하고 TensorFlow, XGBoost, scikit-learn, PyTorch, ONNX와 같은 일반적인 머신 러닝 (ML) 프레임 워크에 대해 고성능의 높은 추상화 인터페이스를 제공, 프로덕션 모델을 해결합니다.

### 주요 기능

임의의 프레임 워크에서 ML 모델을 제공하기위한 Kubernetes 커스텀 리소스 정의를 제공합니다.

auto scailing, networking, status check, 서버 구성의 복잡성을 캡슐화하여 GPU 자동 확장, 0으로 확장, 카나리아 롤아웃과 같은 최첨단 서비스 기능을 ML 배포에 제공합니다.

예측, 사전 처리, 사후 처리 및 설명 가능성을 즉시 제공하여 프로덕션 ML 추론 서버에 대한 간단하고 플러그 가능하며 완전한 스토리를 지원합니다.

> KFServing

![](https://user-images.githubusercontent.com/31475037/96206778-9e4f9180-0fa4-11eb-883f-8cfeb02d2969.png)

<br>

**참고 자료**

[PyTorch](https://pytorch.org/blog/model-serving-in-pyorch/)

[Architecture | TFX | TensorFlow](https://www.tensorflow.org/tfx/serving/architecture)