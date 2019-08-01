---
layout: post
title: Auto Encoder
tags: [Deeplearning]
use_math: true
---

## Auto Encoder

오토 인코더는 머신러닝 기법의 일종입니다. 차원 축소를 위해 비 지도학습의 형태로 학습하는 신경망 기법(Neural Network)입니다. 오토 인코더는 단순히 입력을 출력으로 복사하는 신경망입니다. 이는 비 지도 학습을 지도학습처럼 훈련시키는 방식입니다. 그림 9 의 모습처럼 bottleneck(병목) 구조를 띄고 있어 중간 부분의 hidden layer 에서 latent vector z 를 만들어 내는 구조를 띄고 있으며, 이 latent vector z 가 입력 벡터 x 에 대한 차원 감소된 값 입니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59673542-74ddf480-91fc-11e9-896a-d4a1d4ea5451.png"></center>
<br>

### 이론적 배경

**Discriminant Model**

머신러닝 중 지도학습은 대부분 Discriminant Model 입니다. 즉 구별하는 Model 이란 뜻으로 다음에 대해서 알아내는 것입니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59673544-74ddf480-91fc-11e9-847f-15ccef935abf.png" width="30%"></center>
어떠한 입력 x 벡터가 들어갔을 경우의 정답(클래스 혹은 레이블)인 z 의 확률을 의미합니다.

그렇다면 확률은 무엇인가요? 

확률은 단순하게 생각해서 ‘판단의 근거’가 됩니다. 도박을 생각하면 쉽습니다. 

그렇다면 확률 분포란?

 어떠한 파라미터(Gaussian distribution 의 경우 평균과 분산)에 따라서 확률이 변하는 일종의 “함수”입니다. 

그렇다면 결국 확률 분포를 추정하는 것은 확률을 추정하는 것이 되고, 이것이 판단의 근거가 됩니다.

 즉 Discriminant Model 의 경우에는 p(z|x)를 추정함으로서 주어진 입력으로부터 정답의 판단 근거를 삼겠다는 것입니다.

 즉 입력 x 가 많아질수록 p(z|x)를(최대로) 추정할 수 있게 하는 파라미터를 구할 수가 있게 된다는 것이며, 이를 Maximum Likelihood Estimation(MLE)이라고 합니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59673545-74ddf480-91fc-11e9-81e9-8f633caee744.png" width="50%"></center>
여기서 f 란 Probability Function 인데 확률을 의미합니다.

이 식을 머신러닝 관점에서 본다면? 

Discriminant Model  대표적으로 CNN 을 예를 들면,
Convolution 등을 통해서 어떠한 Feature 가 입력으로부터 더 잘 정답을 구분 하기 위해 feature 로 학습되는지를 배우고 그에 따라 구분 하겠다는 것입니다.

 즉 feature learning 이 목적이고 그 목적은 구분입니다. 개와 고양이를 가르는 문제라고 치면 개와 고양이의 차이점을 중점적으로 본다는 의미입니다.



**Generative Model**

Generative Model 이란 생성 모델이란 의미로, 데이터를 생성해내는 model 이라는 의미입니다. 입력 데이터를 생성해 내기 위해서 결국 필요한 것은 다음과 같은 joint probability 를 알아 내야 합니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59673546-75768b00-91fc-11e9-88bf-69c22ab787db.png" width="50%"></center>
그러려면 입력 데이터의 확률 분포인 p(x)를 알아야 합니다. 

결국에는 p(x)를 알아내는 것이 Generative Model 의 최종 목표입니다. 

그런데 이 문제에서는 분류 문제도 아닌데 z 는 뭘까요? 

정답이 없기에 결국 z 의 의미 부여는 우리가 해야 합니다.

 Generative Model 에서는 Hidden Parameters 혹은 latent vector 라고 부릅니다.
머신러닝의 관점에서 본다면, 결국 바로 p(x)를 알아 낼 수가 없으니, Generative Model 의 학습은 z 와 그것으로부터 데이터를 생성해 내는 p(x|z)의 분포도 학습한다고 볼 수 있습니다. 

그리고 z 를 학습하기 위해서 p(x|z)도 학습하는 방법이 존재하고(Auto Encoder 계열) 아니면 간접적으로 p(x)를 학습하게하는 방법(GAN 계열)도 존재합니다.

결국 핵심은 p(x|z)를 학습하고 z 는 단순하게 가정하는 것입니다.
Z 를 입력의 차원 감소된 중요한 feature 로 본다면, z 를 변화 시켰을 때 “말도 안되는” 결과물 이미지가 아니라 사람이 납득 할 수 있게 자연스럽게 변하는가 혹은 z 를 변화시켰을 때 의미론적인 변화가 일어나는가에 대해 알아야 합니다.

**오토 인코더 특징**

오토 인코더는 encoder 와 decoder 로 구성되어 있습니다.
Encoder 는 인풋 데이터로부터 차원감소를 통해 z 벡터(latent vector)를 생성해내는 역할을 하며, Decoder 는 Generative Network 라고도 하며, z 벡터를 다시 입력 데이터와 똑같이 되돌려놓습니다. 
오토 인코더는 입력을 차원감소를 통해 축소시켰다가 다시 복원시키는 특성을 지니기에 오토 인코더 학습시 Loss function 은 입력과 출력의 차이(L1, MSE 등등..)를 가지고 계산합니다.

<br>

### VAE(Variational Auto Encoder)

VAE 는 오토 인코더의 방식이 Generative Model 에는 맞지 않는 다는 생각에서 시작되었습니다. 오토 인코더가 입력 벡터를 따라 그리는 것에만 맞게 학습되며, latent vector 가 의미론적이지 않다는 것입니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59673547-75768b00-91fc-11e9-9c35-6dbdbf6bd3df.png" width="50%"></center>
VAE 에서는 각각을 다음과 같이 정의합니다
- p(z) : latent vector 의 분포로 Gaussian 으로 가정
- p(z|x) : Encoder 가 어떻게 입력 데이터를 잠재 변수로 표현할 것인가?
- P(x|z) : Decoder 가 어떻게 latent vector 로부터 데이터를 복원할 것인가?

이는 오토 인코더처럼 학습하되 p(z)의 분포를 고려하겠다는 의미입니다. 

이것은 우리의 최종목표인 p(x)로부터 시작됩니다. 하나 알고 갈 것은 q 의 등장인데, q 는 해당 확률 분포를 추정하는 확률분포입니다. 

p(x)를 구하고자 하는 것은 아래 수식으로부터

<center><img src="https://user-images.githubusercontent.com/31475037/59673548-75768b00-91fc-11e9-8dd5-dcd200c9637e.png" width="50%"></center>
Expectation 할 수 있는 것은, p(x)가 z 와 독립적이기 때문입니다. Bayes 정리를 적용하면

<center><img src="https://user-images.githubusercontent.com/31475037/59673549-75768b00-91fc-11e9-8c8a-2ee225970048.png" width="70%"></center>
똑같은 값을 분모, 분자에 곱해주면

<center><img src="https://user-images.githubusercontent.com/31475037/59673550-760f2180-91fc-11e9-8df5-22b0db7d3418.png" width="70%"></center>
로그를 분리해 보면,

<center><img src="https://user-images.githubusercontent.com/31475037/59673552-760f2180-91fc-11e9-994c-7f0aed03eddd.png"></center>
KL-Divergence 의 정의에 따라 정리하면,

<center><img src=""></center>
<center><img src="https://user-images.githubusercontent.com/31475037/59673553-760f2180-91fc-11e9-8505-c0cda4c8831b.png"></center>
이 3 가지 term 을 하나씩 살펴봅시다.
첫 번째 term 은 기댓값으로서, q(z|x)로부터 z 를 Sampling 해서 그것을 기반으로 한 조건부 확률 p(x|z)의 Log-likelihood 입니다. 

즉 Encoding-Decoding 으로 본다면 입력 데이터와의 차이인 복원 Loss 에 해당됩니다. 위 식을 최대화한다는 것은, Loss 를 최소화한다는 것과 일치합니다. 

이 term 을 Monte Carlo Estimation 을 사용하면 다음과 같습니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59673554-760f2180-91fc-11e9-9c08-7652f2832074.png"></center>
Monte Carlo Estimation 은 간단하게 말해서 많은 Sample 로서 전체 확률 분포를 근사 하겠다는 의미입니다.

 이렇게 변환하면 각 Sample 당 Loss 의 평균이 됩니다. MSE 가 대표적인 예입니다. 

두번째 term 은 우리가 근사하려는 q(z|x) 확률 분포와 z 분포의 유사성입니다. 

우리가 근사하는 확률 분포는 z 와 비슷해야 한다는 것입니다. 이 식 앞에 마이너스가 붙어 있는 것을 최대화해야 함으로, KL-Divergence term 을 최소화한다는 것과 같습니다. 

그런데 z 는 Gaussian 이기 때문에 closed-form 으로 표현할 수 있습니다. Closed-form 의 해답을 구해보면 z~N(0,1)이므로 q 분포도 정규분포라 가정하면 다음 수식과 같이 전개됩니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59673558-76a7b800-91fc-11e9-9a7a-7de23424eadb.png"></center>
평균과 분산을 Encoder 의 결과로 생각하면 됩니다. 즉, Encoder 는 마지막 Layer 가 평균과 분산을 결과로 만들도록 학습하는 방식입니다..
마지막 term 은 우리가 p(x)를 모르기에 p(z|x)를 제대로 구할 수가 없습니다. 

하지만 KL-Divergence 의 특징인 KL-Divergence 의 값이 0 이상이라는 조건이기에 이 term 때문에 전체 값이 음수가 될 일은 없기에 이 term 을 제외한 앞의 2 개의 term 을 최대화하면 됩니다.

결국 최종 Loss 는 다음과 같이 됩니다. 앞의 term 이 복원 오차, 뒷 term 이 regularizer 가 됩니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59673559-76a7b800-91fc-11e9-8f8a-d78a2c22b361.png"></center>
학습 과정을 정리해보면

1. Loss 의 첫 번째 항 :
   Input -> Encoding -> 평균과 분산, 정규분포 -> Sampling z -> Decoding -> 복원된 것과 input 의 오차
2. Loss 의 두 번째 항:
   Input -> Encoding -> 평균과 분산이 얼마나 N(0, 1)에 가까운가?
   하지만 Sampling Operation 은 미분이 불가능해서 학습 과정 중 Backpropagation 이 불가능 합니다.
   VAE 에서는 Reparameterization Trick 이라는 것을 사용하며, 이것은 Encoder Network Output 인 평균과 분산의 정규 분포로부터 Sampling 하는 것이 아니라 Sampling 은 N(0, 1)으로부터 하고 다음을 이용해서 Decoder Input 을 만들어 미분 가능하게 만드는 것입니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59673560-76a7b800-91fc-11e9-9e61-bbb6c9022906.png"></center>
결국 오토 인코더와의 차이점은 z 의 분포를 알아내는 것이 Regularizer 역할을 수행해서, 원본을 그대로 답습하는 것에 그치지 않고 어느 정도 의미론적인 Generation 을 수행 합니다. 하지만 역설적이게도 Regularizer 밖에 안되기에 결과물에 Blur 가 많습니다. 하지만 결과 이미지의 형체가 확실하게 생성됩니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59673562-76a7b800-91fc-11e9-9562-bc1b58436c88.png"></center>
