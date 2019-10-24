---
layout: post
title: Architecture Design of Efficeint DL2
tags: [Efficeint DL]
use_math: true
---

### NetSocre

기존의 대부분의 연구들은 모델의 accuracy(정확도)를 높이는 것을 목표로 모델을 설계했습니다. 그러나 실제 적용, 특히 모바일과 같은 edge computing device에서는 accuracy뿐만 아니라 complextiy(계산량) 또한 작아야 합니다. 그렇기에 accuracy와 complexity사이의 균형을 맞추는 모델 설계는 주목받는 연구 분야가되었습니다.

**Information Density**

가장 대중적으로 사용되던 방식은 information density를 측정하는 방식입니다. 하지만 이 방식은 parameter 수는 반영하지만, complexity를 반영하진 않기 때문입니다. parameter 수가 적더라도 complexity가 높은 모델이 존재하기 때문에 새로운 metric이 필요했습니다.

> information density의 정의

![](https://user-images.githubusercontent.com/31475037/67454867-ba0caa00-f666-11e9-9d46-4096fb5bb867.PNG)

**NetScore**

NetScore는 accuracy, complextiy(computational complextiy), parameter(network complextiy) 세가지를 고려한 metric입니다. Metric에선 log로 식을 감싸고, 가장 중요한 factor로써 accuracy를 둠으로써, accuracy를 최대한 보전하면서, Efficeint한 모델을 설계하는 것이 목적인 metric 입니다.

log로 식을 감싼 이유는 linear하게 하면 결과 값의 차이가 너무 커져 log scale을 사용했습니다.

> NetScore

![](https://user-images.githubusercontent.com/31475037/67455216-f68cd580-f667-11e9-8a54-c0ac7d057d0e.PNG)



이러한 NetScore 결과로 비교해 봤을 때, SqueezeNext가 제일 좋은 성능을 보였습니다.

> NetScore 결과

![](https://user-images.githubusercontent.com/31475037/67455287-2b009180-f668-11e9-81a2-a6061263010e.PNG)



### SqueezeNext





[PR-144](https://www.youtube.com/watch?v=WReWeADJ3Pw&t=930s)에서 잘 설명되어 있습니다.

### MobileNet-V2





[PR-108](https://www.youtube.com/watch?v=mT5Y-Zumbbw&t=563s)에서 잘 설명되어 있습니다.

### ShuffleNet-V2





[PR-120](https://www.youtube.com/watch?v=lrU6uXiJ_9Y)에서 잘 설명되어 있습니다.

<br>

