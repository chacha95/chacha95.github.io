---
layout: post
title: Training Neural Network
tags: [deeplearning]
---

## Training Neural Network

### Loss 종류

**L1**



**L2(MSE)**



**Cross-Entropy**



### Regularization

**L1 Regularization**



**L2 Regularization**



### Learning Rate Decay



### Data Set

**Data Set 구성**

> 데이터셋 구성

일반적으로 Data Set은 Training/Validation/Testing으로 나뉘어 집니다.

여기서 **Test Set은 절대로 Training에 쓰이면 안됩니다**

Data Set을 이렇게 나누는 이유는 Test Set이 모델의 Training과정 중에 쓰이는 것을 막기 위해 Training 과정이 잘 되고 있는지 아닌지 판단하는 지표가 loss 밖에 없습니다. 하지만 loss가 낮다고 해서 Test Set에서의 결과가 항상 좋은것만은 아닙니다.(그 이유는 아래서 설명)

그렇기 때문에 Training 과정 중에 모델에 대해 평가를 하기 위한 Validation Set을 만드는 것입니다. 



<center><img src="https://t1.daumcdn.net/cfile/tistory/9951E5445AAE1BE025"></center>

**k-fold cross validation**

k-fold cross validiation 기법은 data set의 수가 적을 때 정확도를 향상시키는 방법입니다. 

방법은 다음과 같습니다.

- Training을 k개의 fold로 나눕니다.
- 아래는 5개의 fold로 나누었을 때
- 한개의 fold에 있는 데이터를 다시 k개로 쪼갠 뒤, k-1개는 Train Set, 마지막 한개는 Validation Set을 지정합니다.
- 다음 fold에서는 Validation Set을 바꿔 지정하고, 이전 fold에서 Validation 역할을 했던 Set은 다시 Training Set으로 활용합니다.
- 이를 K번 반복합니다

이러한 방법을 이용하여 적은 Training Set을 크게 만들 수 있다.

<center><img src="https://user-images.githubusercontent.com/31475037/59551217-06592680-8fb1-11e9-9adf-c8a0592101b2.png"></center>



**Bias-Variance Tradeoff와 Training**

[Bias-Variance Tradeoff](<https://chacha95.github.io/2018-12-15-Deeplearning5/>)라는 성질 때문에 계속 학습 시킨다고 해서 loss가 계속해서 줄어드는 것이 아닙니다.

>  그렇다면 언제까지 학습을 해야 하는 것일까?

<center><img src="https://user-images.githubusercontent.com/31475037/59551074-022c0980-8faf-11e9-9466-5f242652d0a6.png"></center>

학습시킨 모델을 Validation에 놓고 조금씩 비교하다보면 초반 loss는 떨어지나, 어느 시점부터 loss가 증가하는 것을 보입니다. 결국 Training을 무조건 지속하기보다는 Validation의에러가 증가하는 시점에 학습을 중단하는 것이 좋습니다.

즉, 모델을 한번 학습 시켰다면 그것을 Validation Set으로 Error가 얼마나 내려가는지 한번 더 확인해 보는 것입니다. Validation Set이 최적의 Error를 보인다면 그 지점이 가장 학습이 잘 된 지점으로 생각하고 학습을 중단 시키면 됩니다.

그리고 Test Set으로 실제 모델을 평가 하면 됩니다. 



### Data Set Preprocessing

Data 전처리로는 

**Mean Subtraction**



**Normalization**



<br>

