---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 7 - Optimization "
date:   2021-08-10 17:15:36 +0530
categories: TIL
---

-

---
### 일반화(Generalization)


![image](https://user-images.githubusercontent.com/61610411/128797889-7460499f-e245-4091-b893-2d80974151e3.png)

- 학습데이터와 Input data가 달라져도 출력에 대한 성능 차이가 나지 않게 하는 것을 일반화라고 한다. 즉 Generalzation Error 을 줄이는게 목표.


### 교차검증(Cross-Validation)


![image](https://user-images.githubusercontent.com/61610411/128793098-8f4d2905-3d27-4951-9dcb-002e74a280c5.png)


- 교차 검증의 종류
    1. K-겹 교차 검증(K-fold cross validation)
    2. Holdout 교차 검증
    3. Leave-one-out 교차 검증
    4. Leave-p-out 교차 검증


### Bias and Variance

![image](https://user-images.githubusercontent.com/61610411/128793288-2c0d9f27-5ca8-4a84-9039-8d272333deaf.png)

![image](https://user-images.githubusercontent.com/61610411/128798730-daffef87-d958-4001-a43f-169fc8d5f289.png)


### Booststrapping

![image](https://user-images.githubusercontent.com/61610411/128798870-a24fbe77-11fb-45ba-8147-673fc05ab9ab.png)


- 데이터 내에서 반복적으로 샘플을 사용하는 resampling 방법 중 하나이다.


![image](https://user-images.githubusercontent.com/61610411/128799840-df18e03a-ca9f-401d-bf7e-3e87200ee045.png)


---
##### [Optimization 종류 및 설명](https://velog.io/@yookyungkho/%EB%94%A5%EB%9F%AC%EB%8B%9D-%EC%98%B5%ED%8B%B0%EB%A7%88%EC%9D%B4%EC%A0%80-%EC%A0%95%EB%B3%B5%EA%B8%B0%EB%B6%80%EC%A0%9C-CS231n-Lecture7-Review)
----
## Regularization 


### Early stopping


![image](https://user-images.githubusercontent.com/61610411/128794895-7f4c87fc-ce8a-4812-8d79-9b6d08dd354e.png)


- 학습 횟수(=epoch 수) 가 많을수록 학습 데이터에 관한 오차는 작아지지만 이것이 오버피팅(over fitting)을 초래해서 모델의 일반화 성능이 떨어지게 된다.
  

  이 문제를 해결하기 위해 사용되는 방법이 Early Stopping 으로 **이전 epoch 때와 비교해서 오차가 증가했다면 학습을 중단한다** 는 방법이다.


### Data augmentation


![image](https://user-images.githubusercontent.com/61610411/128795015-994ebe03-b0aa-4157-a415-4ac6e86bc3e0.png)


데이터의 수가 부족해 과적합을 방지하고자 데이터를 부풀려내는 방법.


[DMQA data augmentation 세미나 발표 영상](http://dmqm.korea.ac.kr/activity/seminar/307)


### Noise robustness


![image](https://user-images.githubusercontent.com/61610411/128795160-8cac6d69-63f8-4b64-9ceb-19d7b4c77073.png)


### Label smoothing (Mixup, CutMix)


![image](https://user-images.githubusercontent.com/61610411/128795262-698461a0-668f-4250-be76-aec6aefbca5c.png)


### Dropout

### Batch normalization


![image](https://user-images.githubusercontent.com/61610411/128795454-9a1c2af9-59da-446a-b9eb-160937f12554.png)




참고자료 : (https://www.boostcourse.org/)