---
layout: post
title: Activation Function
tags: [deeplearning]
use_math: true
---

## Activation Function

이전 layer의 모든 입력에 대한 가중 합을 취하고 출력 값(일반적으로 비선형)을 생성하여 다음 레이어로 전달하는 함수입니다.

각 뉴런의 입력이 모델의 예측과 관련되어 있는지 여부에 따라 활성화 됩니다. 이런 활성화를 통해 신경망은 입력값에서 필요한 정보를 학습합니다.

activation function은 대게 non-linear function을 씁니다. 활성화 함수로 linear function을 쓰지 않는 이유는 다음과 같습니다.

선형함수인 h(x) = cx를 활성화 함수로 사용한 3층 네트워크를 생각해보세요. 이를 식으로 나타내면 y(x) = h(h(x))가 됩니다. 이는 실은 y(x) = ax와 똑같은 식입니다. a = c^3이라고만 하면 끝이죠. 즉, hidden layer가 없는 네트워크로 표현할 수 있습니다. 

또한 선형함수의 미분값은 상수값이기에 입력값과 상관 없는 결과를 가집니다.

### Sigmoid

로지스틱 함수로도 불립니다. 

<center><img src="https://user-images.githubusercontent.com/31475037/59671599-ec118980-91f8-11e9-9e1c-51d1b6fc101e.png" width="50%"></center>

**시그모이드 특징**

- 함수의 결과 값이 [0, 1] 범위
- 중간값은 0.5
- 매우 큰 값을 가지면 함수값은 거의 1이며 매우 작은 값을 가지면 거의 0이다



**문제점**

- <span style="color:red">Gradient vanishing</span> 

  미분함수에 대해 x=0에서 최댓값 0.25를 가지고, input값이 일정이상 올라가면 이분값이 거의 0에 수렴하게 되며, 이는 |x|값이 커질 수록 gradient Backpropagation시 미분 값이 소실 될 가능성이 큼 (Saturated neurons이 gradient를 죽임)

- <span style="color:red">zero-centered 되있지 않음</span>

  만약 모든 x값들이 같은 부호라고 가정할 시, 한 노드에 대해 모든 파라미터 w의 미분값은 모두 같은 부호를 같게된다. 따라서 같은 방향으로 update 되는데 이러한 과정은 학습을 zigzag 형태로 만들어 느리게 만드는 원인이 됨

- <span style="color:red">exp 함수 사용시 계산 비용이 큼</span>



> Saturate 영역에 있을시 Gradient가 죽는다

<center><img src="https://user-images.githubusercontent.com/31475037/59671596-eb78f300-91f8-11e9-9e55-1ac74fad0eac.png" width="80%"></center>

> x=-10, x=0, x=10 에서 gradient가 어떻게 되는지 생각해보자!

<center><img src="https://user-images.githubusercontent.com/31475037/59671600-ec118980-91f8-11e9-9c33-3f2bdb64e67f.png"></center>

> zero-centerd 되있지 않아서 weight 업데이트가 느리다.

이상적으로 weight가 업데이트 된다면 아래 그림의 파란색 선과 같이 업데이트 되야 하나, zero-centerd 되 있지 않으면, weight가 빨간색과 같이 zig-zag로 업데이트됩니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59671601-ec118980-91f8-11e9-90d3-acf6eed9bbce.png"></center>

<br>

### Tanh

zero-centered 되있고, 함수의 결과값이 [-1, 1]임

하지만 미분 함수에 대해 일정 값 이사 커질 시 미분값이 소실되는 gradient vanishing 문제는 여진히 남아 있다.

<center><img src="https://user-images.githubusercontent.com/31475037/59671602-ec118980-91f8-11e9-9236-0316f2d4a6b6.png"></center>

<br>

### ReLU

최근 가장 많이 사용되는 활성화 함수입니다.

다음과 같은 특징을 가집니다.

- 계산 효율이 좋음

- x > 0 이면 기울기가 1인 직선이고 , x< 0이면 함수값이 0이 됨

- sigmoid, tanh 보다 학습이 더 잘 됨(gradient vanishing문제가 덜 일어남)

- x < 0 인 값들에 대해서는 기울기가 0이기 때문에 gradient가 죽어버리는 일이 발생하기도 함

<center><img src="https://user-images.githubusercontent.com/31475037/59671604-ecaa2000-91f8-11e9-9fe5-bb724fca7688.png" ></center>

> Sigmoid나 Tanh 보다 Gradient가 죽는 일은 많이 줄었으나 x < 0인 구간에서 gradient가 죽음

<center><img src="https://user-images.githubusercontent.com/31475037/59671605-ecaa2000-91f8-11e9-8b69-6c46b8ab6c73.png"></center>

<br>

### Leaky ReLU

Relu에서 x< 0 에서 gradient vanishing 문제를 해결하기 위해서 만들어 졌습니다.

x < 0인 부분에서도 약간의 값을 줘 gradient vanishing 문제를 해결하였습니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59671608-ed42b680-91f8-11e9-8463-d4b0aa56427c.png"></center>

<br>

### PReLU

새로운 파라미터 a를 추가혀여 x < 0 에서 기울기를 학습할 수 있게 만들었습니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59671609-eddb4d00-91f8-11e9-9533-2d5170d59db8.png"></center>



<br>

### Max Out

연결된 두개의 뉴런 값 중 큰 값을 취하는 방식입니다. ReLU를 일반화 시켜 w1, w2, b1, b2가 학습이 되도록 파라미터를 추가합니다.

계산량을 많이 먹어 자주는 쓰이지 않는 방식입니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59671610-eddb4d00-91f8-11e9-909d-376a763c87eb.png"></center>

<center><img src=""></center>

<br>

### 사용 가이드라인

- 우선 가장 많이 사용되는 함수는 ReLU
- sigmoid의 경우에는 사용하는것을 추천하지 않음
- Sigmoid는 쓰지 말라고 권장하지만 LSTM 같은 모델에서 씀
- tanh의 경우에도 좋은 성능 보장 힘듬

<br>

**참고 강의**

[CS231n](https://www.youtube.com/playlist?list=PL3FW7Lu3i5JvHM8ljYj-zLfQRF3EO8sYv)

<br>



