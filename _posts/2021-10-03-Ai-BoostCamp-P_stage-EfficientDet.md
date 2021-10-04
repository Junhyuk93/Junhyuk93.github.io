---
layout: post
title:  "부스트캠프 AI Tech 2기 Level2 P_stage EfficientDet "
date:   2021-10-03 15:30:10 +0530
categories: Object Detection EfficientDet
tag: [CV, Object Detection, Detection, EfficientDet]
---

EfficientDet

# 1. Efficient in Object Detection

## 1.1 Model scaling



### Model Scaling

![](https://i.imgur.com/JuUcDAR.png)




### Width Scaling

![](https://i.imgur.com/81NGBM4.png)


### Depth Scaling

![](https://i.imgur.com/Q0VXnSi.png)


## 1.2 등장 배경

- 더 높은 정확도와 효율성을 가지면서 ConvNet의 크기를 키우는 방법(scale up)은 없을까?
> "EfficientNet팀의 연구는 네트워크의 폭(width), 깊이(depth), 해상도(resolution) 모든 차원에서의 균형을 맞추는 것이 중요하다는 것을 보여주었다. 그리고 이러한 균형은 각각의 크기를 일정한 비율로 확장하는 것으로 달성할 수 있었다."


## 1.3 무엇을 ? - Accuracy & Efficiency

![](https://i.imgur.com/ySkVfHo.png)


# 2. EfficientNet

## 2.1 등장 배경

- 파라미터 수가 점점 많아지고 있는 모델들
- ConvNet은 점점 더 커짐에 따라 정확해지고 있음

- 점점 빠르고 작은 모델에 대한 요구 증가
- 효율성과 정확도의 trade-off를 통해 모델 사이즈를 줄이는 것이 일반적
- 하지만 큰 모델에 대해서는 어떻게 모델을 압축시킬지가 불분명함
- 따라서 이 논문은 아주 큰 SOTA ConvNet의 efficiency를 확보하는 것을 목표로 함
- 그리고 모델 스케일링을 통해 이 목표를 달성함


## 2.2 Scale Up

### Baseline

![](https://i.imgur.com/nsrDVpj.png)


### Width Scaling

![](https://i.imgur.com/fHtD41h.png)


### Depth Scaling

![](https://i.imgur.com/NHmV4RJ.png)


### Resolution Scaling

![](https://i.imgur.com/kYaIT2D.png)


### Scale up

![](https://i.imgur.com/q1s38ZQ.png)


## 2.3 Accuracy & Efficiency

### Better Accuracy & Efficiency

![](https://i.imgur.com/oEZu4q4.png)


### Better Accuracy & Efficiency - Notation

![](https://i.imgur.com/wJOyLgY.png)


### Observation 1

- 네트워크의 폭, 깊이, 혹은 해상도를 키우면 정확도가 향상된다.
- 하지만 더 큰 모델에 대해서는 정확도 향상 정도가 감소한다.

![](https://i.imgur.com/FeovRVr.png)


### Observation 2

- 더나은 정확도와 효율성을 위해서는, ConvNet 스케일링 과정에서
- 네트워크의 폭, 깊이, 해상도의 균형을 잘 맞추는 것이 중요하다.

![](https://i.imgur.com/wcGO2vJ.png)


### Compound Scaling Method

![](https://i.imgur.com/ABYKYeT.png)

![](https://i.imgur.com/EcEogRe.png)


## 2.4 EfficientNet

### EfficientNet-B0

- MnasNet에 영감을 받음
- ACC(m) * (FLOPS(m)/T)^W 를 최적화 목표
- Accuracy와 FLOPs를 모두 고려한 Neural Network을 개발함
- Nas 결과, EfficientNet-B0

### Step 1

![](https://i.imgur.com/NvPxbig.png)


### Step 2

![](https://i.imgur.com/Kg6YFg9.png)


# 3. EfficientDet



## 3.1 등장 배경

![](https://i.imgur.com/mSfl8pz.png)


### Detection

![](https://i.imgur.com/H3svq9H.png)


### Object Detection은 특히나 속도가 중요하다!

- 모델이 실생활에 사용되기 위해서는 모델의 사이즈와 대기 시간에 제약이 있기 때문에, 모델의 사이즈와 연산량을 고려해 활용 여부가 결정됨.

- 이러한 제약으로 인해 Object Detection 에서 Efficiency가 중요해지게 됨.

### 그 동안 있었던 많은 시도들..

- 1 stage model
    - YOLO, SSD, RetinaNet ...
- Anchor free model
    - CornerNet ...

- 하지만 Accuracy 가 낮음.

### Motivation

- 자원의 제약이 있는 상태에서 더 높은 정확도와 효율성을 가진 detection 구조를 만드는 것이 가능할까?


![](https://i.imgur.com/hKFxpUu.png)


## 3.2 Challenge

### 1) Efficient multi-scale feature fusion

- Hint - Neck

- In Neck, Simple Summation 

![](https://i.imgur.com/YoGB4Vk.png)

기존의 FPN을 기반으로한 feature pyramid Network에서는 High level + low level feature map을 할 때 그냥 channel 과 resolution을 맞춰 단순 합을 했다는 점이다.


### 2) Model Scaling

- Previous work focus on large backbone & image size


---

### Efficient multi-scale feature fusion

- EfficientDet 이전에는 multi-scale feature fusion을 위해 FPN, PANet, NAS-FPN 등 Neck 사용

- 하지만 대부분의 기존 연구는 resolution 구분 없이 feature map을 단순 합

- 서로 다른 정보를 갖고 있는 feature map을 단순합 하는게 맞을까?

- 따라서 이 문제를 다루기 위해 EfficientDet 팀은 각각의 input을 위한 학습 가능한 웨이트를 두는 Weighted Feature Fusion 방법으로 BiFPN(bi-directional feature pyramid network)를 제안.

![](https://i.imgur.com/Bx8VroL.png)


- 모델의 Efficiency 를 향상시키기 위해 다음과 같은 cross-scale connections 방법을 이용
    - 하나의 간선을 가진 노드는 제거
    - output 노드에 input 노드 간선 추가
    - 양방향 path 각각을 하나의 feature Layer로 취급하여, repeated blocks 활용

![](https://i.imgur.com/SwRe8IE.png)


- EfficientDet은 여러 resolution의 feature map을 가중 합
- FPN의 경우 feature map의 resolution 차이를 Resize를 통해 조정한 후 합

![](https://i.imgur.com/dmevgp9.png)


- BiFPN의 경우 모든 가중치의 합으로 가중치를 나눠줌.
- 이 때 가중치들은 ReLU를 통과한 값으로 항상 0 이상
- 분모가 0이 되지 않도록 아주 작은 값 e를 더해줌

![](https://i.imgur.com/K3Jtjea.png)


---

### 2) MOdel Scaling

- 더 좋은 성능을 위해서는 더 큰 backbone 모델을 사용해 dewtector의 크기를 키우는 것이 일반적임
- EfficientDet은 accuracy와 efficiency를 모두 잡기 위해, 여러 constraint를 만족시키는 모델을 찾고자 함.
- 따라서 EfficientNet과 같은 compound scaling 방식을 제안.

- EffcientNet B0 ~ B6을 backbone 으로 사용
- BiFPN network
    - 네트워크의 width(=#channels) 와 depth(=#layers)를 compound 계수에 따라 증가시킴

![](https://i.imgur.com/cLEp1cv.png)


- Box/class prediction network
    - Width는 고정, depth를 다음과 같은 식에 따라 증가

![](https://i.imgur.com/n1sGnvb.png)

- input image resolution 
    - Resolution으 ㄹ 다음과 같이 선형적으로 증가  

![](https://i.imgur.com/hyToKhU.png)
