---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 2"
date:   2021-07-29 22:34:36 +0530
categories: TIL
---

-


## 공부한 내용

#### 비선형모델 신경망(Neural Network)


1. 소프트맥스(softmax)

 - 모델의 출력을 확률로 해석할 수 있게 변환해주는 연산
   분류 문제를 풀 때 선형모델과 소프트맥스 함수를 결합하여 예측 (다중 클래스)


 ![20](https://user-images.githubusercontent.com/61610411/127939028-ca6d2ce5-1234-40a6-a7cd-207d500e57ed.PNG)


2. 활성 함수(activation function)

 - 실수 위에 정의된 비선형(nonlinear) 함수
   입력된 데이터의 가중 합을 출력 신호로 변환
 

 ![21](https://user-images.githubusercontent.com/61610411/127939442-277b07d2-1a24-4c65-bcff-7dcd974fa3d1.PNG)


**신경망은 선형모델과 활성함수를 함성한 함수**


#### 딥러닝 학습원리 - 역전파 알고리즘


##### 순전파(Feedforward) 와 역전파(Backpropagation) 의 개념

- 다층 퍼셉트론(Multi-layer Perceptron, MLP)으로 학습 한다는 것은 최종 출력값과 실제값의 오차가 최소화 되도록 가중치와 바이어스를 계산하여 결정하는 것.
  순전파 알고리즘에서 발생한 오차를 줄이기 위해 새로운 가중치를 업데이트하고, 새로운 가중치로 다시 학습하는 과정을 역전파 알고리즘 이라고 함. 역전파 알고리즘을 실행할 때
  가중치를 결정하는 방법에서는 경사하강법이 사용됨.


**순전파(Feedfoward)**


 - 입력층에서 은닉층 방향으로 이동하면서 각 입력에 해당하는 가중치가 곱해지고, 결과적으로 가중치 합으로 계산되어 은닉층 뉴런의 함수 값이 입력된다. 그리고 최종 결과(output)가 출력된다.


 ![22](https://user-images.githubusercontent.com/61610411/127940818-785a02d2-a34a-4ae3-9ff6-c0e54dc8bd5b.PNG)

 
**역전파(Backpropagation)**

 
 - 역전파 알고리즘은 input과 output 값을 알고 있는 상태에서 신경망을 학습 시키는 방법. 이 방법을 지도학습(Supervised learning) 이라고 함.
   초기 가중치, weight 값은 랜덤으로 주어지고 각각   노드들은 하나의 퍼셉트론으로 노드를 지날때 마다 활성함수를 적용.


 ![23](https://user-images.githubusercontent.com/61610411/127941020-04674e94-1256-42b4-8138-ee103d95fdc5.PNG)



( 참고자료 https://sungwookkang.com/1415 ) 