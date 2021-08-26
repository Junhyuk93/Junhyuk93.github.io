---
layout: post
title:  "부스트캠프 AI Tech 2기 day - 18 Train Inference"
date:   2021-08-26 13:15:10 +0530
categories: TIL
tag: [Pytorch, Model, LossFunction]
---

-


![image](https://user-images.githubusercontent.com/61610411/130900765-0fc0bdfc-7187-49dd-832f-735fbed44cc3.png)

## Train Inference

---

* 학습 프로세스에 필요한 요소는 크게 세가지(Loss, Optimizer, Metric)으로 나누어진다.


### Error Backpropagation 

![image](https://user-images.githubusercontent.com/61610411/130900796-09781ba5-f157-4959-97cc-e60870869095.png)


![image](https://user-images.githubusercontent.com/61610411/130900799-684bfe04-af07-42a0-86b2-4fb734f5b1c8.png)


![image](https://user-images.githubusercontent.com/61610411/130900809-4899d50c-ff4b-45bb-961c-531df6de5efb.png)


![image](https://user-images.githubusercontent.com/61610411/130900817-376d4422-a3ba-4bda-88f9-4888965d376e.png)


![image](https://user-images.githubusercontent.com/61610411/130900831-684cae0a-adb7-4e61-8838-0791cbb08f84.png)


![image](https://user-images.githubusercontent.com/61610411/130900838-9a007161-724c-47b0-ac6d-5d5eb84aec28.png)


---
### Focal loss / Label Smoothing loss

논문(https://arxiv.org/pdf/1708.02002.pdf)

RetinaNet 모델을 학습시키는데 Focal loss가 성능 향상

Focal loss는 분류 에러에 근거한 loss에 가중치를 부여함.

샘플이 CNN에 의해 올바르게 분류되었다면 그것에 대한 가중치는 감소한다.

즉 좀 더 문제가 있는 loss에 더 집중하는 방식으로 불균형 클래스 문제 해결

![image](https://user-images.githubusercontent.com/61610411/130909789-e142f23e-94fd-4ed6-b9ac-84a619fc80be.png)
