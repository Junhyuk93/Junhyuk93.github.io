---
layout: post
title:  "부스트캠프 AI Tech 2기 Level2 P_stage Advanced Object Detection 1 "
date:   2021-10-07 15:30:10 +0530
categories: Advanced Object Detection 1
tag: [CV, Object Detection, Detection, Cascade RCNN, Swin, Transformer, ViT]
---


Advanced Object Detection 1

# 1. Cascade RCNN

## 1.1 Contribution

![](https://i.imgur.com/GVnr1WY.png)

## 1.2. motivation

![](https://i.imgur.com/w1SIrsN.png)

![](https://i.imgur.com/XLsmhL3.png)

- IoU threshold에 따라 다르게 학습되었을 때 결과가 다름
- Input IoU가 높을수록 높은 IoU threshold에서 학습된 model이 더 좋은 결과를 냄

![](https://i.imgur.com/2lR2o9s.png)
 
- IoU threshold에 따라 다르게 학습되었을 때 결과가 다름
- 전반적인 AP의 경우 IoU threshold 0.5로 학습된 model이 성능이 가장 좋음
- 그러나 AP의 IoU threshold가 높아질수록(ex. AP70, AP90) IoU threshold가 0.6, 0.7로 학습된 model의 성능이 좋음

- 학습되는 IoU에 따라 대응 가능한 IoU 박스가 다름
- 그래프와 같이 high quality detection을 수행하기 위해선 IoU threshold를 높여 학습할 필요가 있음
- 단, 성능이 하락하는 문제가 존재
- 이를 해결하기 위해 Cascade RCNN을 제안

## 1.3 Method

![](https://i.imgur.com/IH1pObP.png)

![](https://i.imgur.com/y0Gql7k.png)

![](https://i.imgur.com/iN403WA.png)

RPN으로부터 B0을 얻고 projection을 하여 head를 통해 B1을 얻는다.

그 이후 B1을 projection을 하여 B2를 얻는 방식이 반복된다.

## 1.4 Result

![](https://i.imgur.com/4FqIvZG.png)

![](https://i.imgur.com/aUm2wl6.png)


- Box pooling을 반복 수행할 시 성능 향상되는 것을 증명(iterative)
- IOU threshold가 다른 Classifier가 반복될 때 성능 향상 증명(Integral)
- IOU threshold가 다른 RoI head를 cascade로 쌓을 시 성능 향상 증명(Cascade)

# 2. Deformable Convolutional Networks(DCN)

## 2.1 Contribution

### CNN 문제점

- 일정한 패턴을 지닌 convolution neural networks는 geometric transformations에 한계를 지님

![](https://i.imgur.com/kCK3TH0.png)

![](https://i.imgur.com/6dh9exS.png)

![](https://i.imgur.com/xyj3MFz.png)


### 기존 해결 방법

- Geometric augmentation
- Geometric invariant feature engineering

![](https://i.imgur.com/cUC7UV1.png)

![](https://i.imgur.com/uLFe0Jg.png)


### 제안하는 Module

- Deformable convolution

![](https://i.imgur.com/mOQM7wr.png)

![](https://i.imgur.com/OjyCUPM.png)

Conv의 각각의 filter의 영역에 offset을 추가하여 계산하는 것.

![](https://i.imgur.com/CmENFAm.png)

![](https://i.imgur.com/8pteLMl.png)

![](https://i.imgur.com/2MlOrMs.png)

![](https://i.imgur.com/52PIs2j.png)


## 2.2 Results

![](https://i.imgur.com/xI2PM1M.png)

![](https://i.imgur.com/WUIVOfC.png)


# 3. Transformer

## 3. Overview

### Transformer

- NLP에서 long range decpendency를 해결. 이를 vision에도 적용.
- Vision Transformer(ViT)
- End-to-End Object Detection with Transformers(DETR)
- **Swin Transformer**

### Self Attention

![](https://i.imgur.com/tpM7K6U.png)

![](https://i.imgur.com/nQ8gLcV.png)

![](https://i.imgur.com/h1OaNzD.png)

![](https://i.imgur.com/wA9UhUT.png)

![](https://i.imgur.com/n0r7v2P.png)


### Overview

![](https://i.imgur.com/4vngv3x.png)

### Flatten 3D to 2D (Patch 단위로 나누기)

![](https://i.imgur.com/Wo4zATX.png)

### Learnable한 embedding 처리

![](https://i.imgur.com/WgAlfjw.png)

### Add class embedding, position embedding

![](https://i.imgur.com/BEzsyiL.png)

- 앞서 만들어진 embedding 값에 class embedding 추가([CLS]Token)
- 이미지의 위치 따라 학습하기 위해 position embedding 추가


### Transformer

![](https://i.imgur.com/ZQokDlr.png)


- Embedding : Transformer 입력값


![](https://i.imgur.com/LV9bEfm.png)

### Predict 

![](https://i.imgur.com/II37owR.png)


- Class embedding vector 값을 MLP head에 입력시켜 최종 결과를 추출

### ViT의 문제점

- ViT의 실험부분을 보면 굉장히 많은 양의 Data를 학습하여야 성능이 나옴
- Transformer 특성상 computational cost 큼
- 일반적으로 backbone으로 사용하기 어려움


## 3.2 End-to-End Object Detection with Transformer (DETR)

### Contribution

- Transformer를 처음으로 Object Detection에 적용
- 기존의 Object Detection의 hand-crafted post process 단계를 transformer를 이용해 없앰

### Architecture

![](https://i.imgur.com/MDBNFZO.png)

![](https://i.imgur.com/SJYG3G3.png)

- 224 x 224 input image
- 7 x 7 feature map size
- 49개 의 feature vector를 encoder 입력값으로 사용

![](https://i.imgur.com/xo1kjwo.png)

### Train

- 이 때 groundtruth에서 부족한 object 개수만큼 no object를 padding 처리
- 따라서 groundtruth와 prediction이 N:N 맵핑
- 각 예측 값이 N개 unique하게 나타나 post-precess 과정이 필요 없음

## 3.3 Swin Transformer

### ViT의 문제점

- ViT의 실험부분을 보면 굉장히 많은 양의 Data를 학습하여야 성능이 나옴
- Transformer 특성상 computational cost 큼
- 일반적으로 backbone으로 사용하기 어려움

### 해결법

- CNN과 유사한 구조로 설계
- Window 라는 개념을 활용하여 cost를 줄임

### Arichtecture

![](https://i.imgur.com/23TtVCz.png)

### Patch Partitioning

![](https://i.imgur.com/C3jxZdw.png)

### Linear Embedding

![](https://i.imgur.com/ZZNyqnU.png)

### Swin Transformer Block

![](https://i.imgur.com/JCOtDJV.png)

ViT에선 Multi-Head Attention을 활용하지만 Swin Transformer 에선 W-MSA(Window Multi-head Self Attention)과 SW-MSA(Shifted Window Multi-head Self Attention)을 활용한다.

### Window Multi-Head Attention

![](https://i.imgur.com/EdgZmhY.png)

### Shifted Window Multi-Head Attention

![](https://i.imgur.com/WVeK35C.png)

![](https://i.imgur.com/YIostio.png)

