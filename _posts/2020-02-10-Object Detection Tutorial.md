---
layout: post
title: Object Detection(객체 검출) 기초
tags: [Object Detection]
use_math: true
---

## Object Detection(객체 검출) 이란?

Object detection(객체 검출)은 입력 영상이 주어질 때, 영상 내에 존재하는 모든 카테고리에 대해서 classification(분류)과 localization(지역화)를 수행하는 task입니다.

Localization은 bounding box를 찾는 regression(회귀)이고, classification은  bounding box 내 물체가 무엇인지 분류하는 문제입니다.

> object detection = classification + localization

![](https://user-images.githubusercontent.com/31475037/74116000-ef78db80-4bf4-11ea-8b7a-ba42fed2db57.png)

입력 영상에 따라 존재하는 물체의 개수가 일정치 않고, 0~N개로 변하기에 난이도가 높은 문제입니다.

Object detection과 유사한 문제로는 semantic segmentation 문제가 존재합니다. semantic segmentation은 각 픽셀에 대해 클래스를 정해주는 문제입니다. 각 픽셀마다 클래스를 예측하기에 semantic segmentation은 dense prediction이라고도 불립니다.

Semantic segmentation은 같은 클래스의 instance(물체)를 구별하지 않지만, instance segmentation의 경우 같은 class의 다른 instance(물체)를 구분합니다.

> 각 task별 차이점

![](https://user-images.githubusercontent.com/31475037/74116002-f0117200-4bf4-11ea-81e2-a09a25ab6e91.png)

<br>

## Measurement(평가)

Object detction에 대한 평가 방법은 다음과 같습니다.

### IoU(Intersection over Unit)

Object detection에서 bounding box를 얼마나 잘 예측하였는지는 IoU라는 지표를 통해 측정합니다. 정답인 GT(Ground Truth)와 예측된 bounding box가 얼마나 겹치는가를 측정합니다.

> IoU 계산 예시

![](https://hoya012.github.io/assets/img/object_detection_fourth/fig1.PNG)



IoU가 특정 값(threshold)을 넘으면 정답으로 취급됩니다.

- PASCAL VOC: 0.5
- MS COCO: 0.5, 0.55, 0.6 ..... 0.95



> IoU 값 예시

![](https://www.pyimagesearch.com/wp-content/uploads/2016/09/iou_examples.png)



### Confusion matrix(혼동행렬)

Classification 모델의 예측 성공률을 요약한 NxN 표입니다. Confusion matrix의 축 중 하나는 모델이 예측한 label이고, 다른 축은 실제 label입니다. N은 클래스 수를 나타냅니다.

다음과 같이 악성으로 분류된 종양(positive class) 또는 양성으로 분류된 종양(negative class) 샘플 100개의 **accuracy**, **recall**, **precision**을 계산해 보겠습니다.



> binary classification(이진 분류)에서의 예시, N=2인 경우

![](https://user-images.githubusercontent.com/31475037/61768363-3acac880-ae22-11e9-8fa6-23918485ebb5.png)

### accuracy

- 한국말로는 정확도(성)
- 전체 결과에 대해 정답을 맞춘 비율



> accuracy 계산

$$
accuracy = \frac{TP+TN}{TP+TN+FP+FN}=\frac{1+90}{1+90+1+8}=0.91
$$

accuracy는 91%로 나타났으며 이는 분류를 제대로 했음을 나타냅니다.



### recall

- 한국말로는 재현율(직관적으론 검출율이라고 봐도 됨)
- sensitiviy라고도 불림
- 대상 물체들을 빠뜨리지 않고 얼마나 잘 잡아내는지를 나타냄
- 실제 양성 중 정확히 양성이라고 식별된 사례의 비율



이 모델의 재현율이 0.11라는 의미는 모델에서 검출된 모든 악성 종양 중 11%만이 진짜 악성 종양이라는 의미입니다.

> 재현율 계산

$$
recall = \frac{TP}{TP+FN}=\frac{1}{1+8} = 0.11
$$



### precision

- 한국말로는 정밀도
- 검출된 결과가 얼마나 정확한지를 나타냄
- 양성으로 식별된 사례 중 실제로 양성이었던 사례의 비율



이 모델의 정밀도가 0.5라는 의미는 모델이 어떤 종양이 악성일 것이라고 평가 했을 때, 이 평가가 정확할 확률이 50%라는 의미입니다.

> 정밀도 계산

$$
precision = \frac{TP}{TP+FP}=\frac{1}{1+1}=0.5
$$



<br>

### Precision과 Recall의 관계

일반적으로 알고리즘의 recall과 precision은 서로 반비례 관계를 가집니다. recall을 높이면 오검출이 증가하고, 반대로 오검출을 줄이기 위해 조건을 강화하면 recall이 떨어집니다. 그렇기에 알고리즘을 제대로 평가하기 위해선 precision-recall 그래프를 활용해야 합니다.

하지만 정량적으로 분석하기에 힘들기에 average precision을 이용해 평가하는 방식이 나왔습니다.

아래 그래프의 면적으로 AP(Average Precision)가 계산됩니다.

> precision-recall 그래프에서 AP를 구함

<center><img src="https://t1.daumcdn.net/cfile/tistory/220E10365869F5CA34"></center>
Object Detection 관련 논문들을 읽다보면 성능평가 부분에 mAP(mean Average Precision)를 이용해 측정을 하는데, mAP는 AP 값에 대해 평균을 내준다 해서 mAP입니다. Recall을 0.1 단위 증가 시켰을 때, 각 단위별 precision의 평균값을 구하면 mAP가 나오게 됩니다.

<br>

## 기존 방법론

전통적인 computer vision, image processing에서의 object detection에 대해 간단히 소개드립니다. 

가장 간단히 생각해 볼 수 있는 방법은 물체가 존재할 수 있는 모든 크기의 영역에 대해 sliding window 방식으로 이미지를 모두 탐색하는 방식입니다. 사용자가 patch(영역)의 크기를 지정해 모든 영역을 탐색합니다.

그렇지만 이 방식은 탐색 영역의 수가 너무 많고, 겹치는 patch에 대해 feature(특징)를 공유하지 않아 computational cost(연산 시간, 비용)이 많이 소모됩니다.

> 가장 간단한 방법 - sliding window

![](https://user-images.githubusercontent.com/31475037/74116003-f0aa0880-4bf4-11ea-9e6c-cb92482f25cd.png)



이러한 문제점을 해결하기 위해 여러 방법론들이 제안되었고 전체 영역을 빠짐없이 탐색하는 것보다는 물체가 있을 법한 영역을 찾아내는 알고리즘들이 제안 되었습니다. 이러한 알고리즘들은 region proposal(지역 제안) 알고리즘이라 불립니다. 이 중 R-CNN과 연관이 깊은 selective search에 대해 알아 보겠습니다.

> 물체가 있을 법한 영역을 제안하는 region proposal 알고리즘

![](https://user-images.githubusercontent.com/31475037/74116005-f1429f00-4bf4-11ea-8314-d66839296806.png)



이미지 내의 구조적 특징(색상, 무늬, 크기, 모양)의 similarity(유사도)를 계산해 객체의 후보 영역을 뽑아내는 방식입니다. 각 컬러공간(RGB, HSV), 텍스쳐 등의 요소의 히스토그램을 구해서 similarity를 계산, 비슷한 것 끼리 영역을 통합시키는 bottom-up 방식입니다. 방법론적으로 보자면 low-level feature에 대해 superpixel을 greedy하게 합치는 알고리즘입니다.

> Selective search 예시

![](https://user-images.githubusercontent.com/31475037/74116006-f1db3580-4bf4-11ea-8648-ef6e01a323f3.png)



Selective search는 크게 3개의 스텝으로 구성됩니다.

1. 초기 sub-segmentation을 수행

   "Efficeint graph-based image segentation" 논문의 방식을 이용, 각각의 객체가 1개의 영역에 할당 될 수 있도록 많은 초기 영역을 생성

2. 작은 영역을 반복적으로 큰 영역으로 통합

   Greedy 알고리즘을 사용하여 작은 영역을 큰 영역으로 통합

3. 통합된 영역을 바탕으로 후보 영역 생성

여기서 similarity를 계산하는 방식은 세가지 입니다.

1. 컬러 유사도

   각각 컬러 채널에 대하여 히스토그램을 구함

2. 텍스쳐 유사도

   SIFT를 이용, 8방향의 가우시안 미분값을 구하여 히스토그램 계산

3. 크기 유사도

   영역 합칠 때 작은 영역을 우선적으로 고려하도록 만듦

> Selective search의 순서

![](https://user-images.githubusercontent.com/31475037/74121941-da0fab80-4c0c-11ea-88b0-04e7bbaab1c3.png)

이러한 selective search는 SVM과 같이 사용되어 2013년도 ILSVRC의 detection 분야에서 1등을 했으며, 굉장히 좋은 성능을 보여주었습니다.

<br>

## R-CNN 계열

Selective search 이후에 나온 R-CNN 기술은 object detection 기술에 있어 굉장히 좋은 성능을 냈으며, 딥러닝을 이용한 object detection 기술의 근간이 되는 논문입니다. 

### R-CNN

R-CNN은 selective search를 딥러닝에 적용한 알고리즘 입니다. ILSVRC 2012에서 우승한 AlexNet 모델을 CNN 모델로 사용했으며, pretrained된 AlexNet 모델을 다시 학습하는 transfer learning을 이용해 성능을 올렸습니다.

R-CNN의 과정은 다음과 같습니다.

1. selective search를 통해 RoI(Region Of Interest)를 약 2000개 가량 추출 합니다.
2. RoI의 크기를 조절해 동일한 사이즈로 만듦니다.
3. RoI를 CNN에 넣어줘 feature를 추출합니다.
4. CNN을 통해 나온 feature를 SVM에 넣어줘 classification 해줍니다.
5. CNN을 통해 나온 feature를 regression을 통해 bounding box를 예측해 줍니다. 

> R-CNN

![](https://user-images.githubusercontent.com/31475037/74123157-088f8580-4c11-11ea-8555-39cfb5d770c0.gif)

CNN을 통해 나온 bounding box가 정확하지 않기에 bounding box의 위치를 보정해주는 간단한 regression을 진행해줍니다.

> Bounding box(bbox) regression

![](https://user-images.githubusercontent.com/31475037/74116008-f1db3580-4bf4-11ea-8c28-bbb7de1c786c.png)



R-CNN은 좋은 성능을 냈지만 몇가지 문제점이 존재했습니다.

1. 학습이 너무 느림

   Tesla K40을 사용했을 시 84시간이 소요되며, 너무 많은 gpu 메모리 차지

2. Selective search로 찾아낸 2000개의 영역을 각각 CNN을 통해 feature를 추출하기에 연산량이 너무 많음

3. Selective search 자체가 시간이 오래 걸리는 알고리즘

4. Test(inference)에 시간이 너무 많이 걸림

5. CNN, classifier, bbox regression 세 가지를 따로 학습해야 합니다

   End-to-end 학습 방식이 아님

이러한 문제점을 해결하기 위해 Fast R-CNN이 제안되었습니다.



### Fast R-CNN

학습과는 별도로 selective search에서 얻어진 각각을 CNN에 넣는 방식이 아닌 이미지를 통째로 CNN에 넣는 방식입니다. 이 방식을 이용해 R-CNN에서 CNN 연산을 2000번 하던걸 1번으로 줄였습니다.

Fast R-CNN 구조에 대해 알기 위해서는 RoI projection과 SPPNet에 대해 알아야 합니다.

**RoI projection**

RoI projection은 feature map 상에 있는 RoI를 구하기 위한 프로세스입니다. 과정은 다음과 같습니다.

- 이미지에 대해 selective search를 통해 RoI를 구해줍니다. 
- 이미지에서 구해진 RoI 좌표값은 CNN을 통과한 feature map 상에 projection 시켜줍니다. 이를 통해 feature map 상에 RoI가 구해집니다.
- 이 때, CNN을 통과한 feature map의 spatial resolution은 입력 이미지의 spatial resolution과 동일합니다. 

<center><img src="https://user-images.githubusercontent.com/31475037/74294049-1283c680-4d80-11ea-8c68-fa1b84f52bd3.png" width="70%"></center>
이러한 RoI projection이 가능한 이유는 CNN을 통과해 추출된 feature map에 이미지와 같이 물체의 중요한 정보들이 포함되어 있기 때문입니다. 



**SPP(Spatial Pyramid Pooling)**

SPPNet은 R-CNN에서 resize(warping)하는 과정을 SPP를 이용해 고정 크기로 바꿨습니다. 이 중 SPP layer는 다양한 크기의 입력으로부터 일정한 크기의 feature를 추출해 낼 수 있는 방법입니다. 이미지를 여러 개의 일정 개수 지역으로 나눈 뒤, 각 지역에 BoW(Bag-of-words)를 적용하여 local(지역적)한 정보를 유지합니다. 해당 방식을 제안한 논문이 바로 SPPNet입니다. SPPNet에서 제안한 SPP layer는 feature map 상의 특정 영역에 대해 고정된 개수의 영역으로 나눈 뒤, 각 영역에 대해 max pooling 또는 average pooling을 취함으로써 고정된 길이의 feature를 추출하게 됩니다.



> SPPNet

![](https://1.bp.blogspot.com/-4XYvgIQ6T8E/VZEPbZyYo7I/AAAAAAAABHE/D_HccWnYK6Q/s1600/s4.jpg)



Fast R-CNN에서는 이런 SPP layer의 한 single level pyramid만을 사용하여 이를 RoI layer라 명명했습니다.

> RoI pooling layer의 작동 과정

![](http://openresearch.ai/uploads/default/original/1X/eb4fecb57d3da1f8ba67e35b444091eb5b391d35.gif)

Faster R-CNN의 과정은 다음과 같습니다.

1. 전체 이미지를 CNN에 넣어줘 feature를 추출(CNN을 한번만 사용)
2. RoI projection을 통해 feature map 상에서의 RoI를 구해줌
3. feature map 상의 RoI는 RoI layer를 통과한 후 일정한 크기의 feature가 추출되서 나옴
4. 추출된 feature는 fc layer(fully cunnected layer)를 통과해 나온뒤, classification과 bounding box regression을 해줌, 이 때 각각을 따로 학습하는 것이 아닌 한번에 학습해주는 end-to-end 학습을 함 

> Fast R-CNN

![](https://user-images.githubusercontent.com/31475037/74294468-52977900-4d81-11ea-87d2-cbd12ee7fb11.gif)



R-CNN과 fast R-CNN의 차이는 다음과 같습니다.

> R-CNN과 Fast R-CNN의 비교

![](https://user-images.githubusercontent.com/31475037/74124107-86a15b80-4c14-11ea-832f-90f98776c18d.png)

> 간략히 비교해본 차이점

![](https://user-images.githubusercontent.com/31475037/74124110-87d28880-4c14-11ea-9059-2a532263efce.png)

하지만 Fast R-CNN에서는 여전히 다음과 같은 문제가 남아있습니다.

- Region proposal(selective search) 방식이 CPU에서 동작함
- CNN 외부에서 region proposal이 진행됨
- 2.32초의 test 과정중 region proposal에 2초가량이나 시간이 소모

> Region proposal에 너무 많은 시간이 소모됨

![](https://user-images.githubusercontent.com/31475037/74124112-886b1f00-4c14-11ea-91d9-1861d26d890c.png)

### Faster R-CNN

Faster R-CNN은 기존에 Fast R-CNN이 가진 속도 문제를 해결하기위해 제안되었습니다. 문제가 되던 selective search 부분을 RPN(Region Proposal Network)으로 대체하였습니다. 즉 selective search를 통해 region proposal을 하던것을 RPN을 통해 region proposal을 합니다.

Faster R-CNN의 학습은 크게 RPN 학습과 detector학습으로 구분됩니다. 학습시 이미지에서 feature를 추출하는 CNN은 공유가 됩니다.(논문상에선 shared CNN이라 나오며, 이후 backbone이라고 보통 불립니다) detector는 Fast R-CNN과 동일합니다.

> Faster R-CNN의 구조

![](https://i1.hdslb.com/bfs/archive/bfe6c083590272fbb8fcdbc94859c96e866de9a5.jpg)

**RPN 학습**

RPN은 내부 feature map의 영역내에서도 충분히 객체의 위치, 특징정보가 남아 있기 때문에 이 feature map 정보를 통해 학습을 하는 방식입니다. 

RPN에서 각각의 영역을 어떻게 학습할지에대해 논문에서 도입한 개념이 anchor box입니다. Anchor를 중심으로 anchor box를 설정해 feature map에서 영역을 설정합니다. 저자는 anchor box를 사용하면 기존 방법들과는 다르게 translation-invariance(이동에 불변)하다고 주장합니다. 또 anchor box를 사용하면 모델의 사이즈를 크게 줄일 수 있다고 말합니다.

> Translation-invariance

![](https://user-images.githubusercontent.com/31475037/74295754-4ca39700-4d85-11ea-96ef-99320b686da5.png)

RPN 이전엔 translation-invariance하게 만들기 위해 여러가지 시도들이 존재해왔습니다.

아래 그림의 (a)는 이미지 scale을 조절해 각 이미지 크기별 이미지 정보를 수집하는 multi scale 방식, (b)는 다양한 filter size를 이용하는 방식, (c)가 다양한 anchor box를 사용하는 방식입니다. 저자는 기존 방법 대비 성능과 속도 측면 모두 우수하다고 주장합니다.

> 기존 방법론과 anchor box 방법론의 차이

![](https://user-images.githubusercontent.com/31475037/74295756-4ca39700-4d85-11ea-9a1b-0f9a7fe64e1d.png)



RPN 학습은 객체를 분류하기 위한것이 아닌, 객체가 있는 영역인 positive anchor box를 RPN이 잘 찾는 것이 목표입니다. 논문에서 제안하는 positive anchor box와 negative anchor box의 정의는 다음과 같습니다.

- positive anchor box: GT box와 IoU가 0.7이상
- negative anchor box: GT box와 IoU가 0.3이하
- GT box와 IoU가 0.3~0.7 사이의 값은 사용하지 않음

논문에선 k개의 anchor box를 정의해 사용한다고 나와있습니다. 논문에서 실제 사용한 anchor box는 크기에 따라 3가지, 가로 세로 비(ratio)가 다른 3가지를 포함해 총 9가지입니다. Scale 종류는 [128x128, 256x256, 512x512], ratio 종류는 [2:1, 1:1, 1:2] 입니다.

> anchor

![](https://user-images.githubusercontent.com/31475037/74295757-4d3c2d80-4d85-11ea-8691-c16c64fd96e8.png)

이렇게 다양한 anchor를 기반으로 k개의 anchor box가 적용되어 multi-scale에 대해 학습하게 됩니다.

> anchor와 anchor box가 실제 적용된 예시

![](https://user-images.githubusercontent.com/31475037/74295761-4dd4c400-4d85-11ea-8a26-5387a5a063b8.png)

이렇게 만들어진 anchor 값들은 한 이미지당 랜덤하게 256개를 sampling합니다. Sample은 positive anchor : negative 비율을 1:1 비율로 섞어 RPN에 넣어주면, 해당 anchor에 object가 있는지 없는지 이진 분류를 하는 classifer를 학습하고, 앵커 내 물체 위치를 찾는 bbox regression을 해줍니다. 만약 positive anchor의 개수가 128개보다 낮을 경우, 빈 자리는 negative anchor sample로 채웁니다.

RPN의 학습에는 다음과 같은 loss 함수가 사용됩니다.

> loss 함수

![](https://curt-park.github.io/images/faster_rcnn/LossFunction.png)



- **pi**: Predicted propability of anchor
- **pi***: GT label(1: positive anchor, 0: negative anchor)
- **Lcls**: object 인지 아닌지 판별하는 binary classification 부분, cross entropy를 이용해 loss를 구해주는 부분
- **Lreg**: bbox regression을 해주는 로스 함수 
- **Ncls**: classification loss의 normalize를 위한 하이퍼파라미터, 논문에선 256으로 설정
- **Nreg**: regression loss의 normalize를 위한 하이퍼파라미터, 논문에선 2400
- **lambda**: 밸런스를 조절하는 하이퍼파라미터로 Ncls와 Nreg의 차이로 발생하는 불균형을 방지하기 위해 사용됨. Ncls가 256이고 Nreg가 2400이라 했을 때, lambda 값은 10으로 설정함
- **ti**: Predicted bbox 좌표
- **ti***: GT bbox 좌표

이 중 bounding box regression을 해주는 loss 함수에서 bbox의 4개 좌표값에 대해 smooth L1 loss를 이용해 regression을 해줍니다. Smooth L1 loss는 L1 혹은 L2 loss보다 outliers에 덜 민감해 bbox regression에서 자주 사용됩니다.

> smooth L1 loss를 이용해 bbox regression을 해줌

![](https://curt-park.github.io/images/faster_rcnn/SmoothL1.png)



> L1, L2, smooth L1의 차이

![](https://www.researchgate.net/profile/Zhenhua_Feng3/publication/321180616/figure/fig4/AS:631643995918366@1527607072866/Plots-of-the-L1-L2-and-smooth-L1-loss-functions.png)



**Detector 학습**

RPN을 학습시킨 후 RPN과 CNN 부분을 freeze(고정)시킨 후 detector만 학습합니다. Detector는 fast R-CNN에서 쓰이던 것과 동일합니다.

Detector를 학습시키는 loss는 RPN과 거의 비슷합니다. 다만 classification시 binary classification을 하는게 아닌, multi-class classification을 해줍니다. 즉, 분류해야될 클래스가 2개가 아닌 여러개입니다.

> Detector 학습

![](https://user-images.githubusercontent.com/31475037/74295747-4ad9d380-4d85-11ea-9be8-f6e63e901f6f.png)

**전체 학습 과정**

본 논문에서 제안하는 전체 학습 과정은 다음과 같습니다.

1. RPN과 CNN만 따로 학습합니다.
2. RPN과 CNN은 freeze한채로 detector만 학습합니다.
3. CNN은 freeze한채로 RPN만 finetune합니다.
4. RPN과 CNN은 freeze한채 detector만 finetune합니다.

![](https://user-images.githubusercontent.com/31475037/74299120-0f440700-4d8f-11ea-84de-585dcf1d0198.png)



본 논문에선 4 스텝으로 나누어진 복잡한 학습 방법을 제안했지만, 저자가 나중에 둘다 한번에 학습하는 방법을 공개해서 현재는 4 스텝 학습 방식은 쓰지 않습니다.



**Non-Maximum Suppression(NMS)**

RPN이 region proposal을 해 bbox를 예측하면, 한 객체당 여러개의 bbox를 얻습니다. 이 중복 문제를 해결하기 위해서 NMS 알고리즘을 이용해 bbox의 개수를 줄입니다. NMS는 RPN에서 나온 class값(object인지 아닌지 판별하는 확률값)으로 bbox를 정렬시킨 뒤, bbox 간의 IoU가 threshold 이상이면 지우는 알고리즘입니다. 그렇게 되면 서로 겹치지 않으면서 IoU가 높은 bbox만 남게 됩니다. 논문에서 사용된 threshold 값은 0.7입니다. NMS 이후, class 값이 N번째로 큰 bbox까지만 사용합니다. 논문에서 test시에는 N값이 300, training시에는 N값이 2000입니다.



> NMS

![](http://incredible.ai/assets/images/faster-rcnn-nms-algo.jpg)



**Performence**

Faster R-CNN은 기존 R-CNN 계열의 기술들 보다 속도적인 측면, 성능적인 측면 둘다 좋은 결과를 냈습니다. 특히나 속도적인 측면에 있어 괄목할만한 결과를 냈습니다.

> Faster R-CNN의 성능

![](https://bloglunit.files.wordpress.com/2017/06/20170525-research-seminar-google-slides-2017-06-01-20-27-50.png?w=508&h=177)



<br>

## FPN

전통적인 컴퓨터 비전 알고리즘에서 많이 쓰이던 알고리즘 중 하나는 이미지를 여러 scale로 resize 한 뒤 그 각각으로부터 feature를 뽑아내는 방식이 존재합니다. 이러한 방법을 **feature pyramid**라 부릅니다. 대표적으로 SIFT나 HOG가 있습니다. 

하지만 딥러닝이 등장한 뒤 object detection 분야에서 feature pyramid 방식은 computation cost가 높고 memory 문제가 존재했습니다. 왜냐하면 각각의 scale에 대해서 CNN을 통해 feature를 뽑아야 했기 때문입니다.

> 전통적인 feature pyramid 기법

<center><img src="https://user-images.githubusercontent.com/31475037/74503142-52d77600-4f33-11ea-808d-50106668ffa7.png" width="70%"></center>
CNN을 통해 feature를 뽑는 방식은 single feature map이라고 불립니다. 이미지가 있을 때 CNN을 통해 순차적으로 feature map을 뽑고 마지막 feature map에서 prediction을 하는 구조입니다. CNN은 Bottom-up 방식으로 이미지를 본다고도 불립니다.


> CNN을 통한 feature 추출

<center><img src="https://user-images.githubusercontent.com/31475037/74511677-def49800-4f49-11ea-96ff-9ed82d42dc8e.png" width="70%"></center>
다음으론 SSD(Single Shot Detector)에서 제안한 방식입니다. 마지막 feature map에서만 prediction을 하는 것이 아니라, 각 feature map 마다 prediction을 하는 방식으로, 중간 중간 나오는 feature map을 활용하는 방식입니다.

> hierarchy한 방식

<center><img src= "https://user-images.githubusercontent.com/31475037/74511676-de5c0180-4f49-11ea-89e1-9e1680ac08bd.png" width="70%"></center>
마지막으론 FPN 저자가 제안하는 방식이 있습니다. FPN의 주요 아이디어는 Bottom-up 방식의 feature 추출과 Top-down 방식의 feature 추출 두가지를 모두 사용합니다. 

> FPN에서 제안하는 방식

<center><img src= "https://user-images.githubusercontent.com/31475037/74511675-de5c0180-4f49-11ea-96fa-730c64517ead.png" width="70%"></center>
### Top-down 과 Bottom-up

Top-down과 Bottom-up 둘다 이미지의 feature를 뽑는지에 대한 접근 방법입니다. Bottom-up은 이미지의 픽셀 단위부터 특징을 계산해서 전체적인 형태나 특징을 찾아내는 방식입니다.

> 대표적인 Bottom-up 방식인 CNN

![](https://user-images.githubusercontent.com/31475037/74509490-f9784280-4f44-11ea-8789-036d2a8f2095.png)

Top-down의 접근방식은 이미지 전체를 보고 해당 이미지에서 원하는 특징을 찾아 내는 방식입니다. 사람의 눈이 사물을 인식하는 방식이 보통 Top-down 입니다.



### FPN 구조

FPN의 저자는 이러한 Top-down 방식과 Bottom-up 방식을 둘다 사용하면서 두가지 방식에서 추출된 feature map을 lateral cunnection(측면 연결)을 통해 각각의 stage마다 prediction을 하는 설계를 하였습니다. 이러한 설계를 통해 중간 중간 feature map들이 잘못된 feature를 추출하지 않으면서도, Top-down, Bottom-up 방식 둘다 사용하는 결과를 내었습니다. 

논문에서는 Bottom-up은 기존의 CNN을 사용하였으며, Top-down은 upsampling 기법을 사용하여 높은 해상도의 이미지를 만들도록 하였습니다. 여기서 lateral-cunnection을 사용해 bottom-up에서 추출된 feature map을 재사용하였습니다.

같은 classifier/regressor를 사용하기에 채널이 전부 같고, 논문에서는 256채널을 사용했습니다. 때문에 lateral-cunnection을 이용해 feature를 합칠 때 1x1 Conv를 이용해 채널을 맞춰줍니다.



> FPN 구조

![](https://user-images.githubusercontent.com/31475037/74509754-9e931b00-4f45-11ea-858f-b77b08141d3d.png)



### FPN + Faster R-CNN

FPN은 기본적으로 RPN의 컨셉을 많이 가져온 네트워크이나 몇가지 차이점이 존재합니다. 먼저 RPN에서 사용됬던 anchor를 여기서도 사용했습니다. 다만 Pyramid 각각의 층별로 scale이 달라 다양한 scale의 anchor는 사용하지 않고 다양한 ratio의 anchor를 사용했습니다. RPN에서 쓰인 ratio는 [1:2, 1:1, 2:1] 입니다.

저자는 Faster R-CNN 구조에서 RPN을 FPN으로 바꾼 구조를 제안했고, mAP 측면에서 더 나은 결과를 보여줬습니다. 

> 결과 분석

![](https://user-images.githubusercontent.com/31475037/74510234-b4551000-4f46-11ea-83bc-77f7b95c5dcd.png)



<br>

**참고 강의**

CS231N

[hoya012 blog](https://hoya012.github.io/blog/Tutorials-of-Object-Detection-Using-Deep-Learning-how-to-measure-performance-of-object-detection/)

[Google 머신러닝](https://developers.google.com/machine-learning/crash-course/classification/precision-and-recall?hl=ko)

[Precision and Recall에 대한 해석](https://darkpgmr.tistory.com/162)

[Lunit Blog](https://blog.lunit.io/2017/06/01/r-cnns-tutorial/)

