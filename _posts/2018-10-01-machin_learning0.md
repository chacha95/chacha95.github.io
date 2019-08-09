---
layout: post
title: What is machine learning?
tags: [Machine Learning]
---

## Machine Learning

machine learning은 데이터로부터 학습하도록 컴퓨터를 프로그래밍하는 과학입니다.

예를들면 스팸 필터는 스팸 메일과 일반 메일의 샘플을 이용해 스팸 메일을 구분하는 법을 배울 수 있게 만들어 졌습니다.

> 왜 machine learning을 사용하는가?

**전통적 접근 방법**

전통적인 접근 방법으로 문제를 풀려하면, 문제가 단순치 않아 규칙이 점점 길고 복잡해져 유지 보수하기가 굉장히 힘들어 집니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59028100-a9b18980-8895-11e9-8dbd-34105da26ad8.png" width="70%"></center>
**machine learning 접근 방법**

프로그램이 훨씬 짧아지고 유지보수하기 쉬우며 대부분 정확도가 더 높습니다. 또한 machine learning은 데이터 업데이트만 잘 해준다면 자동으로 변화에 대응합니다. 복잡한 문제와 대량의 데이터로부터 패턴을 분석 할 수 있습니다.

> machine learning을 이용한 시스템

<center><img src="https://user-images.githubusercontent.com/31475037/59028106-ab7b4d00-8895-11e9-9a5d-9c85406c9302.png" width="70%"></center>
> 머신러닝은 자동으로 변화에 대응할 수도 있음

<center><img src="https://user-images.githubusercontent.com/31475037/59028455-9521c100-8896-11e9-9b8e-d972f93f054b.png" width="80%"></center>
<br>


### machine learning 시스템의 종류

<center><img src="https://user-images.githubusercontent.com/31475037/59027794-eaf56980-8894-11e9-8829-acd3264fb66d.png" width="50%"></center>
> 사람 감독 유무에 따른 종류

<center><img src="https://swalloow.github.io/assets/images/ml-diagram.png" width="100%"></center>
**Supervised Learning(지도 학습)**

훈련 데이터에 레이블(답)이 달려 있는 데이터를 이용해 학습을 시키는 방법입니다.

전형적인 지도학습으로는 분류가 있습니다.

지도학습은 Classification(분류)와 Regression(회귀)로 나뉘어집니다. 먼저, **Classification**은 주어진 데이터를 정해진 카테고리에 따라 분류하는 문제를 말합니다.

예를 들어, 이메일이 스팸메일인지 아닌지를 예측한다고 하면 이메일은 스팸메일 / 정상적인 메일로 라벨링 될 수 있을 것입니다. 비슷한 예시로 암을 예측한다고 가정했을 때 이 종양이 악성종양인지 / 아닌지로 구분할 수 있습니다. 이처럼 맞다 / 아니다로 구분되는 문제를 Binary Classification 이라고 부릅니다.

분류 문제가 모두 맞다 / 아니다로 구분되지는 않습니다. 예를 들어, 공부시간에 따른 전공 Pass / Fail 을 예측한다고 하면 이는 Binary Classifiaction 으로 볼 수 있습니다. 반면에, 수능 공부시간에 따른 전공 학점을 A / B / C / D / F 으로 예측하는 경우도 있습니다. 이러한 분류를 Multi-label Classification이라고 합니다.

**Regression**은 연속된 값을 예측하는 문제를 말합니다. 주로 어떤 패턴이나 트렌드, 경향을 예측할 때 사용됩니다. Regression을 설명할 때 항상 집의 크기에 따른 매매가격을 예로 듭니다. 아까와 유사한 예를 들자면, 공부시간에 따른 전공 시험 점수를 예측하는 문제를 예로 들 수 있습니다.

대표적 supervised learning 방식은 다음과 같습니다

* Linear Regression(회귀)
* Logistic Regression(분류)
* Support Vector Machine
* Decision Tree and Random Forests
* Neural Network

**Unsupervised Learning(비지도 학습)**

훈련 데이터에 레이블이 없습니다. 시스템이 아무런 도움도 없이 학습을 해야 합니다.

대표적인 unsupervised learning 방식은 다음과 같습니다

* Clustering
* Visualization

**Semi-Supervised Learning(준지도 학습)**

지도학습은 앞에서 언급한 것 처럼 라벨링이 되어 있지 않은 데이터로부터 미래를 예측하는 학습방법입니다. 평가되어 있지 않은 데이터로부터 숨어있는 패턴이나 형태를 찾아야 하기 때문에 당연히 더 어렵습니다. 비지도학습도 데이터가 분리되어 있는지 (Categorial data) 연속적인지 (Continuous data)로 나누어 생각할 수 있습니다.

**Reinforcemence Learning(강화 학습)**

학습 환경을 관찰해서 그 결과로 보상 또는 벌점을 부여하는 방식입니다. 가장 큰 보상을 얻기 위해 최상의 전략을 스스로 학습합니다. 

대표적 방식은 다음과 같습니다

* Alphago
* DQN

> 실시간 학습 유무에 따라

**Batch Learning**

전체 데이터를 한번에 학습시키는 방식입니다.

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

### Test Set과 Training Set

모델이 새로운 샘플에 얼마나 잘 일반화될지 아는 방법은 새로운 샘플에 실제로 적용해 보는 것입니다.

Training 오차는 낮지만 Test 오차가 높다면 이는 모델이 overfitting되있다는 것을 알 수 있습니다.

보통 데이터의 80%는 Training에 사용하고 20%는 Test용으로 떼어 놓습니다.



### Deeplearning과 MachineLearning

최근들어 deeplearning의 성능이 기존 사람이 만들던 알고리즘(hand-crafted algorithm)들의 성능을 월등히 뛰어넘으면서 굉장히 주목받고 있습니다. 

deeplearning은 machinelearning의 한 분야이며 deeplearning의 여러 기법들이 machinelearning의 기술로 부터 나오고 있습니다..(ensemble, boosting...)

<center><img src="https://www.sumologic.com/wp-content/uploads/compare_AI_ML_DL.png" width="90%"></center>
<br>

[회귀와 분류](https://nexablue.tistory.com/entry/ML-%EB%B6%84%EB%A5%98Classification%EC%99%80-%ED%9A%8C%EA%B7%80Regression?category=728962)

<br>



