---
layout: post
title: Bias and Variance
tags: [machine learning, deeplearning, statistic]
---

## Bias and Variance trade-off

### bias variance trade-off?

머신러닝의 학습에 쓰이는 error(loss) 함수는 다음과 같이 분리 될 수 있습니다.

**Error(X) = noise(X) + bias(X) + variance(X)**

-- 수식 추가 ---

(noise는 데이터가 가지는 본질적인 한계치이기 때문에 irreducible error라 불리며, bias/variance는 모델에 따라 변하는 것이기에 reducible error라 불립니다.)

여기서 error에 큰 영향을 미치는 bias와 variance의 상관관계에 대해 알아봅시다.

bias variance trade-off란 bias와 variance 둘다 target에 맞추고 싶지만, 양립할 수 없는 성질입니다.

bias variance trade-off는 지도학습(supervised learning)에서의 error 핸들링에서 가장 중요한 부분 중 하나입니다.

**bias**는 잘못된 추정이나 overly-simplistic한 추정에의해 생기는 error 입니다. bias에 의해 학습 과정 중에 데이터에 대해 under-fitting하는 결과물을 만듭니다. high bias는 우리의 학습 알고리즘이 feature를 제대로 학습하지 못한다는 것을 의미합니다. 추정값의 평균과 참 값들간의 차이 -> 참 값과 추정값의 거리를 의미

**variance**는 overly-complex하게 training 데이터에 대해 fitting하려 하는 것을 의미합니다. high variance에서 모델은 training data에 대해 over-fitting하는 경향이 있습니다. 즉 데이터 내에 있는 에러나 노이즈까지 잘 잡아내는 highly flexible models에 데이터를 fitting시킴으로서, 실제 현상과는 관계없는 random한 것들까지 학습하는 경향이 있습니다. 추정값의 평균과 추정 값들간의 차이에 대한 것 -> 추정 값들의 흩어진 정도

bias는 트레이닝 데이터를 바꿈에 따라서 알고리즘의 평균 정확도가 얼마나 많이 변하는지 보여주고, variance는 특정 입력 데이터에 대해 알고리즘이 얼마나 민감한지를 나타냅니다. variance는 loss를 의미 하지만, 참 값과는 관계없이 추정값들의 흩어진 정도만을 의미합니다.

> fitting

이상적인 모델은 트레이닝 데이터에서 반복되는 규칙성을 정확하게 잡아내면서도 학습되지 않은(unseen) 데이터를 잘 일반화 할 수 있는 모델입니다. 위 문장의 의미를 이해하기 위해, 같은 데이터에 다른 모델을 이용해 학습시킨 아래의 그림을 봅시다.

------- 그래프 삽입 ----

**선형 모델(degree=1)은 under-fit이다**

이 모델은 데이터 내의 모든 정보를 고려하지 못하고 있습니다.(high bias) 새로운 데이터가 들어온다 하더라도 이 모델의 형태는 크게 변하지 않을 것입니다.(low variance)

**반면에 고차 다항함수 모델(degree=20)은 over-fit이다**

이 곡선 모델은 주어진 데이터를 잘 설명하고 있다.(low bias) 이 함수는 새로운 데이터가 들어 왔을 때 완전히 다른 형태로 벼하게 되고, generality를 잃게 될 것이다.(high variance)

**이상적인 모델은 데이터의 규칙성을 잘 잡아내어 정확하면서도 다른 데이터가 들어왔을 때도 잘 일반화 할 수 있는 모델일 것입니다(degree=3)**

이처럼 train error를 줄이기 위해 모델의 복잡도(degree)를  계속 높이면 overfitting이 일어나게 됩니다. 즉 모델 복잡도가 올라갈 수록 bias는 감소하나, variance가 증가하는 현상이 일어나게 됩니다.

실제로 두 가지를 동시에 만족하는 것은 불가능하며, 따라서 training data가 아닌 test data(실제 데이터)에서 좋은 성능을 내기 위해 이런 trade-off는 반드시 생길 수 밖에 없으며 이는 bias variance trade-off라 불립니다.

test 과정에서, high variance를 가진 모델은 제대로 prediction을 하지 못합니다. 왜냐하면 training data의 variation의 정도에 너무 민감해져, test단의 prediction에 노이즈가 생겨 잘못된 prediction을 하게 됩니다.

----- simple, comlex 그래프 넣기 -----



>Bias vs Variance의 의미

우리는 학습 모델의 bias, variance의 특징에 대해 설명하는 아래와 같은 그림이 있습니다.

참고로 아래 그림은 bias-variance trade off를 나타내는 그림은 아닙니다.

<center><img src="https://miro.medium.com/max/494/1*0kwrZw8heXmI2Iq5l5oTQQ.png" width="80%"></center>



붉은색 영역은 target(참 값)을 의미하고, 파란 점은 prediction(추정값)을 의미합니다. 여기서 bias는 참 값들과 추정 값들의 차이(평균간의 거리)를 의미하고, variance는 추정 값들의 흩어진 정도를 의미합니다.

bias와 variance가 loss이므로, 우리는 직관적으로 둘 다 작은 (a)모델이 가장 좋은 모델인 것을 알 수 있습니다. (b) 모델은 추정 값들을 평균한 값은 참 값과 비슷한데(bias가 작은데), 추정 값들의 variance가 커서 loss가 큰 모델입니다. (c)는 bias가 크고 variance가 작은 모델이고, (d)는 둘다 큰 모델입니다.



> train data와 test data 관점

train data에서는 (a)와 같은 결과 이었는데(train loss가 작았는데), test data를 넣어보니 (b), (c), (d)의 결과가 나왔다고 해 보겠습니다. 3가지 모두 train data에서는 loss가 작았는데 test data에 적용해 보니 loss가 커졌습니다. 왜 그런 걸까요?

(b), (c), (d) 모두 에러가 크지만 서로 다른 유형의 에러를 나타내고 있습니다. 즉, 원인이 다르다는 것을 의미합니다. variance가 큰 (b)모델은 train data에 over-fitting된 것이 원인이고, 이는 너무 train data에 fitting된 모델을 만들어 test data에 fitting된 모델을 만들어서 test data에서 오차가 발생한다는 것을 의미합니다. bias가 큰 (c) 모델은 test data를 위한 학습이 덜 된 것이 원인이고, 이는 train data와 test data간의 차이가 너무 커서 train data로만 학습한 모델은 test data를 맞출수가 없는 것입니다. 만일 (c) 그림이 train data에 대한 것이라면 train data에 대해 under-fitting 즉, 학습이 덜 된 모델이라고 할 수 있습니다. (d)는 둘 다의 경우로 생각할 수 있습니다.

데이터 관점에서 보면 (b)의 경우 train data와 test data의 차이는 variance의 차이라고 할 수 있고, (c)의 경우 train data와 test data의 차이는 평균의 차이라고 할 수 있습니다. 학습 모델은 입력 X를 Y로 추정하는 것인데, 입력 X의 분포가 바뀌어 버리면 바뀐만큼의 error가 날 수 밖에 없습니다. 

trian data와 test data의 차이가 많이 나면 어떻게 해야 할까요? 즉 test data에 대해 (c)의 경우 어떻게 해야 할까요? 우선적으로는 test data의 일부를 train set에 포함시키는 것입니다. 그러나 이러한 방법은 test data를 오염시키는 방식입니다.(test data set은 어떠한 일이 있더라도 train 과정 중에 쓰이면 안됨) 

그렇기에 이런 현상을 막기 위해 test set을 나눠 validation set으로 만든 뒤 train 과정 중에 사용하는 것입니다. 특히나 train set의 변동성을 낮추기 위해 k-fold cross validation 기법입니다. 이런 기법으로 validataion set과 train set을 여러번 섞어 사용함으로써 여러 data set을 만들어 평균적으로 적용시키는 방법입니다.

<br>

##### 읽어볼만한 글

##### [모두연](https://modulabs-biomedical.github.io/Bias_vs_Variance)

<br>

