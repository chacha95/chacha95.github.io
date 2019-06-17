---
layout: post
title: CNN(Convolutional Neural Network)
tags: [deeplearning]
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

> 초창기에 제안된 CNN 모델

<center><img src="https://user-images.githubusercontent.com/31475037/59593100-4f81b580-912c-11e9-8c46-4d796dda46b6.png"></center>

**Convloution Filter**

Convolution Filter란 무엇일까? 이제부터 Convolution Filter를 Conv로 부르겠습니다.

> 그림으로 표현된 Conv의 작동 원리

<center><img src="https://user-images.githubusercontent.com/31475037/59593102-501a4c00-912c-11e9-9153-f22ad1b84812.png"></center>

<center><img src="https://user-images.githubusercontent.com/31475037/59593104-501a4c00-912c-11e9-9818-f7fbdeca80fb.png"></center>

<center><img src=""></center>

<center><img src="https://user-images.githubusercontent.com/31475037/59593106-501a4c00-912c-11e9-9fbd-32e433954b46.png"></center>

<center><img src="https://user-images.githubusercontent.com/31475037/59593107-50b2e280-912c-11e9-9f25-4ac95d5f5323.png"></center>

<center><img src="https://user-images.githubusercontent.com/31475037/59593108-50b2e280-912c-11e9-8619-798ff2464b9b.png"></center>

<center><img src=""></center>

<center><img src="https://user-images.githubusercontent.com/31475037/59593110-50b2e280-912c-11e9-8cad-6446db0d0311.png"></center>

<center><img src="https://user-images.githubusercontent.com/31475037/59593111-50b2e280-912c-11e9-9a3a-77be5ce2add9.png"></center>

> Conv layer가 깊어질 수록 high level feature를 추출해 나간다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593112-514b7900-912c-11e9-9959-b4e503dce792.png"></center>

<center><img src="https://user-images.githubusercontent.com/31475037/59593113-514b7900-912c-11e9-8bcc-897929e73146.png"></center>

> Conv layer와 image size

<center><img src="https://user-images.githubusercontent.com/31475037/59593114-514b7900-912c-11e9-8484-60a51f3d54de.png"></center>

<center><img src="https://user-images.githubusercontent.com/31475037/59593115-51e40f80-912c-11e9-86b4-572dff751896.png"></center>

<center><img src="https://user-images.githubusercontent.com/31475037/59593117-51e40f80-912c-11e9-8df1-592bcfa838d2.png"></center>

> 이미지 padding

이미지 padding 기법은 영상처리에서 자주 쓰였던 기법으로 이미지 최 외곽을 특정값(0이 될 수도 있고, 근처 픽셀 값이 될수 도 있습니다)으로 채워주는 기법입니다.

이를 통해 Conv가 제대로 동작하도록 돕습니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593119-51e40f80-912c-11e9-8684-fa85e6d28767.png"></center>

**1x1 Conv**

1x1 Conv란 다음과 같습니다. Conv의 Filter 크기가 1인 Conv로 입력의 필터 크기 보다 Conv의 필터 수가 작을 때 Dimensionality Reduction 효과가 있습니다.

이런 기법을 통해 네트워크의 연산량을 적게 하면서도 네트워크를 깊게 쌓을 수 있도록 도와줍니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593122-527ca600-912c-11e9-9668-d8ca2e3e586f.png"></center>

**Pooling**

pooling 기법은 영상의 width와 hight에서

<center><img src="https://user-images.githubusercontent.com/31475037/59593123-527ca600-912c-11e9-8f52-1a457a02e16a.png"></center>

> Max Pooling

다음은 Max Pooling의 예시로 

<center><img src="https://user-images.githubusercontent.com/31475037/59593124-527ca600-912c-11e9-9663-8af75fdff026.png"></center>



<center><img src="https://user-images.githubusercontent.com/31475037/59593125-527ca600-912c-11e9-86dd-cf72042afc41.png"></center>



<center><img src="https://user-images.githubusercontent.com/31475037/59593126-53153c80-912c-11e9-8c51-3ed83a3cd272.png"></center>





### CNN parameter vs fc parameter





<br>

