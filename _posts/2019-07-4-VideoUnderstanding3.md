---
layout: post
title: Video Understanding 3
tags: [Deeplearning, Video Understanding]
---

## Video Description

Video Description task는 CV(Computer Vision)과 NLP(Natural Language Processing) 두분야를 모두 사용하는 분야입니다.

이 task는 크게 두 부분으로 구성되어 있습니다.

video에 대해 이해하고, 이해를 바탕으로 문법적으로 정확하게 설명하는것입니다.



### Conv3D + Attention

```
Describing Videos by Exploiting Temporal Structure
Submitted on 25 April 2015
```

video description task에서 landmark와 같은 논문입니다.

논문에서는 3D CNN + LSTM을 기반으로 아키텍쳐를 설계를 했습니다.

특이사항으로는 pretrained 3D CNN을 사용해 성능을 올렸습니다.

LRCN의 구조와 거의 비슷한 encoder-decoder 구조입니다.

하지만 다음 2가지 사항이 다릅니다.

- 





![](http://blog.qure.ai/assets/images/actionrec/Larochelle_paper_high.png)

video description task에서 처음으로 attention 메카니즘을 사용한 논문입니다.

주요 contribution

- local spatiotemporal information의 포착에 대한 3D CNN-RNN encoder-decoder 구조 제안
- attention 메카니즘을 global context 포착에 대한 용도로 사용

<br>

[Deep Learning for Videos: A 2018 Guide to Action Recognition](http://blog.qure.ai/notes/deep-learning-for-videos-action-recognition-review)

[awesome-action-recognition](https://github.com/jinwchoi/awesome-action-recognition)

[video description review](https://arxiv.org/pdf/1806.00186.pdf)

<br>

