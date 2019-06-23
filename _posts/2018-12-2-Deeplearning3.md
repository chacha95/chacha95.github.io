---
layout: post
title: CNN(Convolutional Neural Network)
tags: [deeplearning]
use_math: true
---

## CNN

컴퓨터에서는 이미지는 R, G, B 3차원의 array(행렬)로 표현됩니다. 각 pixels는 0(black)-255(white)사이의 한 값으로 표현됩니다.

아래 고양이 사진에서 height가 400 pixels, width가 248 pixels이고 R, G, B 3 channel을 가진 3차원 행렬로 표현됩니다.

> 행렬로 표현된 고양이

<center><img src="http://cs231n.github.io/assets/classify.png" width="90%"></center>

> 컴퓨터가 이미지에 대해 인식을 잘 못하는 경우

이러한 이미지를 컴퓨터가 인식을 하기 어려운 경우는 다음의 예시들이 대표적입니다.

- Viewpoint variation 

  같은 물체 일지라도 다른 각도에서 찍을 시 인식이 어려움

  ![](https://user-images.githubusercontent.com/31475037/59824788-22701580-936d-11e9-92d9-ed4f3eefbd2d.png)

- Deformation

  물체가 변형되어 있으면 인식이 어려움
  
  ![](https://user-images.githubusercontent.com/31475037/59824759-07050a80-936d-11e9-8235-0a0c2f544789.png)

- Illumination conditions

  조명에 따라 인식이 달라짐

  ![](https://user-images.githubusercontent.com/31475037/59824758-07050a80-936d-11e9-9de5-0eb1a7fd3027.png)

- Occlusion

  다른 물체에 가려져 알고자 하는 물체를 인식하기 어려움

  ![](https://user-images.githubusercontent.com/31475037/59824760-07050a80-936d-11e9-8585-f984d0a7ab94.png)

- Background Clutter

  배경과 물체가 비슷해서 구분이 어려워 인식하기 어려움

  ![](https://user-images.githubusercontent.com/31475037/59824761-07050a80-936d-11e9-8179-8393f0568ba5.png)

- Intra-class variation

  같은 클래스 내에서도 여러 모양이나 색이 나올 수 있음
  
  ![](https://user-images.githubusercontent.com/31475037/59824762-079da100-936d-11e9-95cd-f4d3cfd84112.png)

<br>

### CNN

> CNN의 기원

David H. Hubel과 Torsten Wiesel은 1958년과 1959년에 시각 피질의 구조에 대한 결정적인 통찰을 제공한 고양이 실험을 했습니다. 이들은 시각 피질 안의 많은 뉴런이 작은 local receptive field(국부 수용영역)을 가진다는 것을 보였으며, 이것은 뉴런들이 시야의 일부 범위 안에 있는 시각 자극에만 반응을 한다는 의미입니다. 뉴런의 receptive field는 서로 겹칠 수 있으며, 이렇게 겹쳐진 receptive field들이 전체 시야를 이룹니다. 추가적으로 어떤 뉴런은 수직선의 이미지에만 반응하고 다른 뉴런은 다른 각도의 선에 반응하는 뉴런이 있을 뿐만 아니라, 어떤 뉴런은 큰 수용영역을 가져 저수준의 패턴(edge 등)이 조합되어 복잡한 패턴(texture, object...)에 반응한다는 것을 알게되었습니다. 이러한 관찰을 통해 고 수준의 뉴런이 이웃한 저수준의 뉴런의 출력에 기반한다는 아이디어를 생각해 냈습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile22.uf.tistory.com%2Fimage%2F99F879415BC97DDD0B0E68)

> 초기의 CNN 모델


<center><img src="https://user-images.githubusercontent.com/31475037/59593100-4f81b580-912c-11e9-8c46-4d796dda46b6.png"></center>

Convolution Network의 장점은 다음과 같습니다

- Locality : 인접 신호의 correlation 관계를 비선형적으로 추출함
- Sub sampling : feature 수를 줄이고 maxpooling 방식을 사용해 가장 강한 신호만 전달함

**Convloution Filter**

Convolution Filter란 무엇일까? 이제부터 Convolution Filter를 Conv로 부르겠습니다.

Conv는 영상으로부터 feature를 추출하기 위한 필터로 다음과 같은 특징을 가집니다

- Locality : 인접 신호의 correlation 관계를 비선형적으로 추출
- shared weight : 동일한 계수의 weight filter(총 파라미터 수를 줄임)

이러한 특징에 대해 알아보기 위해 Conv의 작동원리에 대해 알아볼까요?

> 그림으로 표현된 Conv의 작동 원리

여기 height가 32, width가 32, channel(depth)이 3인 이미지가 있다고 합시다.

일반적으로 컬러 이미지는 R, G, B 3채널로 표현됩니다.

이 이미지에 height가 5 width가 5 channel이 3인 Conv를 적용을 시켜 볼까요?

(이 이미지는 32x32x3의 shape를 가진다고 불립니다.)

<center><img src="https://user-images.githubusercontent.com/31475037/59593102-501a4c00-912c-11e9-9153-f22ad1b84812.png"></center>

Conv 하나를 적용하게 되면 하나의 Conv에 있는 75개의 파라미터(5x5x3)와 이미지가 dot product 연산을 통해 하나의 값을 추출해냅니다. 여기서 사용자의 설정에 따라 bias를 추가하게 되면 76개 추가하지 않으면 75개입니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593104-501a4c00-912c-11e9-9818-f7fbdeca80fb.png"></center>

이런 일련의 과정을 통해 하나의 Conv가 하나의 feature map(activation map)을 만들어 냅니다

<center><img src=""></center>

<center><img src="https://user-images.githubusercontent.com/31475037/59593106-501a4c00-912c-11e9-9fbd-32e433954b46.png"></center>

그 다음 Conv를 적용시켜 또 다른 feature map을 만듭니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593107-50b2e280-912c-11e9-9f25-4ac95d5f5323.png"></center>

이런 Conv 필터 6개를 사용해 총 6개의 feature map을 추출해내고, 추출된 feature map들은 channel 단위로 묶어서 하나의 새로운 이미지 처럼 취급됩니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593108-50b2e280-912c-11e9-8619-798ff2464b9b.png"></center>

즉, 32x32x3의 shape를 가진 이미지가 6개의 Conv를 거쳐 28x28x6의 shape를 가진 이미지로 바뀌게 됩니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593110-50b2e280-912c-11e9-8cad-6446db0d0311.png"></center>



> Conv layer가 깊어질 수록 high level feature를 추출해 나간다.

이런 Conv 연산을 하는 layer를 여러번 거침에 따라 점점더 고차원의 feature들을 추출해 낼수 있습니다.

앞 부분에 있는 layer들은 선과 같이 저차원의 feature를 뒤 부분에 있는 layer들은 texture같은 고차원의 feature를 추출해 냅니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593112-514b7900-912c-11e9-9959-b4e503dce792.png"></center>



> Conv layer와 image size

아래는 7x7 이미지에 filter size가 3x3, stride가 1인 Conv를 적용하는 모습입니다.

여기서 stride는 Conv가 다음 영역으로 이동할 때 얼마나 움직일지 지정해주는 값입니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593114-514b7900-912c-11e9-8484-60a51f3d54de.png"></center>

아래는 7x7 이미지에 filter size가 3x3, stride가 1인 Conv를 적용하는 모습입니다.

여기서 stride는 Conv가 다음 영역으로 이동할 때 얼마나 움직일지 지정해주는 값입니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593115-51e40f80-912c-11e9-86b4-572dff751896.png"></center>

다음 공식을 통해 Conv 연산자를 통해 나오는 이미지의 Width와 Height를 알 수 있습니다.(일반적으로 넣어주는 이미지는 Height와 Width를 같게 고정시킵니다.)

<center><img src="https://user-images.githubusercontent.com/31475037/59593117-51e40f80-912c-11e9-8df1-592bcfa838d2.png"></center>

Conv와 이미지의 크기가 제대로 일치하지 않는다면 연산이 제대로 되지 않겠죠. 그래서 나온 방식이 padding을 통해 크기를 맞춰주는 방식입니다.

> 이미지 padding

이미지 padding 기법은 영상처리에서 자주 쓰였던 기법으로 이미지 최 외곽을 특정값(0이 될 수도 있고, 근처 픽셀 값이 될수 도 있습니다)으로 채워주는 기법입니다.

이를 통해 Conv가 제대로 동작하도록 돕습니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593119-51e40f80-912c-11e9-8684-fa85e6d28767.png"></center>

> 실제 3 Channel image에 CNN이 적용되는 모습

실제 Conv가 적용되는 모습은 다음과 같습니다.

Conv를 일종의 직사각형 모양의 큐브 블럭이 이미지에 적용된다고 보시면 편합니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593125-527ca600-912c-11e9-86dd-cf72042afc41.png"></center>



**1x1 Conv**

1x1 Conv란 다음과 같습니다. Conv의 Filter 크기가 1인 Conv로 입력의 필터 크기 보다 Conv의 필터 수가 작을 때 Dimensionality Reduction 효과가 있습니다.

이런 기법을 통해 네트워크의 연산량을 적게 하면서도 네트워크를 깊게 쌓을 수 있도록 도와줍니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593122-527ca600-912c-11e9-9668-d8ca2e3e586f.png"></center>

**Pooling**

pooling 기법은 영상의 width와 hight를 절반으로 줄여주며, 가장 강한 신호만 추출하거나, 혹은 신호의 평균값을 전달해주는 역할을 합니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593123-527ca600-912c-11e9-8f52-1a457a02e16a.png"></center>

> Max Pooling

다음은 Max Pooling의 예시로 2x2 filter size를 가진 max pooling 연산자가 feature를 추출하는 모습입니다. filter 사이즈 내에서 가장 높은 값만 추출해 냅니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59593124-527ca600-912c-11e9-9663-8af75fdff026.png"></center>

<br>

### CNN parameter vs fc parameter

입력이 36x36x1의 shape를 가진 이미지가 있다 했을 때 CNN에서의 parameter 수와 FC(Fully Cunnected)만을 사용한 네트워크에서의 paramter 수를 비교해 봅시다.

![](https://user-images.githubusercontent.com/31475037/59670831-8c66ae80-91f7-11e9-8465-76a0100ec34d.png)

![](https://user-images.githubusercontent.com/31475037/59670832-8c66ae80-91f7-11e9-9615-9a55adcb6bfd.png)

위의 그림과 같이 parameter 수가 5배 정도 차이가 납니다.

<br>

### Neuron 관점에서의 Conv Layer

Fully Cunnected Layer에서는 dot product가 이루어지는 각 노드가 각 뉴런입니다.

그렇다면 Conv Layer에서는 어떤가요?

바로 Conv Layer에서는 각 필터가 각 뉴런을 의미합니다.

> 5개의 필터를 이용한 Conv

다음과 같이 입력에 대해 Conv를 5개를 썼다면, 뉴런이 5개 있다는 것과 동일한 의미입니다. 

![](https://user-images.githubusercontent.com/31475037/59965152-45552200-9545-11e9-9547-a12d8a66aef2.png)

<br>

### Abstraction

CNN이 학습을 통해 Classification 문제를 해결하는 모습은 다음과 같이 요약되어 나타낼 수 있습니다.

![](https://scontent-icn1-1.xx.fbcdn.net/v/t1.0-9/61902071_328557711158298_181285961064251392_n.png?_nc_cat=103&_nc_eui2=AeGBO-zPUE4Xi9_jur050hRO_8ncltL-ANNTP3PQhMLUEIgQzWx1izLeAaO8iZsqBIEJsiUaJ3n1frTqF9sU1kCEGWREVK4AOyAuo9hHyg6I6Q&_nc_oc=AQkp-QVCjS1hi8Gw1q72utZQMM9e7jk8dxIjzNnez_bfcqGxSt80OeRpAfCcW7cZqp8&_nc_ht=scontent-icn1-1.xx&oh=3978f86aafc4c62efdc1fee38c1c1668&oe=5D877262)



<br>

**읽어볼만한 글**

[CNN](https://excelsior-cjh.tistory.com/180)

**참고 강의**

[CS231n](https://www.youtube.com/playlist?list=PL3FW7Lu3i5JvHM8ljYj-zLfQRF3EO8sYv)

<br>

