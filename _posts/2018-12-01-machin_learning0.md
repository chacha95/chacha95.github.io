---
layout: post
title: What is machine learning
tags: [machine learning]
---

## Machin Learning

> machine learning이란?

machine learning은 데이터로부터 학습하도록 컴퓨터를 프로그래밍하는 과학입니다.

예를들면 스팸 필터는 스팸 메일과 일반 메일의 샘플을 이용해 스팸 메일을 구분하는 법을 배울 수 있게 만들어 졌습니다.

> 왜 machine learning을 사용하는가?

**전통적 접근 방법**

전통적인 접근 방법으로 문제를 풀려하면, 문제가 단순치 않아 규칙이 점점 길고 복잡해져 유지 보수하기가 굉장히 힘들어 집니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59012157-c2a84380-8871-11e9-8e05-8f52f67da8a2.png" width="70%"></center>

**machine learning 접근 방법**

프로그램이 훨씬 짧아지고 유지보수하기 쉬우며 대부분 정확도가 더 높습니다. 또한 machine learning은 데이터 업데이트만 잘 해준다면 자동으로 변화에 대응합니다. 복잡한 문제와 대량의 데이터로부터 패턴을 분석 할 수 있습니다.

<center><figure class="second">
	<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Interpolation-bilinear.svg/220px-Interpolation-bilinear.svg.png">
	<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/BilinearInterpolation.svg/220px-BilinearInterpolation.svg.png">
</figure></center>
<br>


### machine learning 시스템의 종류

<center><img src="https://user-images.githubusercontent.com/31475037/59012157-c2a84380-8871-11e9-8e05-8f52f67da8a2.png" width="90%"></center>

> 사람 감독 유무에 따라

**Supervised Learning(지도 학습)**

훈련 데이터에 레이블(답)이 달려 있는 데이터를 이용해 학습을 시키는 방법입니다.

전형적인 지도학습으로는 분류가 있습니다.

<center><img src="" width="90%"></center>

대표적 supervised learning 방식은 다음과 같습니다

* Linear Regression
* Logistic Regression
* Support Vector Machine
* Decision Tree and Random Forests
* Neural Network

**Unsupervised Learning or Self-Supervised Learning(비지도 학습)**

훈련 데이터에 레이블이 없습니다. 시스템이 아무런 도움도 없이 학습을 해야 합니다.

대표적인 unsupervised learning 방식은 다음과 같습니다

* Clustering
* Visualization

**Semi-Supervised Learning(준지도 학습)**

레이블이 있는 데이터와 레이블이 없는 데이터를 훈련 데이터로 함께 사용해 모델의 성능을 높이는 학습 방식을 말합니다.

**Reinforcemence Learning(강화 학습)**

학습 환경을 관찰해서 그 결과로 보상 또는 벌점을 부여하는 방식입니다. 가장 큰 보상을 얻기 위해 최상의 전략을 스스로 학습합니다. 

대표적 방식은 다음과 같습니다

* Alphago
* DQN

> 실시간 점진적 학습 유무에 따라

**Batch Learning**

시스템을 훈련시키고 제품 시스템에 적용할시 더 이상의 학습 없이 실행됩니다. 

새로운 데이터를 사용하려면 기존 데이터도 모두 처음부터 다시 훈련 시켜야 합니다. 

일반적으로 시간과 자원을 많이 소모하며, 보통 오프라인에서 수행됩니다.

**Online Learining**

데이터를 순차적으로 한개씩 혹은 mini batch라 부르는 작은 묶음단위로 주입하여 시스템을 훈련시킵니다.

매 학습단계가 빠르고, 비용이 적게 들어가 시스템은 데이터가 도착하는 즉시 학습할 수 있습니다.

메인 메모리보다 큰 거대한 데이터 셋을 학습할 수 있습니다.

데이터 일부를 읽어 들이고 훈련 단계를 수행하며, 전체 데이터가 모두 적용 될 때까지 반복합니다.

<br>

### machine learning의 주요 도전 과제

**충분하지 않은 훈련 데이터**

machin learning 알고리즘이 잘 작동하려면 데이터가 굉장히 많아야 합니다.

아주 간단한 문제에서라도 수천개의 데이터가 필요합니다.

**대표성 없는 훈련 데이터**

일반화가 잘되려면 우리가 일반화하기 원하는 새로운 사례를 훈련 데이터가 잘 대표하는것이 중요합니다.

**낮은 품질의 데이터**

훈련 데이터가 에러, 이상치, 잡음으로 가득하다면 machine learning 알고리즘이 데이터의 패턴을 파악하기 힘듭니다.

따라서 훈련 데이터의 전처리에 많은 시간을 투자해야 합니다.

**Overfitting(과적합)**

Overfitting은 모델이 훈련 데이터의 정답을 너무 잘 맞추지만, Generalization(일반성)이 떨어지는 경우를 말합니다. 과대적합을 막기위해 Regularization(정규화)와 같은 기법을 많이 씁니다.

<br>

### Test와 Validation

모델이 새로운 샘플에 얼마나 잘 일반화될지 아는 방법은 새로운 샘플에 실제로 적용해 보는 것입니다.

훈련 오차는 낮지만 test 오차가 높다면 이는 모델이 overfitting되있다는 것을 알 수 있습니다.

보통 데이터의 80%는 훈련에 사용하고 20%는 test용으로 떼어 놓습니다.

<br>