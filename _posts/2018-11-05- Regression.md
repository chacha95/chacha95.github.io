---
layout: post
title: Regression
tags: [machine learning]
---

## Regression(회귀)

Regression[^1]이란 주어진 training 데이터로부터 가설을 세워 변수 사이의 모델(모형)을 구하는 예측을 말합니다.

대표적인 예로는 주택값 예측 모델이 있습니다.

> Regression의 간단한 예시 모델

H(x)라는 모형을 설계하는데 training 데이터로 하여금 Weight(W)와 bias(b)를 알맞게 조정하는것이 목표입니다.

bias에 대한 자세한 설명은 [여기](<https://chacha95.github.io/2018-09-02-Bias_Varaince/>)

<center><img src="https://user-images.githubusercontent.com/31475037/59034478-19c80b80-88a6-11e9-9f7e-b409e0348301.png" width="90%"></center>

Weight와 bias를 알맞게 조정하기 위해서 우리는 Cost 함수라는것을 정의 합니다.

Cost 함수(Loss 함수)는 우리의 모델이 잘 조정이 되는지 평가하는 함수입니다.

Cost함수는 우리의 모델이 잘 예측을 하였을 시 값이 줄어들고, 잘못 예측했을시 커지는 특징을 가지고 있습니다.

이러한 특징에 따라 cost함수를 최소화 시키는것이 우리의 목표입니다.

보통 Cost(x, y) = H(x) -y 와 같은 꼴로 표현 됩니다.

여기서 y는 정답 데이터입니다.

우리의 모델은 Cost 함수로부터 계수인 W를 학습합니다.

> Cost 함수를 최소화 하는것이 목적

<center><img src="https://user-images.githubusercontent.com/31475037/59034482-1a60a200-88a6-11e9-84c9-7b8dda913e15.png" width="70%"></center>



> W값을 계속 업데이트 해줌

<center><figure class="second">
	<img src="https://user-images.githubusercontent.com/31475037/59034483-1a60a200-88a6-11e9-88a5-7a5b007cc497.png" width="49%">
	<img src="https://user-images.githubusercontent.com/31475037/59034485-1af93880-88a6-11e9-85d8-9f28aa5239b3.png" width="49%">
</figure></center>


> Cost함수 최적화의 실제 적용


<center><img src="https://user-images.githubusercontent.com/31475037/59034486-1af93880-88a6-11e9-9192-a0a3ff4391ae.png" width="50%"></center>

> w1, w2의 Loss 함수에 따른 업데이트 예시

<center><img src="https://user-images.githubusercontent.com/31475037/59034488-1af93880-88a6-11e9-8c01-5d7972ea642d.png" width="70%"></center>

> Convex, Concave

<center><img src="https://user-images.githubusercontent.com/31475037/59034491-1cc2fc00-88a6-11e9-845b-ac4774396a3c.png"></center>

> Local minima와 Global minima

<center><img src="https://user-images.githubusercontent.com/31475037/59034492-1cc2fc00-88a6-11e9-9a3b-af150a4460e7.png"></center>

### Multi Variable

> 입력 데이터가 Multi Variable일때는?

<center><img src="https://user-images.githubusercontent.com/31475037/59034493-1d5b9280-88a6-11e9-9a3e-a5000870ac14.png"></center>

> 스칼라 값으로 표현되던 W, x 둘다 벡터의 형태로 표현된다.

<center><img src="https://user-images.githubusercontent.com/31475037/59034494-1d5b9280-88a6-11e9-8bb4-71e90de0c79b.png"></center>

> Matrix multiplication

<center><img src="https://user-images.githubusercontent.com/31475037/59034496-1d5b9280-88a6-11e9-9995-1618f5ac3be7.png" width="70%"></center>

> Matrix 형태의 표현

<center><img src="https://user-images.githubusercontent.com/31475037/59034497-1d5b9280-88a6-11e9-8437-8eae0bc0cc00.png" width="500" height="300"></center>

> Tensor 개념

<center><img src="https://user-images.githubusercontent.com/31475037/59034498-1df42900-88a6-11e9-9999-4c3b9035ff68.png" width="600" Height="400"></center>

### Logistic Regression

> 데이터가 이럴경우는?

<center><img src="https://user-images.githubusercontent.com/31475037/59034501-1e8cbf80-88a6-11e9-951e-e92ed4523f47.png" width="80%"></center>

> 새로운 모형의 함수를 사용

<center><img src="https://user-images.githubusercontent.com/31475037/59034502-1e8cbf80-88a6-11e9-87b0-e28d9ee426f0.png" width="80%"></center>



<br>

[^1]: [회귀의 어원](<https://m.blog.naver.com/PostView.nhn?blogId=wjn21&logNo=220993310138&proxyReferer=https%3A%2F%2Fwww.google.com%2F>)

<br>

