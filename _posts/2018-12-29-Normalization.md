---
layout: post
title: Normalization
tags: [deeplearning]
---

## Normalization

이 포스트에서는 Normalization 기법에 대해 간단히 리뷰해 보겠습니다.

### Feature Scaling

**Norm**

Norm(Normalization)의 목표는 입력 값의 범위의 차이를 왜곡시키지 않기 위해서 사용하는 방법입니다.

데이터 처리에 있어 각 특성의 스케일을 조정한다는 의미로 feature scaling이라고 불립니다.

Norm은 흔히 스케일 조정 중 Min-Max Scalar의 의미로 사용되기도 합니다.(데이터를 [0, 1]사이로 스케일 조정)

**standarization**

Standard Sclar 혹은 z-score normalization을 의미합니다. 기존 데이를 평균 0, 표준 편차 1로 데이터를 만듭니다.

평균을 기준으로 얼마나 떨어져 있는지를 살펴볼 때 보통 사용합니다. 보통 데이터 분포가 가우시안 분포를 따를 때 유용합니다.

-- 수식 삽입 --

**왜 이런 feature scaling을 하나?**

scale의 범위가 너무 크면 노이즈 데이터가 생성되거나 overfitting이 될 가능성이 높아집니다.

activation function을 거치는 의미가 사라집니다 -> 값이 너무 커지게 되면 활성화 함수를 거친다고 하여도 한쪽으로 값이 쏠릴 가능성이 높기 때문입니다.

weight 초기화 방법으로 정규분포로 부터 값을 생성하는 방법을 자주 사용하는데, 정규 분포의 값은 +=2.58 내에 99%가 존재합니다. 따라서, scale이 너무 커서 값의 분포 범위가 넓으면 값을 정하기가 힘듭니다.

### Regularization

Regularization 기법

- Early stopping
- Noisy input
- drop-out
- emsemble

<br>

### Batch Norm

이 정규화 방법은 Gradient Vanishing / Gradient Exploding이 일어나지 않도록 하는 아이디어 중 하나입니다. 

- Gradient Vanishing : 신경망에서 손실 함수의 gradient 값이 0에 근접하여, backpropagation이 잘 되지 않는 문제
- Gradient Exploding : error gradient가 축적되어 가주치 업데이트 시 overflow 되는 현상

**Internal Covariance Shift**

Batch Norm 논문에서는 위에서 언급된 문제의 근본적인 원인이 Internal Covariance Shift라고 주장했습니다(경험적으로 그렇다~~) 이는 네트워크의 각 층이나 활성 함수마다 입력의 값들의 분포가 계속 바뀌는 현상을 의미합니다.

그래서 standarization과 유사한 식으로 mini-batch 단위로 평균을 0 표준편차를 1로 만듭니다

-- 수식 ---

- mini-batch의 평균 계산
- mini-batch의 분산과 표준 편차
- normalize(입실론을 제외하면 standarization)
- 스케일 조정 및 분포 조정

해당 논문에서는 이 방법의 장점이 다음과 같다고 이야기 하고 있습니다.

- backpropagation에서 parameter의 scale 영향을 받지 않으므로, learning rate를 크게 설정 할 수 있다.
- weight regularization과 Dropout(이 방법과 성능이 같지만 느림)을 제외할 수 있어, 학습 속도를 향상시킬 수 있다.

하지만 최근 주장으로는 BN의 성공은 Internal Covariance Shfit와 상관이 없다고 주장합니다. 

batch norm이 잘되는 이유

- 신경망에서 가중치의 변경은 그와 연결된 layer에 영향을 미칩니다.
- 이는 가중치의 변화는 매우 복잡하게 신경망에 영향을 미칩니다.
- 그렇기 때문에 Vanishing 또는 Exploding을 막기 위해 small learning rate를 사용하거나, 활성화 함수 사용을 선택합니다.
- Batch Norm은 최적화를 하기 쉽게 만들기 위해 네트워크를 reparameterization합니다.
- 이 방법은 모든 레이어의 평균, 크기, 활성화 함수를 독립적으로 조정할 수 있게 만듭니다.



bach norm 정리

- loss surface를 보다 쉽게 찾음
- 최적화를 쉽게 만듬
- higher learning rate를 사용할 수 있게 만듬
- 여러 작업에서 모델 성능 향상



<br>

### Layer Norm

BN과 LN은 거의 유사한 형태를 지닙니다. 위의 큐브 사진 또는 아래이 사진이 이를 시각화한 모양입니다

-- 사진 삽입 -

LN은 mini-batch의 feature 수가 같아야 합니다.

각각 BN과 LN은 Batch 차원에서 Normalization / Feature 차원에서 Normalization 이라는 차이가 있습니다

BN의 경우에는 mini-batch 단위로 계산이 이뤄지며, 각 mini-batch에서 동일하게 계산합니다. 하지만 LN의 경우에는 각 특성에 대하여 따로 계산이 이뤄지며, 각 특성에 독립적으로 계산합니다.

LN은 batch 사이즈와 상관이 없습니다. 이는 RNN에서 좋은 성능을 보입니다.

<br>

### Instance Norm

Instance Norm은 LN과 비슷하지만 한 단계 더 나아가 평균과 표준 편차를 구하여 channel 단위의 normalization을 진행합니다.

Style Transfer에서 좋은 성능을 냈습니다.

<br>

### Group Norm

IN과 LN의 중간입니다. 여기서는 채널을 그룹으로 묶어 평균과 표준편차를 구한다는 점입니다. 모든 채널이 단일 긃에 있다면 LN, 각 채널을 다른 그룹에 배치하면 IN 입니다.

LN과 IN은 모두 공통적으로 RNN과 Style Transfer에 효과적이지만, BN에 비해 이미지 인식면에서는 성능이 좋지 못합니다.

Group Norm의 경우 ImageNet Classification task에서 batch size가 32일 때 BN과 거의 근접한 성능을 냈고, 그 보다 작은 batch size에 대해서는 더 좋은 성능을 냈습니다(BN의 경우 batch size가 작아지면 성능이 안좋아짐)

왜 GN이 LN 혹은 IN보다 효과적일까요?

LN은 모든 채널이 동등하게 중요하다 라는 가정하에 정규화가 진행됩니다. 하지만 이미지의 경우 이미지 가장자리와 중심부의 중요성은 다르다는 것을 경험적으로 알 고 있습니다. 즉 여러 채널에서 서로 다르게 계싼하면 모델에 따라 유연성을 제공할 수 있습니다. 또한 이미지의 경우 각 채널은 독립된 것이 아니므로(IN) 주변 채널을 활용하여 더 정규화를 넓게 적용할 수 있습니다.

-- 루닛 블로그 참조

<br>

### Batch-Instance Norm

Batch-Instance Norm은 이미지에서 style과 contrast의 차이를 설명하기 위해 IN을 확장한 정규화 입니다.

IN의 문제점은 style 정보를 완전히 지운다는 것입니다. style transfer에는 유용할 수 있으나, weather classification과 같이 스타일이 중욯ㄴ 특징일 때는 문제가 될 수 있습니다.

--루닛 블로그 참조

### Spectral Norm





<br>

