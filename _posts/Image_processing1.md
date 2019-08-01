---
layout: post
title: Interpolation(보간)
tags: [Image Processing]
---

## Interpolation

> 영상 interpolation이란?

영상처리에서 interpolation이란 원 영상을 확대 축소 시 발생할 수 있는 점들에 대해 주변 픽셀들의 정보를 취합해 새로운 점을 만들어 내는 알고리즘을 말합니다.

<br>

> Nearest Neighbor Interpolation

가장 가까운 화소값을 사용해 보간을 해준는 방식입니다.

계산이 빠르지만, 계단현상(aliasing artifact)가 많이 나타납니다.

<center><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/Interpolation-nearest.svg/220px-Interpolation-nearest.svg.png"></center>
<br>

> Bilinear Interpolation

인접한 4개 화소의 화소값과 거리비를 사용하여 결정해주는 방식입니다.

<center><figure class="second">
	<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Interpolation-bilinear.svg/220px-Interpolation-bilinear.svg.png">
	<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/BilinearInterpolation.svg/220px-BilinearInterpolation.svg.png">
</figure></center>
<br>


> Bicubic Interpolation

인접한 16개의 화소의 화소값과 거리에 따른 가중치(weight)의 곱을 사용하여 결정합니다

<center><img src="https://user-images.githubusercontent.com/31475037/58941486-db075800-87b6-11e9-9c7d-43c790525a52.png" width="50%"></center>
<center><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f5/Interpolation-bicubic.svg/220px-Interpolation-bicubic.svg.png"></center>
<br>

> 그래프 모형

<center><img src = "https://upload.wikimedia.org/wikipedia/commons/thumb/9/90/Comparison_of_1D_and_2D_interpolation.svg/512px-Comparison_of_1D_and_2D_interpolation.svg.png" width="65%"></center>
<br>