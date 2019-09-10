---
layout: post
title: CNN Light Way
tags: [Machine Learning]
use_math: true
---

## CNN Light Way

이번 포스트에서는 CNN의 문제점과 이를 해결하기 위한 방안, 또 CNN을 Light(가볍게)하게 만드는 방법에 대해 알아봅시다.

### Problem of CNN

일반적인 2D Convolution을 기반으로한 CNN 모델에서는 다음과 같은 문제점이 존재합니다.

- Expensive Cost

  네트워크는 점점 깊어지고, 채널의 수가 증가하고 있습니다. 그에따라 연산량과 학습해야할 parameter가 비례해서 증가하고 있습니다.

  다만 네트워크가 깊어짐에 따라 receptive field도 증가함으로서 네트워크가 더 넓은 영역을 볼 수 있게 됬습니다.

  대다수의 모델에서 이러한 연산량과 학습해야할 parameter의 수를 줄이기 위해 pooling을 사용합니다.

  채널의 수를 늘릴수록 더 많은 filter를 학습 할수 있지만, 그에따라 학습해야할 parameter 수 증가, 학습속도의 하락, overfitting 문제가 산재해 있습니다.

- Dead Channels

  학습 과정에서, 출력 결과에 영향을 거의 주지 않는 정점이 나타납니다.

  이러한 문제로인해 모델의 연산량이 계속해서 증가하게 됩니다.

  > Dead Channels의 예시, 초록색은 영향을 미치는 채널, 회색은 영향이 거의 없는 채널

  <center><img src="https://user-images.githubusercontent.com/31475037/63741860-a85e8e80-c8d1-11e9-8ee1-39abf98e92c5.PNG"></center>

- Low Correlation between channels

  모든 채널-필터 쌍이 높은 correlation을 가질 수 만은 없습니다.

  sparse한 correlation matrix 생성이 되면 연산량이 늘어나지만, 효율성은 떨어집니다.
  
  

이러한 문제점을 해결하기 위해 제안된 방법에 대해 알아봅시다.

<br>

### Dilated Convolution

영상 내의 Objects에 대한 정확한 판단을 위해서는 Contextual Information이 중요합니다.

- 객체 주변의 배경은 어떤가?
- 객체 주변의 다른 객체들은 어떤 종류인가?

만일 receptive field가 충분히 확보되지 않았다면 영상의 일부분만 보고 판단을 하게되고, 이로인해 머핀과 치와와를 구분하지 못하는 일이 생기게 됩니다.

> Receptive Field의 중요성

<center><img src="https://user-images.githubusercontent.com/31475037/63749576-b2d75300-c8e6-11e9-83ff-d42f545ed8f1.PNG"></center>
충분한 Contextual Information 확보를 위해 넓은 Receptive field가 중요하다가 바로 요점입니다.

일반적으로 CNN에서 Receptive Field 확장을 위해선 두가지 방법이 쓰입니다.

- kernel size의 확장
- 더 많은 Conv layer 사용

하지만 두 방법 모두 연산량을 크게 증가시키는 단점이 있습니다.

그래서 나온 방식이 바로 Dilated Convolution입니다. Dilated Conv는 Conv 필터에 간격을 둔 filter입니다.

입력 픽셀 수는 동일 하지만, 더 넓은 범위에 대해 입력 수용 가능합니다.

> Dilated Convolutions

<center><img src="https://cdn-images-1.medium.com/max/1200/1*SVkgHoFoiMZkjy54zM_SUw.gif"></center>
Dilated Convolution은 필터 내부에 padding을 추가해 강제로 recpetive field를 늘리는 방식입니다.

위 그림에서 진한 파란 부분만 weight가 있고 나머지 부분은 0으로 채워집니다.

pooling을 수행하지 않고도 receptive field를 크게 가져갈 수 있습니다.



### Convolution Factorization

Convolution Factorization을 통해 receptive field의 크기는 유지한채 계산량을 줄일 수 있습니다.

> Factorization

![](https://user-images.githubusercontent.com/31475037/63760100-ae686580-c8f9-11e9-9501-0c739d8bde25.PNG)



이러한 Factorization이 적용된 것이 Inception v2입니다. 

> Inception module의 Fatorization

![](https://user-images.githubusercontent.com/31475037/63759870-369a3b00-c8f9-11e9-869d-0ae2902f7ed0.PNG)





<br>

### Point-wise Convolution

Point-wise Convolution은 커널의 크기가 1x1로 고정된 Convolution layer를 말합니다.

Point-wise Convolution은 하나의 필터는 채널 별로 Coefficient를 가지는 Linear Combination으로 볼 수 있습니다.

> 1x1 Conv

<center><img src="https://user-images.githubusercontent.com/31475037/59593122-527ca600-912c-11e9-9668-d8ca2e3e586f.png"></center>
일반적으로는 Dimensionality Reduction(DR) 기법으로 많이 사용합니다.

왜냐하면 출력 채널의 수가 입력 채널의 수보다 적게 되면, DR의 효과가 있기 때문입니다.

이러한 특성을 이용해, 전체 채널수를 줄여 Dead Channel 문제를 해소합니다.

> 채널수를 줄여 Dead Channel 문제 해소

![](https://user-images.githubusercontent.com/31475037/63741937-00959080-c8d2-11e9-8fa2-e40d468421f7.PNG)



또한 point-wise Conv는 여러 구조에서 실전적으로 적용되어, 경량화와 성능 개선이 가능함을 실험적으로 증명하였습니다.

<br>

### Group Convolution

Group Convolution은 입력 영상의 채널들을 여러개의 그룹으로 나누어 독립적인 Convolution을 수행하는 방식입니다.

이 아이디어는 AlexNet에서 처음 제안되었으며, 이후 ResNext 구조에서 Cardinality의 개념으로 확장됩니다.

> Group Conv

![](https://user-images.githubusercontent.com/31475037/63742178-f0ca7c00-c8d2-11e9-84b3-654c675f75d4.PNG)

AlexNet에서 첫번째 convolution layer를 분석해 보았을 때, 각각의 그룹이 서로 다른 특성을 학습을 하는것을 확인할 수 있습니다.

즉, 각 그룹마다 독립적인 feature의 학습을 기대할 수 있습니다.

> 외곽선, 음영 위주 학습(위), 색상, 패턴 위주 학습(아래)

<center><img src="https://user-images.githubusercontent.com/31475037/63742179-f0ca7c00-c8d2-11e9-971d-12352d7dd3e3.PNG"></center>
또한 그룹 수를 조정하면 성능과 parameter의 수가 변경되는데, 이는 사용자가 HyperParameter로서 조정할 수 있습니다.

> group 수에 따른 결과 차이

<center><img src="https://user-images.githubusercontent.com/31475037/63742181-f0ca7c00-c8d2-11e9-8fb3-65d078af3a86.PNG"></center>
<br>

### Depth-wise Convolution

일반적인 Convolution Filter는 입력 영상의 모든 채널의 영향을 받게 됩니다.

즉, 완벽히 특정 채널만의 Spatial Feature를 추출하는 것은 불가능 합니다.

Depth-wise Convolution은 각 단일 채널에 대해서만 수행되며, 각 채널의 Spatial Feature에만 집중합니다.

<center><img src="https://user-images.githubusercontent.com/31475037/63750785-1793ad00-c8e9-11e9-9157-4f5489a9c708.PNG"></center>
### Depth-wise Separable Convolution

기존의 Convolution에서 Cross-channel Correlation을 완벽히 분리하기 위해 제안된 구조입니다.

각 채널 별 Spatial Convolution 이후 Channel별 Linear Combination을 진행합니다.

기존의 convolution과 비슷한 작용을 하지만 계산량에 있어 현저히 적은 계산 비용을 보입니다.

<center><img src="https://user-images.githubusercontent.com/31475037/63750783-1793ad00-c8e9-11e9-9202-c6af86ccb1a6.PNG"></center>
<br>

### Xception

Xception 구조는 Inception 모듈과 Depth-wise separable convolution의 중간에 있는 모델로 계산 비용에 있어 이점과 함께, 기존 모델들에 비해 성능 측면에서도 좋은 성능을 냈습니다.

또 극단적으로 모델의 width를 늘려 좋은 성능을 냈습니다.

> Xception 구조, point-wise convolution을 먼저 한다.

<center><img src="https://user-images.githubusercontent.com/31475037/63760650-ab21a980-c8fa-11e9-80aa-549967e93af5.PNG"></center>
<br>

### Squeeze Net

다음의 두 가지 아이디어를 사용한 네트워크 설계법입니다.

1. 더 큰 Feature Map을 사용하는 것이 성능 향상에 유리합니다. 

   이를 위해 Sub-sampling을 최대한 후반부에 배치하여 Feature Map의 크기를 유지

2. 너무 많은 채널은 Convolution 연산의 비용을 증가시킵니다.

   Point-wise Convolution을 사용해 채널 수를 줄여줍니다.

   일부 3x3 Convolution Layer를 Point-wise Convolution으로 대체하여 계산 비용을 감소 시킴

두 아이디어를 차용해 만들어진 것이 Fire Module이며, 이를 통해 계산 비용은 감소시키면서 네트워크의 depth를 유지해 성능을 유지했습니다..

> Fire Module

<center><img src="https://user-images.githubusercontent.com/31475037/63752110-a0abe380-c8eb-11e9-8a02-61d41979c96a.PNG"></center>
<br>

**참고 강의**

[blog.yani.io](https://blog.yani.io/filter-group-tutorial/)

[slideshare](https://www.google.com/search?q=group+convolution&oq=group+convolution&aqs=chrome..69i57j0j69i59j0j69i60l2.4007j0j7&sourceid=chrome&ie=UTF-8)

[PR-34](https://www.youtube.com/watch?v=V0dLhyg5_Dw&t=139s)

<br>