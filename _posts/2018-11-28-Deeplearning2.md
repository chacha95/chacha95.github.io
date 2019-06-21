---
layout: post
title: Neural Network
tags: [deeplearning]
---

## Neural Network

### Perceptron

> 어떻게 하면 기계가 학습해서 풀 수 있게 할까?

어떻게 하면 기계가 학습해서 다음과 같은 문제를 풀 수 있을까요? 라는 질문에 답하기 위해 제안된 모델이 바로 Perceptron입니다.

> AND NAND OR XOR 문제

<center><img src="https://user-images.githubusercontent.com/31475037/59172735-b5969780-8b84-11e9-8d3a-8445513adfc2.png" width="100%"></center>

> Perceptron

퍼셉트론은 인공신경망(Artificial Neural Network)의 한 종류료서, 1957년에 코넬 항공 연구소의 Rosenblatt에의해 만들어졌습니다. 이것은 가장 간단한 feedfoward 네트워크로 볼 수 있습니다. (feedforward는 앞으로 순차적으로 feed를 주는 시스템을 말합니다)

Perceptron은 input 벡터 [x1, x2]가 있고 이를 weight 벡터 [w1, w2]와 dot product를 통해 scalar 값만을 보내고, activation function(sigmoid)를 통해 나오는 값이 출력 값이 됩니다.

여기서 기계가 출력 값을 정답으로 배출하도록 weight 벡터를 학습(조정)하는것이 목표입니다.

**손으로 weight를 조정해서 and나 or 문제를 풀어보면 이해에 도움이 많이 됩니다!**

<center><img src="https://user-images.githubusercontent.com/31475037/59171562-651c3b80-8b7e-11e9-9ede-2b6bfd8e67e6.png" width="100%"></center>

이를 통해 Perceptron은 and나 or 문제를 풀 수 있습니다.




> 그렇다면 XOR문제를 Perceptron이 해결 가능할까요?

안타깝게도 아무리 weight를 조정해봐도 XOR 문제를 풀 수가 없습니다.


<center><img src="https://user-images.githubusercontent.com/31475037/59157949-5dad5180-8aee-11e9-861f-1b07de6e436d.png" width="90%"></center>

이런 문제를 해결하기 위해 제안된 것이 바로 MLP입니다.

### MLP(Multi Layer Perceptron)

그렇게 나온 모델이 바로 MLP입니다.

MLP는 input layer와 output layer 사이에 하나 이상의 hidden layer를 추가하여 학습하는 모델입니다.

> XOR 문제를 풀 수 있음

MLP 모델은 기존의 단일 Perceptron이 해결하지 못한 XOR 문제를 성공적으로 풀었습니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59157950-5dad5180-8aee-11e9-8dde-bf5e6d623d37.png" width="90%"></center>

> 일반적인 MLP 구조

MLP 구조에서 hidden layer를 깊게(많이) 쌓은 네트워크 모델이 바로 우리가 일반적으로 많이 알고 있는 Deep Neural Network 입니다.

아래 그림의 hidden layer에서 처럼 다음 layer의 모든 뉴런과 다 연결 되있는 layer를 fully cunnected layer라고 부릅니다.

<center><img src="https://user-images.githubusercontent.com/31475037/59157951-5dad5180-8aee-11e9-8bef-b367921c7343.png" width="90%"></center>



### Universal Approximation Theorem(UAT)

> 왜 Neural Network가 각광을 받는가?

Neural Network는  Universal Approximation(만능 근사)를 할 수 있습니다.

**Universal Approximation란?**

우리가 뉴럴 네트워크를 인공지능의 두뇌로 사용할 수 있는 것은 어떤 종류의 입력이든지 원하는 출력을 내놓도록 훈련시킬 수 있다는 믿음이 있기 때문입니다.

이처럼 Neural Netork의 weight를 조절하면 우리가 원하는 함수를 근사하여 출력 시킬 수 있다는 이론입니다.

아래 링크는 UAT를 시각적으로 체험할 수 있는 페이지입니다.

[Universal Approximation](http://neuralnetworksanddeeplearning.com/chap4.html)

<br>

**읽어볼만한 글**

[Perceptron](https://www.youtube.com/watch?v=xMRKQBbHOzA&list=PL6ip5tgLI7PcStXTz8CRMhNWmT8M0dAWO&index=2)

[Neural Net의 역사](http://solarisailab.com/archives/1206)

[Neural Net의 역사2](https://jinseob2kim.github.io/deep_learning.html)

**참고 강의**

[CS231n](https://www.youtube.com/playlist?list=PL3FW7Lu3i5JvHM8ljYj-zLfQRF3EO8sYv)

<br>