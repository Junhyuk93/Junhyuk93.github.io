---
layout: post
title:  "부스트캠프 AI Tech 2기 Level2 P_stage HRNet 이해 "
date:   2021-10-25 15:30:10 +0530
categories: Segmentation, HRNet
tag: [CV, Segmentation, Sementic Segmentation,HRNet]
---

# HRNet

## HRNet의 필요성

### Image Classification Networks

![](https://i.imgur.com/XmA0yPF.png)

<center>LeNet 뿐만 아니라 AlexNet,VGGNet,ResNet,Efficientnet 들도 고해상도 입력을 받아

저해상도로 줄여나가는 설계 방식을 사용함</center>


### Image Classification 모델이 해상도를 점점 줄여나가는 이유는?

* 특정 물체를 분류하는데 이미지 내 모든 특징이 필요하지 않음
* 해상도가 줄어들어 효율적인 연산이 가능하며, 각 픽셀이 넓은 receptive field를 갖게 됨
* 중요한 특징만을 추출하여 과적합을 방지

![](https://i.imgur.com/fMQwYif.png)


![](https://i.imgur.com/FyccvyD.png)


### DeconvNet, SegNet, U-Net

**저해상도 특징을 생성하고, 다시 고해상도로 복원하는 방식의 기존 연구**

![](https://i.imgur.com/h4ItBcv.png)


### DeepLab

![](https://i.imgur.com/Dp13j5O.png)


### DilatedNet, DeepLab

* DilatedNet 및 DeepLab 구조는 해상도의 이점을 살리고자 dilated convolution을 적용
* U-Net 등의 구조와 달리 저해상도가 아닌 중해상도 정보를 고해상도로 복원

![](https://i.imgur.com/UvtSQ8i.png)


### DeepLab V3+

- 자세한 정보를 유지하기 위해 Xception 구조 내 max pooling 연산을 depthwise separable convolution으로 변경


![](https://i.imgur.com/LysEhmk.png)

![](https://i.imgur.com/6s7DH7O.png)

<center>low level feature를 전달해주는 skip connection 활용</center>


### Classification nased Networks

- 기존 classfication Network 사용에 필요했던 높은 시간 복잡도와 Upsampling을 이용해 저해상도부터 고해상도로 복원하며 생성되는 특징은 공간상의 위치 정보의 민감도(position-sensitivity)가 낮음

> **위 문제점들을 해결하기 위해 강력한 위치 정보를 갖는 visual recognition 문제에 적합한 구조가 필요하다..!!**

 ![](https://i.imgur.com/NuzFw97.png)

저해상도/ 중해상도를 고해상도로 복원하는 것이 아닌, 고해상도 정보를 계속 유지하고자 하는 것이 핵심이였고 이를 토대로 연구한 것이 **HRNet(High Resolution Network)**


### HRNet(High Resolution Network)

주로 image classification 문제에 사용되는 backbone network이 아닌, 위치정보가 중요한 visual recognition 문제 (segmentation, object detection, pose estimation 등)에 사용할 수 있는 새로운 backbone network 

![](https://i.imgur.com/6reUHtD.png)

1. 전체 과정에서도 고해상도 특징을 계속 유지
    - 입력 이미지의 해상도를 그대로 유지하는 것이 아닌, strided convolution을 이용해 해상도를 1/4로 줄임. (U-Net이나 DeepLab v3+에 비해 상대적으로 높은 해상도를 유지함.)

![](https://i.imgur.com/fwibzBb.png)

2. 고해상도부터 저해상도까지 다양한 해상도를 갖는 특징을 병렬적으로 연산

![](https://i.imgur.com/KEOCMsW.png)

### Paralled Multi-Resolution Convolution Stream

- 고해상도 convolution stream을 시작으로 점차 해상도를 줄여 저해상도 stream을 새롭게 생성
- 새로운 stream이 생성될 때 해상도는 이전 단계 해상도의 1/2로 감소
    - **해상도를 줄여 넓은 receptive field를 갖는 특징을 고해상도 특징과 함께 학습함**
![](https://i.imgur.com/u9g9il2.png)


3. 각각의 해상도가 갖는 정보를 다른 해상도 stream에 전달하여 정보를 융합

- 고해상도 특징: 공간 상의 높은 위치 정보 민감도(position-sensitivity)를 가짐
- 저해상도 특징: 넓은 receptive field로 인해 상대적으로 풍부한 의미 정보(semantic informantion)를 가짐


![](https://i.imgur.com/9r5f50B.png)

pooling 대신 strided convolution을 사용한 이유는 정보 손실을 최소화 하기 위함이고, convoltion 대신 Upsampling을 사용한 이유는 시간 복잡성을 고려했기 때문

### Representation Head

![](https://i.imgur.com/Azxb6dJ.png)


![](https://i.imgur.com/aQinBUr.png)

![](https://i.imgur.com/FbsIE74.png)
