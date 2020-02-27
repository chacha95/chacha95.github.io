---
layout: post
title: 1-stage Detector 리뷰
tags: [Object Detection]
use_math: true
---

기존에 존재하던 R-CNN 계열의 기술들은 2-stage detector 였습니다. 2-stage detector는 객체를 검출하는 정확도 측면에선 굉장히 좋은 성능을 냈지만, 속도(FPS) 측면에서는 너무나도 느렸습니다. 실제 Real-time detection을 하기 위해선 보통 30fps정도가 필요한데 Faster R-CNN의 경우 5fps 밖에 안됬으니까요. 

이런 속도 문제를 해결하기 위해 region proposal과 classification을 동시에 하는 1-stage detector가 제안되었습니다.

> object detection에서 중요한 모델들

![](https://user-images.githubusercontent.com/31475037/75324222-d2612f80-58b9-11ea-8ae5-e1ad0e1ccfd5.png)

<br>

## YOLO(You Only Look Once)

YOLO는 2015년에 나온 논문으로 기존에 object detection에서 주류가 되던 R-CNN 계열의 기술의 단점인 FPS(Frame Per Second) 문제를 해결했습니다. 

[YOLO CVPR2015 발표 영상](https://www.youtube.com/watch?v=NM6lrxy0bxs&t=680s) 을 보시면 기존 기술들의 FPS 문제가 무엇이고 해당 문제를 해결하기 위해 저자가 제안한 YOLO에 대해 간략히 설명하고 있습니다. 

기존에 발표된 DPM(Deformable Parts Models)에서 사용된 sliding window나 R-CNN에서 사용된 selective search를 기반으로한 region proposal 방식 때문에 속도 문제가 있습니다. 이러한 방식들을 탈피해 grid라는 개념을 도입해 속도를 향상시켰습니다.



### Unified Detection

기존에 region proposal과 classification을 따로 하던것을 한번에 하는 방식입니다.

입력 이미지를 **S x S** grid로 나눕니다. 만약에 객체의 중심이 grid cell에 들어가면, 해당 grid cell은 그 객체를 탐지하는 역할을 합니다.

각 gird cell은 **B**개의 bbox를 예측합니다. 그리고 해당 bbox에 대한 confidence score를 예측합니다. Confidence score란 해당 bbox안에 물체가 있는지 없는지 확률값으로 나타낸 값입니다. 

각 grid cell은 조건부 확률인 C를 P(class|object)로 예측합니다.  Bbox 개수인 **B**에 상관하지 않고, 오직 grid cell당 하나의 클래스 확률을 예측합니다.

논문에서 사용된 **B**와 **S**는 2와 7입니다.

> YOLO Unified Detection

![](https://www.dropbox.com/s/qf4vrq8udy8mcwe/Screenshot%202018-05-02%2014.33.00.png?raw=1)



### YOLO Pipeline

기본 네트워크 구조는 24개의 Conv layer와 2개의 FC layer로 이루어져 있습니다. 저자는 기본 구조 이외에도 Conv layer를 9개만 사용하는 Fast YOLO 구조도 제안해 성능을 극한으로 끌어올렸습니다.

> 전체 pipeline

![](https://miro.medium.com/max/1204/1*z5lLvL3r8MeOmgcGkYnT2A.png)



### YOLO 성능

YOLO는 물체를 탐지하는 성능은 많이 줄였지만 속도적인 측면에서 기존 기술들을 훨씬 뛰어넘는 결과를 가져왔습니다.

> Real-Time 성능을 보여줌

![](https://miro.medium.com/max/538/1*zxDddLucPHk2sM5DHHEWvw.png)



### YOLO의 한계

1. grid cell이 하나의 클래스만 예측 -> 작은 object가 하나의 cell에 여러개 있으면 예측이 힘듭니다. YOLO는 각 grid cell이 두개의 bbox만 예측하고, 하나의 클래스만 가지기에 이런 문제가 나타납니다.
2. Grid cell의 개수가 이미지당 98개 밖에 안되기에 bbox를 제대로 찾기가 힘든 문제가 존재합니다.



<br>

## SSD(Single Shot MultiBox Detector)

SSD는 전부 새로 만든 구조가 아닌 원래 잘 만들어졌던 네트워크 구조에서 중간에 feature map 추가적으로 뽑아 정량적으로 평가하는 구조를 띄었다.



1. single-shot detector

   single-shot detector는 사진의 변형 없이 한장으로 훈련, 검출을 하는 detector를 의미합니다. 기존의 딥러닝 모델들은 이미지를 변형시켜 네트워크에 집어넣었으나, SSD는 다릅니다.

2. Multi-scale feature map

   단 한장의 사진만을 가지고 여러 크기의 물체를 검출해야기에 



<br>

## RetinaNet





<br>

**참고 강의**

[object detection review](https://arxiv.org/pdf/1905.05055.pdf)

[ICCV2017 oral presentation - RetinaNet](https://www.youtube.com/watch?v=44tlnmmt3h0)

[YOLO review](https://towardsdatascience.com/yolov1-you-only-look-once-object-detection-e1f3ffec8a89)