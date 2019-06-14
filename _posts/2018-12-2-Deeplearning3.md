---
layout: post
title: CNN(Convolutional Neural Network)
tags: [deeplearning, machine learning]
---

## CNN

### Image Representation

컴퓨터에서는 이미지는 R, G, B 3차원의 array(행렬)로 표현됩니다. 각 pixels는 0(black)-255(white)사이의 한 값으로 표현됩니다.

아래 고양이 사진에서 height가 400 pixels, width가 248 pixels이고 R, G, B 3 channel을 가진 3차원 행렬로 표현됩니다.

> 행렬로 표현된 고양이

<center><img src="http://cs231n.github.io/assets/classify.png" width="60%"></center>

> 컴퓨터가 이미지에 대해 인식을 잘 못하는 경우

- Viewpoint variation
- Illumination conditions
- Scale variation
- Deformation
- Occlusion
- Background clutter
- Intra-class variation

---- 이미지 삽입 ----



### CNN

1998년에 얀 르쿤 교수가 convolution을 이용해 zip code(우편 번호)를 인식하는 네트워크를 설계했습니다.

Convolution Network의 장점은 다음과 같습니다

- Receptive field(수용 영역)

- 영상에서의 특정 위치 픽셀은 주변 픽셀들과만 correlation이 높고, 거리가 멀수록 영향이 감소하는데 이를 잘 파악한다

- Locality : 인접 신호의 correlation 관계를 비선형적으로 추출함

- Sub sampling : feature 수를 줄임, -> maxpooling: 가장 강력한 신호만 전달

 ---- 이미지 입력 ----



### CNN parameter vs fc parameter





<br>

