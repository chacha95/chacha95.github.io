---
 polayout: post
title: Video Understanding
tags: [Deeplearning, 영상처리]
---

## Video Understanding

최근 딥러닝을 이용해 image에 대한 classification이나 VQA(Visual Question Answer) 뿐만 아니라 temporal한 정보를 가진 video에 대한 연구들이 진행되고 있습니다.

 특히나 YouTube에서 이런 기술들이 사용되고 있으며, 대표적으로 large-scale video annotation이나 영상 앞에 쓰이는 tumbnail에 대한 auto selection에 대한 기술들이 사용되고 있습니다.

먼저 이런 video understanding에 대한 task에 필요한 training dataset에 대해 알아볼까요?

<br>

### Datasets

> Video Classification

비디오가 들어왔을 때 어느 class에 속하는지 분류하는 task 입니다.

- UCF 101

  Youtube videos, 13,320개의 video들과 101개의 action class를 가졌습니다.

  다양한 camera motion과 object의 외형, 자세, viewpoint, 배경, illumination을 가집니다.

  ![](https://user-images.githubusercontent.com/31475037/60317244-1dcce200-99a9-11e9-9d28-838437179595.png)

- Sports-1M

  Youtube videos

  1,133,157개의 video들과 487개의 sports class를 가집니다.

  ![](https://user-images.githubusercontent.com/31475037/60317218-08f04e80-99a9-11e9-9ac1-dfb766f3139c.png)

- **YouTube 8M**

  800만개의 video와 3862개의 클래스를 가졌습니다.

  해당 데이터셋을 이용한 YouTube 8M challenge가 진행되고 있습니다.

  현재로썬 video classification dataset 중에는 데이터셋 규모도 가장 크고, 매년 workshop이 진행되고 있습니다.

  [Youtube 8M](https://research.google.com/youtube8m/index.html)

  [technical paper](https://arxiv.org/pdf/1609.08675.pdf)



![](https://storage.googleapis.com/kaggle-media/competitions/youtube/YT8M.png)

![](https://user-images.githubusercontent.com/31475037/60317219-08f04e80-99a9-11e9-89f7-ca0dc424b954.png)



> Movie Querying

video clip에 대한 설명을 다는 task 입니다.

- LSMDC(Large Scale Movie Description Challenge)

  M-VAD and MPII-MD 데이터 셋을 이용한 challenge 입니다.

  main task는 다음 3가지 입니다.

  - Movie description(영화 설명)
  - Movie retrieval(영화 검색)
  - Movie Fill-in-the-Blank(QA)

![](https://user-images.githubusercontent.com/31475037/60317846-ba907f00-99ab-11e9-9ebb-b98ed7e70641.png)

> Challenges in Videos

이미지 영역에서의 task들과는 다르게 video 영역에서 task들은 다음과 같은 특징을 지닙니다.

- image dataset에 비해 video dataset의 계산량이 많다
- resolution이 낮거나, motion blur와 같은 현상이 나타난다
- 많은 training dataset을 요구한다.
- Sequence modeling을 해야 한다(RNN, LSTM과 같은 모델이 많이 쓰임)
- Temporal Reasoning
- Focus on action Recognition

<br>

### Before Deeplearning

딥러닝이 등장하기 이전에는 Video task 에서는 ME(Motion Estimation)&MC(Motion Compensation) 기법을 이용했습니다.

ME을 이용해 Motion을 예측하고 MC를 이용해 프레임 interpolation과 같은 작업을 했었습니다. 

Video Recognition task에서는 주로 ME만을 사용했기에 ME 기술에 대해 알아 봅시다.

- Motion Estimation

  Motion Estimation을 이용해 Motion Vector를 구하는 것이 목표입니다.

  Motion Estimation의 대표 기술로는 block based matching 방식인 Block-Matching-Algorithm(BMA)와 Optical Flow가 있습니다.

  - BMA

    BMA는 ME를 할 때 한 프레임 이미지를 잘게 잘라줍니다.(e.g. 8x8, 16x16)

    > block 단위로 잘린 이미지

    ![](https://user-images.githubusercontent.com/31475037/60324891-f08c2e00-99c0-11e9-95a0-31f02af8c001.png)

    t-1번째 프레임의 블록을 기준으로 t번째 프레임에서 제일 비슷한 block을 찾아줍니다.

    찾는 기준은 Search Range라는 윈도우를 하나 정의한 후 그 크기 안에서 가장 비슷한 블록을 찾는 방식으로 진행됩니다.

    비슷함의 측정은 SAD(Sum of Absolute Difference)방식을 이용해 두 block이 얼마나 비슷한지 측정합니다.

    ![](https://user-images.githubusercontent.com/31475037/60324892-f08c2e00-99c0-11e9-8c8c-2ca719c7586e.png)

    > Motion Vector

    이를 통해 Motion Vector를 구해준 뒤 Motion Vector를 이용해 다음 task인 MC나 Video Recognition과 같은 task를 수행합니다.

    ![](https://user-images.githubusercontent.com/31475037/60324893-f08c2e00-99c0-11e9-8d2b-ee6d2f87d7b4.png)

  - Optical Flow

    Optical Flow는 다음과 같은 가정에 의해 진행이 됩니다.

    - 밝기 항상성 : 어떤 객체 상의 픽셀은 프레임이 바뀌어도 그 값이 변치 않음
    - 시간 지속성 : 영상 내의 움직임은 많지 않음
    - 공간 일관성 : 공간적으로 서로 인접한 점들은 동일한 객체에 속할 가능성이 높음

    이런 가정을 한 뒤, Motion Vector를 구하는데 있어 영상을 x축으로 미분하고, y축으로 미분하고, 시간축으로 미분해서 특정 식을 대입해서 Matrix를 푸는 방식입니다.

    대표적인 방식으로는 Lukas-Kanade방식이 있습니다.

    > 초음판 영상에서 Optical Flow를 이용해 Motion Vector를 구한뒤 표시

    ![](https://t1.daumcdn.net/cfile/tistory/190218454FBC82311F)

<br>

이제부터 소개드릴 기법은 딥러닝에 기반한 방법론들입니다.

### Large-scale Video Classification with CNN





<br>

[What is video understanding?](https://medium.com/syncedreview/video-understanding-is-a-new-vista-for-ai-13be04416f56)

[Optical Flow](https://paeton.tistory.com/entry/%EC%98%B5%ED%8B%B0%EC%B9%BC-%ED%94%8C%EB%A1%9C%EC%9A%B0-Optical-Flow)

<br>

