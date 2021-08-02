---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 1"
date:   2021-08-02 22:34:36 +0530
categories: TIL
---

-

### 공부한 내용
### 1. 경사하강법(Gradient Descent) 이란?


    - 학습률과 손실함수의 순간기울기(gradient)를 이용하여 가중치(weight)를 업데이트 하는 방법
    즉, 미분의 기울기를 이용하여 도표의 오차들을 비교하고 오차를 최소화하는 방향으로 이동하는 방법.



#### 파이썬으로 경사하강법 코드 구현하기


```python
import numpy as np
import matplotlib.pyplot as plt
X = np.random.rand(100)
Y = 0.4 * X * 0.6

plt.figure(figsize=(8,6))
plt.scatter(X,Y)
plt.show()

def plot_prediction(pred,y):
    plt.figure(figsize=(8,6))
    plt.scatter(X,y)
    plt.scatter(X,pred)
    plt.show()
    
# 경사 하강법 파이썬 구현

W = np.random.uniform(-1,1)
b = np.random.uniform(-1,1)

learning_rate = 0.9

for epoch in range(100):
    Y_pred = W * X + b
    
    error = np.abs(Y_pred - Y).mean()
    
    if error < 0.001:
        break
        
    w_gred = learning_rate * ((Y_pred - Y) * X).mean()
    b_gred = learning_rate * (Y_pred - Y).mean()
        
    W = W - w_gred
    b = b - b_gred
        
    if epoch % 5 == 0:
        Y_pred = W * X + b
        plot_prediction(Y_pred, Y)
```


![15](https://user-images.githubusercontent.com/61610411/127855455-c61c7f92-8dc3-49e1-862b-bd3771d7d130.png)


-   실행시 실제값인 주황색 선이 그려지고 예측값인 파란색 선이 그려진다.

    과정이 진행될수록 예측값인 파란색 선이 주황색 선에 가까워짐을 알 수 있다.


-   non-conex한 그래프에선 경사하강법을 쓸 수 없다. 이를 보완하기 위해 **확률적 경사하강법**을 사용한다.


---
### 2. 확률적 경사하강법(Stochastic Gradient Descent)


    - 확률적 경사하강법은 모든 데이터를 사용해서 업데이트하는 대신 데이터 한개 또는 일부 활용하여 업데이트한다.


    SGD는 미니배치(Mini-Batch)를 가지고 그레디언트 벡터를 계산한다. 미니배치는 확률적으로 선택하므로 목적식 모양이 바뀐다.


![16](https://user-images.githubusercontent.com/61610411/127858518-9ed67cc6-23d1-4ba6-9846-3b3010216168.png)


---
### |<span style="color:blue">궁금했던 점.</span>|




**<u>Object Detection?    Localization?    Semantic Segmentation?    Instance Segmentation?</u>** 

![17](https://user-images.githubusercontent.com/61610411/127860040-02562c8d-3945-43f8-9500-86b07fba264c.PNG)


---


![18](https://user-images.githubusercontent.com/61610411/127862694-c0b5fef8-1e9d-4c0e-b2e2-da90060bdfe1.png)


- 물체를 바운딩 박스로 나타내는 Object Detection과 다르게, Semantic Segmentation은 이미지 내 모든 픽셀이 각각 어느 클래스에 속하는지 예측하고 그것들을 모아서 이미지를 의미 있는 단위로 분할한다. 


![19](https://user-images.githubusercontent.com/61610411/127862733-918c43df-f166-4009-8086-fcd4b36bffc1.png)


- Semantic Segmentation 의 경우 같은 클래스에 속하면 각각 독립된 개체라고 하더라도 하나의 색으로 나타냈었으나, Instance Segmentation의 경우 같은 경우에 속하더라도 독립된 개체라면 각기 다른 색으로 표현해준다.


##### url 관련 unbalanced data를 해결하기 위해 GAN(Generative adversarial network)를 적용해보려 했으나 실패했었다. CV에서는 최근 논문에도 GAN이 자주 언급되는거 보니 성능이 괜찮은가보다.


<참고자료>  
- https://iyousys.tistory.com/42  
- https://modulabs-biomedical.github.io/FCN  
- https://light-tree.tistory.com/75  