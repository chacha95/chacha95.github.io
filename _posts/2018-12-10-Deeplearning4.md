---
layout: post
title: Activation Function
tags: [deeplearning, machine learning]
---

## Activation Function

### Activation Function

Neural Network의 출력을 결정하는 식입니다.

각 뉴런의 입력이 모델의 예측과 관련되어 있는지 여부에 따라 활성화 됩니다. 이런 활성화를 통해 신경망은 입력값에서 필요한 정보를 학습합니다.

activation function은 대게 non-linear function을 씁니다. 활성화 함수로 linear function을 쓰지 않는 이유는 다음과 같습니다.

선형함수인 h(x) = cx를 활성화 함수로 사용한 3층 네트워크를 생각해보세요. 이를 식으로 나타내면 y(x) = h(h(x))가 됩니다. 이는 실은 y(x) = ax와 똑같은 식입니다. a = c^3이라고만 하면 끝이죠. 즉, hidden layer가 없는 네트워크로 표현할 수 있습니다. 

또한 선형함수의 미분값은 상수값이기에 입력값과 상관 없는 결과를 가집니다

### Sigmoid

로지스틱 함수로도 불립니다. 

시그모이드 특징

- 함수의 결과 값이 [0, 1] 범위
- 중간값은 0.5
- 매우 큰 값을 가지면 함수값은 거의 1이며 매우 작은 값을 가지면 거의 0이다

단점

- Gradient vanishing 미분함수에 대해 x=0에서 최댓값 0.25를 가지고, input값이 일정이상 올라가면 이분값이 거의 0에 수렴하게 된다. 이는 |x|값이 커질 수록 gradient Backpropagation시 미분 값이 소실 될 가능성이 크다. (Saturated neurons이 gradient를 죽임)

---- 예시 이미지 ----

- zero-centered 되있지 않다 : 만약 모든 x값들이 같은 부호라고 가정할 시, 한 노드에 대해 모든 파라미터 w의 미분값은 모두 같은 부호를 같게된다. 따라서 같은 방향으로 update 되는데 이러한 과정은 학습을 zigzag 형태로 만들어 느리게 만드는 원인이 된다.

----옛이미지

- exp 함수 사용시 계산 비용이 크다

<br>

### Tanh

zero-centered 되있고, 함수의 결과값이 [-1, 1]임

하지만 미분 함수에 대해 일정 값 이사 커질 시 미분값이 소실되는 gradient vanishing 문제는 여진히 남아 있다.

<br>

### ReLU

최근 가장 많이 사용되는 활성화 함수이다.

계산 효율이 좋다.

x > 0 이면 기울기가 1인 직선이고 , x< 0이면 함수값이 0이 된다.

sigmoid, tanh 보다 학습이 더 잘 된다.

x < 0 인 값들에 대해서는 기울기가 0이기 때문에 gradient가 죽어버리는 일이 발생하기도 한다.

----- 예시 이미지

<br>

### Leaky ReLU

Relu에서 x< 0 에서 gradient vanishing 문제를 해결하기 위해서 만들어 졌습니다.

<br>

### PReLU

새로운 파라미터 a를 추가혀여 x < 0 에ㅓ 기울기를 학습할 수 있게 한다.



<br>

### Max Out

연결된 두개의 뉴런 값 중 큰 값을 취한다. ReLU를 일반화 시켜 w1, w2, b1, b2가 학습이 되도록 파라미터를 추가합니다.

결과값이 saturate되지 않음

<br>

### Swish

시그모이드 함수에 입력값을 곱한 함수입니다

2차원에서 확인핼ㅆ을 때, linear 또는 ReLU보다 부드러운 형태를 가집니다.



<br>

### 사용 가이드라인

- 우선 가장 많이 사용되는 함수는 ReLU
- sigmoid의 경우에는 사용하는것을 추천하지 않음
- Sigmoid는 쓰지 말라고 권장하지만 LSTM 같은 모델에서 씀
- tanh의 경우에도 좋은 성능 보장 힘듬

<br>

