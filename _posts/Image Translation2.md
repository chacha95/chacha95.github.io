---
layout: post
title: After CycleGAN
tags: [Image Translation]
use_math: true
---

이번 포스트에서는 CycleGAN 이후 나온 CycleGAN 모델을 기반으로 한 Image Translation 연구에 대해 알아보겠습니다.

할 수 있을까? 논문도 조낸 읽어야 될텐데 시발.... 좆같다 4편을 읽어야하넹

## UNIT

논문 요약
\* unsupervised 이미지 변환의 대표적인것은 CycleGAN
\* CycleGAN의 경우, object의 변화가 불가능한것이 단점
\* UNIT의 경우 이를 일부 해결
\* 전체적인 network구조는 CycleGAN과 비슷

구조
\* VAE + GAN 구조

해결방식
\* Encoder, Generator간 weight들을 공유
\* Discriminator의 receptive field를 넓힘

Issue
\* 저자의 학습데이터(고양이 <-> 호랑이)의 경우, center에 모두 얼굴이 나타나도록 preprocessing을 진행한뒤 실험함

## MUNIT





##  DRIT





## U-GAT-IT



## FunIT





<br>

**참고 강의**

[NVIDIA NCSOFT](https://www.youtube.com/watch?v=Ko31fYGT20Y)