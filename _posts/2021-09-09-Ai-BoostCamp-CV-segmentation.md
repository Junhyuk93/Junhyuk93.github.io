---
layout: post
title:  "부스트캠프 AI Tech 2기 Level2 CV segmentation "
date:   2021-09-08 21:30:10 +0530
categories: Computer Vision
tag: [CV, segmentation]
---


-



# Semantic segmentaiton

## 1.1 Semantic segmentation ?

- Semantic segmentation 이란 image classification 을 영상 단위로 하는게 아니라 pixel 단위로 분류하는 문제(하나의 pixel이 사람에 속하는지, 오토바이에 속하는지 등에 대한 구분하는 문제). 같은 클래스이지만 서로 다른 물체를 구별하지는 않는다.

![image](https://user-images.githubusercontent.com/61610411/132726433-4b604f66-ffee-44b8-9350-8878ca814083.png)

## 1.2 Where can semantic segmentation be apllied to ?

### Application

- Medical images 나 Autonomous driving 응용 등 영상 내의 장면에 대한 컨텐츠를 이해하는데 다양하게 사용될 수 있다.

![image](https://user-images.githubusercontent.com/61610411/132726935-44217686-cf2c-45c5-8858-328fa2f7d64d.png)


## 2.1 Fully Convolution Networks (FCN)

### Fully convolutional networks

- 첫 end-to-end 구조의 semantic segmentation

- end-to-end 란? 입력부터 출력까지 모두 미분가능한 nueral network 의 형태로 구성되어 입력과 출력데이터의 페어만 있다면 학습을 통해 target task (중간의 nueral network를 학습하여) 를 해결할 수 있다.


![image](https://user-images.githubusercontent.com/61610411/132728155-34cbc1f2-50b8-4c1c-8dab-3f5eb57104e6.png)


### Fully connected vs. Fully convoltional

- Fully **connected** layer : vector 가 주어지면 matrix 가 multiplication 되서 또 다른 fix dimension 이 나오는 구조.

- Fully **convolutional** layer : 입력과 출력이 activation map 이고 spatial coordinate(공간 좌표)를 유지한 채, operation이 수행된다. 그리고 1x1 convolution을 활용한다.

![image](https://user-images.githubusercontent.com/61610411/132728932-25821b17-5da6-4b9d-af97-d7c372d95da0.png)


### interpreting fully connected layers as 1x1 convolutions

![image](https://user-images.githubusercontent.com/61610411/132780348-1d15d5d5-2478-4957-9b93-15e7aba0f7a2.png)

AlexNet에선 Flattening 을 통해 긴 vector 형태로 만들어 주어서 Fully-connected layer 에 input 으로 넣어준다. 이러한 경우 영상의 공간 정보를 고려하지 않고, 하나의 vector 로 섞이게 된다. 어떻게 하면 각 위치마다 classification을 할 수 있도록 할 수 있을까? 

![image](https://user-images.githubusercontent.com/61610411/132780409-8d77e2a8-c49b-4f2d-a77c-62846b216a7d.png)

 -> 각 위치 마다 채널 축으로 flatten을 한다. 채널 축의 각각의 값을 vector로 쌓는다. 이렇게 되면 각각에 대해서 입력에 Fully-connected layer를 적용할 수 있게 된다.

 ![image](https://user-images.githubusercontent.com/61610411/132780780-9ea11928-ecb1-49ab-9096-72d7ecbd6d94.png)

채널 축으로 1x1 Convolution kernel 이 Fc layer의 한 weight columns 이라고 볼 수 있다. 따라서 filter 갯수만큼 통과시킨다면 우리가 각각의 위치마다 FC layer를 별도로 돌려서 결과값을 채워넣는 것과 동일한 결과를 가질 수 있다.

**결론적으로** FC Layer 를 1x1 Convolution 으로 대체할 수 있게 되고, 어떤 입력 사이즈에도 대응이 가능한 그런 Fully-Convolution Neural network 를 만들 수 있게 된다.

이렇게 semantic segmentation 을 통하면 굉장히 작은 score map을 얻게 된다. stride나 pooling layer를 통하면서 최종적으로 굉장히 저해상도인 activation map이 되는데 이러한 저해상도 문제를 해결하기 위해 **upsampling**이 활용된다.


### Upsampling

![image](https://user-images.githubusercontent.com/61610411/132781360-18f4bb8b-e461-4c0e-897a-cb66e1f79800.png)

- Upsampling 의 두가지 방법

    1. Transpose convolution
    2. Upsample and convolution

![image](https://user-images.githubusercontent.com/61610411/132782241-f5bce3b9-75ef-420d-bad3-1e415695fc82.png)

![image](https://user-images.githubusercontent.com/61610411/132782348-b1e8c6f6-8e36-491f-b1e7-9db5dfa3205b.png)

transpose convolution를 사용할 때에는 매우 주의해야 할 부분이 convolution kernel size 와 stride 를 잘 조절해서 중첩이 생기지 않도록 신경써서 튜닝을 해줘야 한다. (반복되는 blocky 한 checkboard 구조가 생기지 않도록)

(https://runebook.dev/ko/docs/pytorch/generated/torch.nn.upsample)

### Better approaches for upsampling

![image](https://user-images.githubusercontent.com/61610411/132785793-61eaea88-99a8-48c9-a215-03c4a4ab4364.png)

일부분이 Overlap 되는 Transfose 방법과 달리 upsampling은 그러한 중첩 문제가 없고 골고루 영향을 받게 한다. 

### Adding skip connections for enlaging the score map

![image](https://user-images.githubusercontent.com/61610411/132786028-1dff3853-4386-462b-b7e1-ae098a53a107.png)

초반부(낮은 layer)에서는 receptive filed size가 작으므로 굉장히 국지적이고 작은 detail에 민감한 경향이 있고, 후반부(높은 layer)에서는 해상도는 낮아지지만 큰 receptive filed 를 가지며 영상에서 전반적이고 의미론적인 정보들을 많이 포함하는 경향이 있다.

![image](https://user-images.githubusercontent.com/61610411/132786506-afb2e082-6d3e-4181-bcdb-0251c56daac9.png)
