---
layout: post
title:  "부스트캠프 AI Tech 2기 Level2 CV instance/panoptic segmentation and landmark localization "
date:   2021-09-13 21:30:10 +0530
categories: Computer Vision
tag: [CV, segmentation, localization]
---
segmentation, localization

# segmentation, localization



## 1. Instance segmentation

---

### 1.1 What is instance segmentation?

![image](https://user-images.githubusercontent.com/61610411/133177323-a4e1b155-1903-4141-8349-6d8d39d27ccf.png)

semantic segmentation 과 instance segmentation 의 가장 큰 차이는 객체, 즉 instance 가 다르다면 구분이 가능한지의 여부이다.

### 1.2 Instance segmenters

![image](https://user-images.githubusercontent.com/61610411/133178037-5dea0cc9-eebf-4b3e-8c1a-de6abd424864.png)


RoIAlign (New Pooling layer) : interpolation 을 통해서 정교한 소수점 pixel level 의 pooling을 지원함으로써 조금 더 정교한 feature를 뽑을 수 있게 되어 성능 향상을 이루었다.


![image](https://user-images.githubusercontent.com/61610411/133178118-c614759a-9897-46c7-9b89-c822093fa9ad.png)


![image](https://user-images.githubusercontent.com/61610411/133178242-4bfc31fa-ba01-4508-8f9a-a4143edc8a45.png)


![image](https://user-images.githubusercontent.com/61610411/133178331-0ebe5871-8838-42e7-8c05-5518783d04d2.png)

1. 기본 backbone 구조는 feature pyramid 구조를 가져와 사용한다. 

2. 가장 큰 특징은 Mask의 prototypes을 추출해서 사용한다는 점이다. (Mask R-CNN에서는 실제로 사용하지 않는다고 해도, 80개의 클래스를 고려하고 있다고 가정하면, 80개의 각각 독립적인 Mask를 한번에 생성해내는 구조, 여기서는 Mask 는 아니지만 Mask를 합성해낼 수 있는 기본적인 여러 물체의 soft segmentation component들을 생성해낸다.)

3. Predition Head에서 Prototype들을 잘 합성하기 위한 계수들(coefficient)을 출력한다.

4. 이 계수들과 prototype을 선형결합해서 각 detection에 적합한 Mask response map을 생성해준다.

5. 이후 Bounding box 에 맞춰 Crop 도 진행해준다.

**여기서 Mask를 효율적으로 생성하기 위해서 prototype 개수를 80개 전부 사용하지 않고, object 개수와 상관없이 적당히 작게 설정하여 그것의 선형 결합으로 다양한 Mask를 생성하는게 Key point 이다.**

![image](https://user-images.githubusercontent.com/61610411/133182015-9dbe0f47-c0eb-41da-94cd-ce811c5aa494.png)

이전 Frame에서 Key Frame에 해당하는 feature를 다음 Frame에 전달해 feature map 의 계산량을 획기적으로 줄였다.


