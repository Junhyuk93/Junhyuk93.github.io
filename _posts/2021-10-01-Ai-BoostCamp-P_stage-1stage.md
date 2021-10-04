---
layout: post
title:  "부스트캠프 AI Tech 2기 Level2 P_stage 1-stage detector "
date:   2021-10-02 15:30:10 +0530
categories: Object Detection 1-stage detector
tag: [CV, Object Detection, Detection, 1-stage]
---

# 1. stage Detectors

## 1.1 1 Stage Detector Background

### 2 stage detectors

- RCNN, FastRCNN, SPPNet, FasterRCNN ...
    - 1) Localization (후보 영역 찾기)
    - 2) Classification (후보 영역에 대한 분류)

- Limitation : 속도가 매우 느림

- Real World에서 응용 가능한 Object Detectors는 없을까?

![](https://i.imgur.com/7XJWGy6.png)

### 1 Stage Detectors

- Localization, Classification이 동시에 진행
- 전체 이미지에 대해 특징 추출, 객체 검출이 이루어짐 -> 간단하고 쉬운 디자인
- 속도가 매우 빠름 (Real-time detection)
- 영역을 추출하지 않고 전체 이미지를 보기 때문에 객체에 대한 맥락적 이해가 높음
    - Background error가 낮음
- YOLO, SSD, RetinaNet, ...

# 2. YOLO v1

## 2.1 Overview

### You Only Look Once History

- YOLO v1 : 하나의 이미지의 Bbox와 classification 동시에 예측하는 1 stage detector 등장
- YOLO v2 : 빠르고 강력하고 더 좋게
    - 3가지 측면에서 model 향상
- YOLO v3 : multi-scale feature maps 사용
- YOLO v4 : 최신 딥러닝 기술 사용
    - **BOF** : Bag of Freebies, **BOS** : Bag of Specials
- YOLO v5 : 크기별로 모델 구성
    - Small, Medium, Large, Xlarge

### 접근 전략

![](https://i.imgur.com/80jxM8H.png)

### YOLO 특징

- Region proposal 단계 X
- 전체 이미지에서 bounding box예측과 클래스를 예측하는 일을 동시에 진행
    - 이미지, 물체를 전체적으로 관찰하여 추론 (맥락적 이해 높아짐)

### Network
- GoogLeNet 변형
    - 24개의 convolution layer : 특징 추출
    - 2개의 fully connected layer : box의 좌표값 및 확률 계산
    
## 2.2 Pipeline
- 입력 이미지를 SxS 그리드 영역으로 나누기 (S=7)
- 각 그리드 영역마다 B개의 Bounding box와 Confidence score 계산 (B=2)

![](https://i.imgur.com/7EcbOix.png)

![](https://i.imgur.com/eASvYlC.png)

[YOLO 참고자료](https://docs.google.com/presentation/d/1aeRvtKG21KHdD5lg6Hgyhx5rPq_ZOsGjG5rJ1HP7BbA/pub?start=false&loop=false&delayms=3000&slide=id.g137784ab86_4_767)


## 2.3 Result

### 장점

- Faster R-CNN에 비해 6배 빠른 속도
- 다른 real-time detector에 비해 2배 높은 정확도
- 이미지 전체를 보기 때문에 클래스와 사진에 대한 맥락적 정보를 가지고 있음
- 물체의 일반화된 표현을 학습
    - 사용된 dataset의 새로운 도메인에 대한 이미지에 대한 좋은 성능을 보임


# 3. SSD

## 3.1 Overview

### YOLO 단점

- 7x7 그리드 영역으로 나눠 Bounding box prediction 진행
    - 그리드보다 작은 크기의 물체 검출 불가능

- 신경망을 통과하여 마지막 feature만 사용
    - 정확도 하락

### YOLO vs SSD

![](https://i.imgur.com/sfG5vYC.png)


### SSD 특징

- Extra convolution layers에 나온 feature map들 모두 detection 수행
    - 6개의 서로 다른 scale의 feature map 사용
    - 큰 feature map (early stage feature map)에서는 작은 물체 탐지
    - 작은 feature map (late stage feature map)에서는 큰 물체 탐지

- Fully connected layer 대신 convolution layer 사용하여 속도 향상
- Default box 사용 (anchor box)
    서로 다른 scale과 비율을 가진 미리 계산된 box 사용
    
## 3.2 Pipleline

### Network

- VGG-16(backbone) + Extra Convolution Layers
- 입력 이미지 사이즈 300x300

![](https://i.imgur.com/02agnH5.png)

### Multi-scale feature maps

![](https://i.imgur.com/daluEOH.png)

![](https://i.imgur.com/mZ2nW05.png)

- N_Bbox : Different scale per feature maps(S_min = 0.2, S_max = 0.9, m = 6)
    ![](https://i.imgur.com/8PL21nA.png)

- N_Bbox : Different aspect ratio
    ![](https://i.imgur.com/45NhtAY.png)

![](https://i.imgur.com/CY7n2Gg.png)

![](https://i.imgur.com/FoY9v4g.png)

![](https://i.imgur.com/YM3adnk.png)

### Multi-scale feature maps & Default Box

![](https://i.imgur.com/qRi3HcY.png)

### Training

- Hard negative mining 수행

- Non Maximum suppression 수행

### Loss

![](https://i.imgur.com/Jyi63es.png)


# YOLO Follow-up

## 4.1 YOLO v2

### Concept

- 3가지 파트에서 model 향상
    - Better : 정확도 향상
    - Faster : 속도 향상
    - Stronger : 더 많은 class 예측 (80->9000)

### Better
- Batch normalization
    - map 2% 상승

- High resolution classifier
    - YOLO v1 : 224x224 이미지로 사전 학습된 VGG 448x448 Detection 테스크에 적용
    - YOLO v2 : 448x448 이미지로 새롭게 finetuning
    - map 4% 상승

- Convolution with anchor boxes
    - Fully connected layer 제거
    - YOLO v1 : grid cell의 bounding box의 좌표 값 랜덤으로 초기화 후 학습
    - YOLO v2 : anchor box 도입
    - K means clusters on COCO datasets
        - 5개의 anchor box
    - 좌표 값 대신 offset 예측하는 문제가 단순하고 학습하기 쉬움
    - map 5% 상승

- Fine-grained features
    - 크기가 작은 feature map 은 low level 정보가 부족
    - Early feature map은 작은 low level 정보 함축
    - Early feature map을 late feature map에 합쳐주는 passthrough layer 도입
    - 26x26 feature map을 분할 후 결합

- Multi-scale training 
    - 다양한 입력 이미지 사용 {320,352,...,608}
    - != multi-scale feature map

### Faster

- Backbone model
    - GoogLeNet -> Darknet-19
- Darknet-19 for detection
    - 마지막 fully connected layer 제거
    - 대신 3x3 convolution layer로 대체
    - 1x1 convolution layer 추가
        - channel 수 125 (=5x(5+20))

### Stronger

- Classification 데이터셋(imageNet), detection 데이터셋(COCO) 함께 사용
    - Detection 데이터셋 : 일반적인 객체 class로 분류 ex) 개
    - Classification 데이터셋 : 세부적인 객체 class 로 분류 ex) 불독, 요크셔테리어
- "개", "요크셔테리어", 배타적 class로 분류하면 안된다.

- WordTree 구성(계층적인 트리)
    - Ex."요크셔테리어" = 물리적객체(최상위 노드) - 동물 - 포유류 - 사냥개 - 테리어(최하위 노드)
    - imageNet 데이터셋과 CoCo 데이터셋 합쳐서 구성 : 9418 범주

- ImageNet 데이터셋 : COCO 데이터 셋 = 4 : 1
    - Detection 이미지 : classification loss는 특정 범주에 대해서만 loss 계산
        ex. 개 이미지 : 물리적객체 - 동물 - 포유류 - 개 에 대해서 loss 계산
    - Classification 이미지 : classification loss만 역전파 수행 (IOU)

![](https://i.imgur.com/E9qBWS8.png)

## 4.2 YOLO v3

### Darknet-53

- Skip connection 적용
- Max pooling x, convolution stride 2 사용
- ResNet-101, ResNet-152와 비슷한 성능, FPS 높음

![](https://i.imgur.com/NAObA4f.png)

### Multi-scale Feature maps

- 서로 다른 3개의 scale을 사용(52x52, 26x26, 13x13)
- Feature pyramid network 사용
    - High-level의 fine grained 정보와 low-level의 semantic 정보를 얻음
    
# 5. RetinaNet

## 5.1 Overview

### 1 Stage Detector Problems


- Class imbalance
    - Positive sample(객체 영역) < negative sample(배경 영역)

- Anchor Box 대부분 Negative Samples(background)
    - 2 Stage detector의 경우 region proposal에서 background sample 제거(selective search, RPN) 
    - Positive / negative sample 수 적절하게 유지(hard negative mining)

### 5.2 Focal Loss

### Concept


- 새로운 loss function 제시 : cross entropy loss + scaling factor
- 쉬운 예제에 작은 가중치, 어려운 예제에 큰 가중치
- 결과적으로 어려운 예제에 집중

![](https://i.imgur.com/4ckzF4F.png)

### 사용

1) Object Detection에서 background와의 class imbalance 조정
2) Object Detection 뿐만 아니라 Class imbalance가 심한 Dataset을 학습할 때 이를 활용
    - Classification, Segmentation, Kaggle, etc

## Summary

![](https://i.imgur.com/Dh5uzkf.png)
