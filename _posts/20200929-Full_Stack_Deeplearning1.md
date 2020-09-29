---
layout: post
title: Full Stack Deep Learning 1 - 소개
tags: [Full Stack Deep Learning]
use_math: true
---

2012년부터 딥러닝은 이미지 인식에서 음성 인식, 로봇 공학 및 오디오 합성에 이르기까지 다양한 컴퓨팅 작업에서 놀라운 발전을 이루었습니다. 딥러닝은 자율 주행 차량, 실시간 번역 및 음성 지원과 같은 이전에는 불가능했던 새로운 기술을 구현하고 기존 소프트웨어 범주를 재창조 할 수있는 잠재력을 가지고 있습니다.

<br>

# 왜 많은 production level의 ML project가 망하는가?

하지만 research 영역에서 ML이 이룬 성과와는 달리, 현실에서 ML을 적용하기에는 너무나도 큰 난관들이 존재합니다. 

Research에서 이룬 성과를 production level로 바꾸기 힘들기에 대다수의 ML project는 실패합니다. 또한 성공에 대한 지표(metric)가 불명확하며, ML의 급부상에 비해 관련 인력과 노하우가 부족해 team management가 힘듭니다.

<br>

## 1. Research <<<(GAP)>>> Real-World

Research와 Real-World 사이의 GAP이 너무 심해 실제로는 사용할 수 없는 경우 입니다.

Research에서는 결과 값이 0 또는 1로 정확히 정의되지만, 현실에선 그렇지 않습니다. 또한 research에서는 70% 이상의 성능만 나온다면 다음 task로 넘어가지만, 현실에선 90%의 성능도 충분하지 않습니다.

> Research와 Real-World에는 분명한 차이가 존재함

![](https://user-images.githubusercontent.com/31475037/92843097-52e31a00-f41f-11ea-8ad7-7d69a133347d.png)



Research에서는 simulation 상황과 같이 한정된 조건에서만 실험을 한다면, 현실에서는 상황에 맞춰 변화해야 합니다.

> 상황에 따른 차이

![](https://user-images.githubusercontent.com/31475037/92843090-51b1ed00-f41f-11ea-9828-b5e1ffdf853a.png)

특히나 현실에선 실제 사람이 보기에도 잘 모르는 것을 모델이 예측해야 하는 경우도 생깁니다.

> 사람이 봐도 힘든 물체를 모델이 예측해야 함

![](https://user-images.githubusercontent.com/31475037/92843084-4fe82980-f41f-11ea-9b1e-90f92b2b6a30.png)

그 중에서도 가장 극명하게 차이가 나는 부분은 ML pipeline을 구축하는 것입니다. 연구실에서 COCO나 ImageNet 데이터셋과 같이 잘 차려진 데이터를 모델에 잘 넣으면 대다수의 경우 잘 작동합니다.

하지만 현실에선 잘 차려진 데이터도 없을 뿐더러, 데이터 수집부터 모델링까지 모든 부분을 해야합니다.

> 꿈에서 깨어나세요....

![](https://user-images.githubusercontent.com/31475037/92835771-99804680-f416-11ea-8586-e78c079b3c2f.png)

그래서 production level의 ML에서 순수한 model research에 들어가는 비중은 5%라고 합니다.

> ML research(ML core)는 정말 극소수 부분만을 차지함

![](https://user-images.githubusercontent.com/31475037/92835769-98e7b000-f416-11ea-9b09-f4d4e12c7d35.png)

<br>

## 2. Cost 측면

### Accuracy에 따른 cost

현실에서는 cost에 대한 중요성이 커지는데, accuracy를 올리기 위한 cost가 linear한게 아니라 exponential하기에 accuracy를 0.1% 올리는데 투자한 비용 대비 그만한 돈을 벌 수 있는가에 대한 고려가 필요합니다.

![](https://user-images.githubusercontent.com/31475037/92834745-55407680-f415-11ea-852f-0df58b7b9e29.png)

### Data 수집에 따른 Cost

이 외에도 production level의 ML에서는 데이터 수집을 위한 생태계 구성이 중요하며 아래와 같이 순환적인 구조를 지닙니다. 하지만 이러한 순환 구조를 이루기 위해선 플랫폼이 필요하며, 초기 투자 비용이 엄청나게 많이 드는 단점이 존재합니다.

![](https://user-images.githubusercontent.com/31475037/92834802-6b4e3700-f415-11ea-9ecf-e4031c7faeec.gif)

또한 이런 순환 생태계를 만드는 것 이외에도, 수집된 데이터에 대한 레이블링이 동반되어야 하는데, 이러한 레이블에 드는 cost 또한 중요한 부분입니다. 

> data collection에 따른 데이터 퀄리티 차이

![](https://user-images.githubusercontent.com/31475037/92834740-5376b300-f415-11ea-95a0-243f4b6909b8.png)

<br>

## 3. Team management

많은 사람들이 간과하는 부분이 바로 team management입니다. 

ML의 경우 기존의 software 개발 방법론과는 다르며, ML 기반의 소프트웨어 개발 방법론을 Andrej Karpathy가 [Software 2.0](https://medium.com/@karpathy/software-2-0-a64152b37c35)이라고 정의했습니다. 

이에 기존 소프트웨어 개발 방법론에 따른 role과는 전혀 다른 형태를 가집니다.

현재 ML job role에 대한 완벽한 role이 정의되있지 않은 상태입니다. 실제로 ML engineer와 ML researcher를 완벽히 구분해서 채용하는 회사는 많지 않습니다. 하지만 정말 넓게 정의해 보면 아래와 같습니다.

![](https://user-images.githubusercontent.com/31475037/92840008-9045a880-f41b-11ea-96dd-f8b2830c700f.png)

그렇다면 각 role 별 필요한 skil은 어떨까요?

![](https://user-images.githubusercontent.com/31475037/92840215-c84ceb80-f41b-11ea-8a7e-92be66e0bb1c.gif)

<br>

# Lifecycle

그렇다면 research 영역이 아닌, 실제 production(ML project)에서의 lifecycle은 어떻게 될까요? 간단히 보여주자면 아래와 같습니다.

![](https://user-images.githubusercontent.com/31475037/92833692-14942d80-f414-11ea-8d90-f3dc80e63fb3.gif)

# ML 생태계

ML project의 생태계는 광활합니다. 거의 대부분이 open source로 이루어져 있으며, 블럭처럼 연동해서 쓸 수 있죠.

> 

![](https://user-images.githubusercontent.com/31475037/92835763-97b68300-f416-11ea-9054-f5dc424e0501.png)





<br>

**참고 강의**

 [Full Stack Deep Learning](https://course.fullstackdeeplearning.com/)

