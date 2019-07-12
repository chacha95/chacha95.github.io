---
layout: page
title: Projects
subtitle: awesome projects!
---

# Project lists

## 딥러닝을 이용한 불교미술 채색

#### 불교미술 채색

기존에 사람이 수작업으로 채색하던 불교미술 그림에 대해, 딥러닝을 이용해 채색을 하는 프로젝트입니다.

핵심 채색 기능은 CycleGAN을 이용해 채색을 하였습니다.

![](https://user-images.githubusercontent.com/31475037/58854545-a2914c80-86d8-11e9-9d33-c3ce496a6abf.PNG)

#### 전처리

여러 실험 결과 딥러닝 네트워크에 들어갈 이미지를 이진화 및 노이즈 제거 연산을 하면 더 나은 결과를 보여줬다.
![](https://user-images.githubusercontent.com/31475037/58854670-f69c3100-86d8-11e9-9b36-4d64f5071d9e.PNG)

#### 딥러닝 채색

사이클 겐 모델을 사용하였으며,  불교 학술 문화원으로 받은 데이터를 input으로, Discriminator에 훈련시킬 이미지 데이터셋(실제 탱화)은 구글에 쿼리를 날려 수집하였습니다.

![](https://user-images.githubusercontent.com/31475037/58854783-4549cb00-86d9-11e9-8e2c-f866b4df6c7c.png)

CycleGAN은 Unpaired Dataset에 대한 Style transfer 기술입니다. 
![](https://user-images.githubusercontent.com/31475037/58854667-f56b0400-86d8-11e9-9ed1-5ec3fc49445f.PNG)

#### 후보정

후보정은 PyQt 및 OpenCV를 이용해 FMM 알고리즘 및 페인팅 툴을 이용해 보정을 하였습니다.

![](https://user-images.githubusercontent.com/31475037/58854785-467af800-86d9-11e9-89ff-8625988b6c49.png)

<br>

# 최종 결과물

![](https://user-images.githubusercontent.com/31475037/58854671-f734c780-86d8-11e9-92f2-5738c133b815.PNG)

![](https://user-images.githubusercontent.com/31475037/58854672-f7cd5e00-86d8-11e9-954b-507ff4a61a96.PNG)



### 개발환경

- PyQT 5
- Pytorch 0.3
- OpenCV 3.2
- Python 3.6
- Window

<br>

#### 프로젝트 페이지

[Slide](https://drive.google.com/uc?id=1mWrj_n7y_87R9GHe7puwFdtTGlbau4bt) | [Code](<https://github.com/chacha95/Colorization_for_BuddistArt>)

<br>

## SVM과 LBP를 이용한 특징점 검출

LBP로 이미지의 feature를 뽑고 이를 SVM을 통해 특징점인지 아닌지 학습시켜 특징점을 검출해내는 프로젝트입니다. 

기존 SIFT에 비해서 70%정도의 성능밖에 나오지 않지만 SVM의 전체적인 work flow에 대해 알 수 있는 프로젝트입니다.

#### DB 생성

SVM을 학습시키기 위한 DB를 만들기 위해 BSD500 이미지를 사용하였다. C++ 코드로 폴더 내 모든 이미지 파일을 읽어오고, 읽어온 이미지에서 SIFT를 이용해 Feature Point를 추출해 Vector에 좌표를 저장한다.  저장된 좌표의 LBP 벡터를 계산한다. LBP 벡터 추출에는 15 x 15 블록을 사용하였으며, 중간 값과 비교해 작으면 0, 크면 1을 부여하는 224차원의 벡터를 만들어낸다. SIFT Featrue Point의 벡터는 클래스 1(특징점)으로 분류시켜 train_Data.txt라는 파일에 저장된다. 저장 방식은“1 1:LBP[0] 2:LBP[1] 3:LBP[2] .... 224:LBP[223]”이다. 여기서 SVM 이진 분류기는 1클래스와 -1 클래스 두 개로 나뉘어 지는데, 1클래스는 Feature Point 클래스이고, -1은 Feature Point가 아닌 클래스이다. 이때 학습을 위해서는 -1 클래스 또한 만들어야 하는데, Feature Point가 아닌 좌표를 임의로 생성하며, 그 개수는 Feature Point의 개수와 같아야 한다.(같지 않으면 분류가 제대로 되지 않음을 확인했음)

#### SVM Train(svm_learn.exe)

SVM은 SVMlight를 사용하였으며, exe 파일로 되어있어, Data_set만 잘 만들어 주면 알아서 학습을 시켜준다.
SVMlight가 저장된 폴더에 들어가 cmd 상에서 svmlight.exe train_Data.txt명령어를 쳐주면, 자동으로 학습이 시작된다. 이후 SVM_model이라는 파일이 학습되어 나온다.

![](https://user-images.githubusercontent.com/31475037/58856936-e8e9aa00-86de-11e9-85b6-4275b9fefdff.PNG)

#### Test(test_extractor.cpp, svm_classify.exe, test_rendering.cpp)

Test는 BSD test 데이터셋 중 한 장을 골랐으며, test 이미지에서 모든 픽셀의 LBP 벡터를 구한다. 구해진 LBP 벡터는 test.txt 파일로 저장된다.
저장된 LBP 벡터는 학습된 SVM을 통과해 해당 값이 Feature Point인지 아닌지 판별하는 정확도 값을 추출해 낸다. 
추출된 정확도 값을 읽어 들여, test 이미지 상에서 렌더링 해준다. 여기서 정확도 값이 1일시 클래스 1, Feature Point라는 소리임으로 test 이미지에 렌더링 해준다.

work flow

![](https://user-images.githubusercontent.com/31475037/58856937-e8e9aa00-86de-11e9-9917-a0154f6138a9.PNG)

<br>

> origin

![](https://user-images.githubusercontent.com/31475037/58856938-e9824080-86de-11e9-92e5-df9ba9634101.PNG)

> SIFT

![](https://user-images.githubusercontent.com/31475037/58856939-e9824080-86de-11e9-9be8-d73298e63b26.PNG)

> LBP_SVM

![](https://drive.google.com/uc?id=1nSL6vnWs1xpkHf2RbTSV5bhdUQ_YHkjg)

<br>

> origin

![](https://user-images.githubusercontent.com/31475037/58856941-e9824080-86de-11e9-8101-6022bded0308.PNG)

> SIFT

![](https://drive.google.com/uc?id=1MUQt0jfIV7n0IuAx_-cl7cn0n9AjsNHP)

> LBP_SVM

![](https://user-images.githubusercontent.com/31475037/58856944-ea1ad700-86de-11e9-8ea7-bf452b7e026d.PNG)

#### 개발환경

- C++
- OpenCV
- SVMLight
- Window

<br>

#### 프로젝트 페이지

 [Code](<https://github.com/chacha95/OpenCV_Project/tree/master/SVM>)

<br>

## 유니티를 이용한 RTLS(Real Time Location System) 구현

이 프로젝트는 워터파크 이용객 재현 어플리케이션입니다. 워터파크 이용객의 위치 정보를 비콘에서 수신하여 중앙 서버에서 해당 정보를 받아 저장해 놓습니다. 

> 워터파크 미니 맵

<center><img src = "https://user-images.githubusercontent.com/31475037/58927389-505c3400-8789-11e9-92f9-0edaa9e32d92.png" width="70%">
</center>

워터파크가 폐장 한 뒤 어플리케이션(클라이언트)에서 서버에 TCP/IP 연결을 요청하고, 현재 어플리케이션 상의 시간과 방위값을 서버에 보내줍니다.

> 서버 클라이언트 통신

<center><img src = "https://user-images.githubusercontent.com/31475037/58927382-489c8f80-8789-11e9-9bb2-cb7583d41e2e.png" width="90%" height="80%">
</center>


 서버는 해당 시간대에 해당하는 사용자의 위치 정보 데이터를 클라이언트에 보내주게 됩니다. 

<center><img src = "https://user-images.githubusercontent.com/31475037/58927379-463a3580-8789-11e9-92a0-5d8710a91c5e.png" width="70%" height="70%">
    <br><caption></caption>
</center>

클라이언트는 받은 데이터를 기반으로 사용자 객체를 그려주고, 사용자들의 정보에 따른 이용자 위치 찾기, 미니맵상에서 정보 띄우기, 사용자 정보의 on/off 등의 기능을 수행합니다.



#### 개발환경

- C#
- Unity
- Python
- Numpy
- Window

<br>

#### 프로젝트 페이지

[Slide](https://drive.google.com/uc?id=1Wcu35Fp16rrOiOjuFCLs2BohZsKtxlLx) | [Code](<https://github.com/chacha95/RTLS-Real-time-Location-System->)

<br>

## OpenGL을 활용한 행성 시뮬레이션

태양계를 OpenGL을 활용해 3D로 구현해 행성을 관측하는 시뮬레이션을 구현합니다. Timer 콜백 함수를 이용해 자전과 공전주기를 설정해줍니다.

구성은 scene1과 scene2로 구성되어 있으며, scene1일 때에는 특정 행성의 시점에서 태양을 바라보는 장면에서 행성들이 공전과 자전을 하는 장면을 연출합니다.

> Scene 1

![](https://user-images.githubusercontent.com/31475037/58870789-d03bbd00-86fb-11e9-9942-bb7a498c4405.PNG)

Scene2에서는 선택된 행성의 상세 정보를 표시해줍니다.

> Scene2

![](https://user-images.githubusercontent.com/31475037/58870793-d0d45380-86fb-11e9-864d-e53b7df70f38.PNG)

> Scene1에서 Scene2로

화면안의 빨간색 박스 안을 마우스로 클릭시 scene2로 넘어가게 설정 해놓았고, scene2에서 esc를 누를시 Scene1으로 되돌아 오도록 했습니다.

![](https://user-images.githubusercontent.com/31475037/58870791-d0d45380-86fb-11e9-8bc7-93bf14b3ccce.PNG)



이 외의 조작법은 다음과 같습니다.

![](https://user-images.githubusercontent.com/31475037/58870790-d0d45380-86fb-11e9-90ac-61cd54e77292.PNG)



#### 개발환경

- C++
- OpenGL
- Window

<br>

#### 프로젝트 페이지

  [Code](<https://github.com/chacha95/Computer_Graphics>)

