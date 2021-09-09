---
layout: post
title:  "부스트캠프 AI Tech 2기 CV - 2 "
date:   2021-09-03 16:30:10 +0530
categories: Segmentation
tag: [Semantic segmentaiton,segmentation]
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

- Fully **convolutional** layer : 

![image](https://user-images.githubusercontent.com/61610411/132728932-25821b17-5da6-4b9d-af97-d7c372d95da0.png)
