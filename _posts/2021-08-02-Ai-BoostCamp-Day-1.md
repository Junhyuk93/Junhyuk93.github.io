---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 1"
date:   2021-08-02 22:34:36 +0530
categories: Python, Math, CV, NLP
---

-

## 공부한 내용

- 경사하강법(Gradient Descent) 이란?
    학습률과 손실함수의 순간기울기(gradient)를 이용하여 가중치(weight)를 업데이트 하는 방법
    즉, 미분의 기울기를 이용하여 도표의 오차들을 비교하고 오차를 최소화하는 방향으로 이동하는 방법.



### 파이썬으로 경사하강법 코드 구현하기


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


- 실행시 실제값인 주황색 선이 그려지고 예측값인 파란색 선이 그려진다.  과정이 진행될수록 예측값인 파란색 선이 주황색 선에 가까워짐을 알 수 있다.