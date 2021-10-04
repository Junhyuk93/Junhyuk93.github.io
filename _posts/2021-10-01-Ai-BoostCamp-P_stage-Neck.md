---
layout: post
title:  "부스트캠프 AI Tech 2기 Level2 P_stage Neck "
date:   2021-10-01 15:30:10 +0530
categories: Object Detection Neck
tag: [CV, Object Detection, Detection, Neck]
---



# Neck

## FPN, PANet, DetectoRS, BiFPN, NasFPN and AugFPN

## 1. Neck

## 1.1 Overview

### Neck은 무엇인가?

![](https://i.imgur.com/QMLlNRH.png)

backbone의 마지막 feature map 을 사용한다 -> why?

![](https://i.imgur.com/s6seQkF.png)


중간 중간의 feature map 도 사용해보자! -> Neck의 발전

---

### Neck은 왜 필요한가?

![](https://i.imgur.com/8RM0TUr.png)

Neck이 없다면 최종적으로 backbone을 통과한 같은 크기의 feature map 에서 다양한 크기의 객체를 예측해야 한다.

하지만 만약에 여러 크기의 feature map을 사용한다면, 각각의 feature map에서 다양한 크기를 대응할 수 있다면 ROI head 에서 보는 feature가 다양해진다.

일반적으로 작은 feature map 일수록 큰 범위를 보고 큰 feature map 일수록 작은 범위를 본다.

high level의 feature map만 사용한다면 작은 객체를 포착할 수 없는 문제가 생기고 이를 해결 하기 위해 다양한 feature map을 사용하자 라는 아이디어가 제안되었다.


> ### **다양한 크기의 객체를 더 잘 탐지하기 위해서 Neck을 사용한다.**

![](https://i.imgur.com/d6YAvTo.png)

Neck 없이 바로바로 뽑아서 region 을 해도 되지 않나? -> Neck을 통한 localization 정보 교류 중요성

> ### 하위 level의 feature는 semantic이 약하므로 상대적으로 semantic이 강한 상위 feature와의 교환이 필요

![](https://i.imgur.com/IappHQN.png)


## 1.2 Feature Pyramid Network (FPN)

### 기존의 아이디어들

![](https://i.imgur.com/gJyQoOq.png)

![](https://i.imgur.com/IGB916a.png)

기존의 방법들은 high level의 semantic의 정보가 교환되지 않는다는 한계점이 존재

### FPN

- high level에서 low level로 semantic 정보 전달의 필요성을 깨닫음.
- 따라서 top down path way 추가
    - Pyramid 구조를 통해서 high level 정보를 low level에 순차적으로 전달
        - Low level = Early stage = Bottom
        - Hoigh level = Late stage = Top

![](https://i.imgur.com/BZFfc2I.png)

### Bottom-up

![](https://i.imgur.com/EY73BOx.png)


### Top-down

![](https://i.imgur.com/aBQYEZt.png)



### Lateral connections

![](https://i.imgur.com/daUnhLd.png)

![](https://i.imgur.com/CYUcAdf.png)

bottom-up 과정에서 나온 feature map은 chnnel이 부족하고 top-down 과정에서 나온 feature map은 w 와 h 가 부족하다.

따라서 bottom-up 과정에서 나온 feature map은 1x1 conv를 수행해 채널을 맞춰주고, top-down과정에서 나온 feature map은 Upsampling을 통해 w와 h를 맞춰준다. 

이를 통해서 두 가지 결과를 더해준다.

![](https://i.imgur.com/1eaGHmp.png)

![](https://i.imgur.com/jdsPB6i.png)

#### Contribution
- 여러 scale의 물체를 탐지하기 위해 설계
- 이를 달성하기 위해서는 여러 크기의 feature를 사용해야할 필요가 있음

#### summary
-  Bottom up(backbone)에서 다양한 크기의 feature map 추출
-  다양한 크기의 feature map의 semantic을 교환하기 위해 top-down을 사용.


#### Code
- Build laterals : 각 feature map 마다 다른 채널을 맞춰주는 단계

![](https://i.imgur.com/fAXEHNr.png)


- Build Top-down : chnnel을 맞춘 후 top down 형식으로 feature map 교환

![](https://i.imgur.com/xwwS7uq.png)

- Build ouputs : 최종 3x3 convolution 을 통과하여 RPN으로 들어갈 feature 완성

![](https://i.imgur.com/yH5ey6f.png)

 ## 1.3 Path Aggregation network (PANet)
 
![](https://i.imgur.com/ggH8TJy.png)

FPN 문제점 -> Low level의 feature가 high level feature에 제대로 전달할 수 없음.

![](https://i.imgur.com/xejV4wN.png)

Bottom-up 을 두번 사용하여 low level의 feature를 다시한번 전달하자 - PANet의 Contribution

![](https://i.imgur.com/NmoXq9m.png)

ROIAlign = ROI Pooling 으로 이해할 것.

ROI의 경계에 대해서 학습이 어려움.

하나의 feature map에서 projection을 하는게 아니라 모든 feature map으로부터 ROI projection을 하여 이후 ROI Pooling을 하고 fc layer를 만들어 max pooling을 해서 최종적으로 fc2 layer를 만들어내자! -> Adaptive Feature Pooling의 Contribution

#### Code

- FPN : Top-down에 3x3 convolution layer 통과하는것 까지 동일

![](https://i.imgur.com/wKy3leA.png)


- Add bottom-up : FPN을 통과한 후, bottom-up path를 더해줌

![](https://i.imgur.com/7i4BP5N.png)


- Build outputs : 이후 FPN과 마찬가지로 학습을 위해 3x3 convolution layer 통과

![](https://i.imgur.com/p72HPm2.png)


## 2.1 DetectoRS


### Motivation

- Looking and thinking twice
    - Region proposal networks (RPN)
    - Cascade R-CNN

### 주요 구성

- Recursive Feature Pyramid(RFP)
- Switchable Atrous Convolution(SAC)

![](https://i.imgur.com/b9I1m0o.png)


### Recusive Feature Pyramid (RFP)

![](https://i.imgur.com/kKRBNXa.png)

FLOPs 가 많이 증가하여 학습속도가 느리다는 단점이 있음.


![](https://i.imgur.com/f3dNcPG.png)

![](https://i.imgur.com/ftaLKLq.png)

![](https://i.imgur.com/Z0JIjgG.png)

ASPP ? 여러가지 receptive field를 만들어 concat함.

### Atrous Convolution

![Convolution](https://i.imgur.com/JoWV6jK.gif) Convolution

- 2D Convolution
    - kernel size = 3x3
    - Stride = 2
    - Padding = 1
    
    **Recetive filed : 3 x 3**

![Atrous Convolution](https://i.imgur.com/nVcj2UZ.gif) Atrous Convolution

- 2D Convolution
    - kernel size = 3x3
    - **dilation rate of 2**
    - Stride = 1
    - Padding = 0

    **Recetive filed : 5 x 5**
    
## 2.2 Bi-directional Feature Pyramid (BiFPN)


### Pipeline

![](https://i.imgur.com/oz9aLn2.png)

### Weighted Feature Fusion

- FPN과 같이 단순 summation을 하는 것이 아니라 각 feature별로 가중치를 부여한 뒤 summation
- 모델 사이즈의 증가는 거의 없음
- feature별 가중치를 통해 중요한 feature를 강조하여 성능 상승

![](https://i.imgur.com/skB4sbt.png)

![](https://i.imgur.com/TDpxBwq.png)

## NASFPN

![](https://i.imgur.com/vhcne4k.png)

### Architecture

![](https://i.imgur.com/1Xlmgop.png)


### Search 

![](https://i.imgur.com/GG2OwZI.png)

### 단점

- COCO dataset, ResNet 기준으로 찾은 architecture, 범용적이지 못함
    - Parameter가 많이 소요

- High search cost
    - 다른 Dataset이나 backbone에서 가장 좋은 성능을 내는 architecture를 찾기 위해 새로운 search cost

## 2.4 AugFPN

### Overview

![](https://i.imgur.com/gJsE0Y7.png)


- Problems in FPN
    - 서로 다른 level의 feature간의 semantic 차이
    - Highest feature map의 정보 손실
    - 1개의 feature map에서 RoI 생성

### Residual Feature Augmentation

![](https://i.imgur.com/QkEFFQY.png)

- Ratio-invariant Adaptive Pooling
    - 다양한 scale의 feature map 생성
    - 256_channels
    

![](https://i.imgur.com/QLFAXJz.png)

- 동일한 size로 upsampling
- N 개의 feature에 대해 가중치를 두고 summation 하는 방법
- 3개의 feature map을 Concat 하고 N x ( 1 x h x w )의 값을 구함
- 이 때 N x ( 1 x h x w )은 spatial weight를 의미
- N x ( 1 x h x w )를 각 N개의 feature에 곱해 가중 summation

### Soft RoI Selection

- FPN과 같이 하나의 feature map에서 RoI를 계산하는 경우 sub-optimal
- 이를 해결하기 위해 PANet에서 모든 feature map을 이용했지만, max pool하여 정보 손실 가능성
- 이를 해결하기 위해 Soft RoI Selection을 설계

![](https://i.imgur.com/H5616yz.png)

- 모든 scale feature에서 RoI projection 진행 후 RoI pooling
- channel-wise 가중치 계산 후 가중 합을 사용
- PANet의 max pooling을 학습, 가능한 가중 합으로 대체