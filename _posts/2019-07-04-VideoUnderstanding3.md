---
layout: post
title: Video Understanding 3
tags: [Deeplearning, Video Understanding, 영상처리]
---

## I3D(Inflated 3D ConvNet)



[Quo Vadis, Action Recognition? A New Model and the Kinetics Dataset(CVPR 2017)](https://arxiv.org/abs/1705.07750) 논문은 action classification 분야의 approach2 분야인 two-stream method에서는 backbone이 되는 모델 중 하나입니다.

해당 논문에서는 action classification을 위한 새로운 데이터셋인 Kinetics 데이터셋을 만들고 이를 기반으로 I3D를 학습 시켰습니다.

논문에서는 I3D의 방법과 기존의 방법들에 대해 소개하면서 장단점을 비교 했습니다.

<br>

### Action Classification Architecture

**The old 1 : ConvNet + LSTM**

Image classification에서 성공했던 방법론을 다시 사용했습니다. 다만 sequntal한 video 데이터셋의 특성을 고려해 RNN계열인 LSTM 네트워크를 추가하였습니다.

CNN을 이용해 프레임마다 feature를 추출하고, 추출된 feature vector를 Decoder인 LSTM에 넣어주는 구조입니다. 논문에선 spirit of bag of words를 이미지 task에서 쓰였다고 말하고 있습니다. 

하지만 이러한 구조가 temporal structure를 무시하는 구조라 주장합니다.

논문에서는 기존 구조에서 BN(Batch Normalization)을 추가해 실험에 사용했습니다.

입력 비디오는 25fps 영상에서 1/5만큼 subsampling합니다. (사실상 5fps로 영상을 만든다는 의미)

**The old 2 : 3D ConvNets**

3D ConvNets은 video modeling에서 가장 자연스러운 선택처럼 보입니다. 3D Conv를 이용해 spatiotemporal 정보를 잘 취득할 수 있습니다.

하지만 한 차원이 늘어난 만큼 학습해야할 파라미터가 너무 많아 학습이 힘들다는 단점이 있습니다.

3D Conv를 사용하면 최소한의 성능은 보장되지만, State-of-the-art 기술들에 비하면 부족합니다.

해당 논문에서는 입력 영상을 16-frame, 112x112 pixel 영상으로 만들어 넣어줬습니다.

**The old 3 : Two-Stream Net**



<br>

### Two-Stream Inflated 3D ConvNet

<br>

[awesome-action-recognition](https://github.com/jinwchoi/awesome-action-recognition)

<br>

