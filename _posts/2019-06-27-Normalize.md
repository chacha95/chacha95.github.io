---
layout: post
title: Normalization Method
tags: [Math, Deeplearning]
use_math: true
---

### Normalization(정규화)

Normalization은 statistic에서 여러가지 의미를 지니지만, neural net training과정에서 데이터 전처리 과정에서 일반적으로 쓰이는 normalization에 대해 알아봅시다.

> Normalization 식

![](https://miro.medium.com/max/341/0*oRhJXkyKqqYp8--e.)

일반적으론 위 식을 이용해 입력값 x의 범위를 [0, 1] 값으로 rescale 합니다.(경우에 따라 [-1, 1]도 있음)



### Standarization(표준화)

Standarization은 표준화로 z-transformation이라고도 합니다. 그렇게 표준화된 값을 z-score라고 부릅니다. mean만큼 빼주고, 표준 편차만큼 나누어 값을 바꾸어줍니다.

> Standarization 식

![](https://miro.medium.com/max/768/0*PXGPVYIxyI_IEHP7.)



### Norm

Norm은 절댓값에서 출발해 추상화된 개념입니다.

Norm은 벡터의 길이 혹은 크기를 측정하는 방식입니다.

Norm이 측정한 벡터의 크기는 원점에서 벡터 좌표까지의 거리 혹은 Magnitude(크기)라고 합니다.

딥러닝 training에서 overfitting을 방지하기 위한 방법으로 많이 쓰입니다.

- L1 Norm
- L2 Norm
- Max Norm
- Frobenius Norm

등이 있습니다.

[딥러닝을 위한 Norm](http://taewan.kim/post/norm/) 포스트를 읽으시는걸 추천드립니다.

<br>

**참고 강의**

[Medium](https://medium.com/@rrfd/standardize-or-normalize-examples-in-python-e3f174b65dfc)

[Blog](https://www.statisticshowto.datasciencecentral.com/normalized/)

<br>




