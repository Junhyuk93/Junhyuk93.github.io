---
layout: post
title:  "부스트캠프 AI Tech 2기 Level2 P_stage Segmentation 기초와 이해 "
date:   2021-10-18 15:30:10 +0530
categories: Segmentation
tag: [CV, Segmentation, Sementic Segmentation, FCN,mIoU]
---
# Semantic Segmentation의 기초와 이해

# 1. 대표적인 딥러닝을 이용한 Segmantation FCN

![](https://i.imgur.com/eIu2FYN.png)

1. VGG 네트워크 백본을 사용(Backbone : feature extractiong network)
2. VGG 네트워크의 FC Layer (nn.Linear)를 Convolution으로 대체
3. Transposed Convolution을 이용해서 Pixel Wise prediction을 수행

## 1.1 Fully Connected Layer vs Convolution Layer

FC Layer를 Conv Layer로 변환했을 때의 이점

![](https://i.imgur.com/AUhPpa0.png)

![](https://i.imgur.com/izQFpC1.png)

![](https://i.imgur.com/bkSTZXd.png)

## 1.2 Transposed Convolution

![](https://i.imgur.com/0N3zquA.png)

![](https://i.imgur.com/WR2zFhZ.png)

![](https://i.imgur.com/ky1uzgK.png)


![](https://i.imgur.com/uYatw63.png)

![](https://i.imgur.com/EaizFC1.png)

![](https://i.imgur.com/RLAf05r.png)

![](https://i.imgur.com/77gy37P.png)

## 1.4 FCN에서 성능을 향상시키기 위한 방법

![](https://i.imgur.com/ehqUafz.png)

![](https://i.imgur.com/e3np6lM.png)

![](https://i.imgur.com/WkHa34V.png)

- FCN에서 성능을 향상 시키기 위한 방법

```python
FCN(backbone:VGG)의 구조
conv1 : [conv→relu→conv→relu→maxpool] 이미지 크기 input의 1/2로 감소
conv2 : [conv→relu→conv→relu→maxpool] 이미지 크기 input의 1/4로 감소
conv3 : [conv→relu→conv→relu→conv→relu→maxpool] 이미지 크기 input의 1/8로 감소
conv4 : [conv→relu→conv→relu→conv→relu→maxpool] 이미지 크기 input의 1/16로 감소
conv5 : [conv→relu→conv→relu→conv→relu→maxpool] 이미지 크기 input의 1/32로 감소
FC6 : [conv(1×1)→relu→dropout]
FC7 : [conv(1×1)→relu→dropout]
score : [conv(input_channel, num_classes,1,1,0)]
upscore : [convTransposed2d(num_classes, num_classes, karnel_size=64, stride=32, padding=16)] 
					1/32가 된 이미지를 32배 키워 원본 사이즈로 복원
```

Issue! 1/32크기로 줄어든걸 바로 32배 키워서 복원하려니 디테일한 정보들이 사라진 것을 확인

- solution skip connection

1. MaxPooling에 의해 잃어버린 정보 복원해주는 작업 진행
2. Upsampled Size를 줄여줘 더 효율적인 이미지 복원 가능

conv4  block에서 pooling하고 난 결과가 score와 같은 구조인 1×1conv거친 뒤(하지만 새롭게 선언 된 conv)의 결과와 deconv(2배 키워주는 deconv)결과를 summation해준 뒤 다시 deconv(16배 키워주는 deconv)를 통해 FCN16s 생성

### 1.5 평가지표

![](https://i.imgur.com/kt2F7Aa.png)

## 2.1 Futher Reading

### 1x1 을 7x7 Convolution으로 바꿀 때 발생하는 문제 

![](https://i.imgur.com/j0RbIUc.png)


![](https://i.imgur.com/TcSNm3M.png)

![](https://i.imgur.com/gS8ljeM.png)

![](https://i.imgur.com/9Xog7VT.png)



- FC6을 1×1이 아닌 7×7conv사용하게 될 경우 이미지의 크기가 변동되는 문제 발생

  Conv5의 이미지 크기가  7×7이기 때문에 7×7conv을 적용시 1×1이 나오게 됨

  이를 해결하기 위해 논문의 저자들은 

![](https://i.imgur.com/u2LLOv0.png)

1. conv1 block의 첫번째 conv에서 zero padding을 100 넣어줌

    → 하지만 Output size가 256×256으로 input 224×224와 달라짐

1. 1을 해결하기 위해 Deconv의 padding을 제거 하고 중앙의 224×224만 crop하는 작업

    FCN 16s, 8s에서도 crop하는 방법 사용