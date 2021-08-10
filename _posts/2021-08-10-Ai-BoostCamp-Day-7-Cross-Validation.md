---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 7 - CrossValidation "
date:   2021-08-10 17:15:36 +0530
categories: TIL
---

cv


---
# 교차 검증 (Cross Validation)


## 교차검증의 필요성


- 교차검증을 하는 이유는 과적합을 피하면서 파라미터를 튜닝하고 일반적인 모델을 만들고 더 신뢰성 있는 모델 평가를 진행하기 위해서이다.

![image](https://user-images.githubusercontent.com/61610411/128806715-2c6433fb-6e75-4494-9249-9d23087ed65d.png)


- 우리는 일반적으로 전체 데이터를 7:3으로 나누고 train set을 학습하여 test set으로 평가한다.
  이 경우 문제점은 **우리가 학습한 모델이 제대로 학습되었는지, 또 모델을 학습하는 데에 있어서 train set에 과적합이 된건 아닌지** 확인 할 수 없다.
  그래서 우리는 최종 평가 전에 모델을 학습하기 위해서 validation set을 지정한다.


![image](https://user-images.githubusercontent.com/61610411/128806937-06721c09-acfb-41ed-853d-eae6e231fb3c.png)


- 위와 같이 데이터셋을 구성한다면 더 이상 train set으로만 학습하는 것이 아니라 validation set으로 평가하면서 하이퍼 파라미터를 튜닝하고 모델을 학습할 수 있다. 
  하지만 여전히 생기는 문제점은 있다. 앞선 방법이 train set 에만 과적합되었다면 이번에는 validation set 에만 과적합 될 수 있다. 또한 모델을 평가하는데 있어서 특정 데이터셋에서만 성능이 잘 나와 test set 에서는 성능이 잘 나오지 않는 경우가 생길 수 있다.


- 이렇듯 과적합을 피하고자 validation set을 구성하더라도 하나의 validation set으로만 구성한다면 하나의 validation set만 잘 맞추는 파라미터를 튜닝하고 모델을 학습하게 될 수 있습니다. 그래서 하나의 검증이 아닌 여러번의 검증으로 일반적인 모델을 만들고 파라미터를 튜닝하기 위해서 교차검증이 등장합니다.


## 교차검증의 종류


### K-Fold Cross Validation


- K-Fold CV는 가장 일반적이고 많이 사용되며 강력한 교차 검증 방법 중 하나이다. 말 그대로 **k개의 fold를 구성하는데 즉 k개의 다른 데이터셋을 구성하고 모델을 각각 학습하는 방법**이다.


![image](https://user-images.githubusercontent.com/61610411/128807319-e6d6b1c3-1c02-47f8-87f8-f159f6e579c0.png)

- k-flod 에서 k 는 우리가 지정할 수 있다. 위의 그림에서는 k를 5로 지정하였고 전체 데이터를 임의로 1/5로 나누어 validation set을 한번씩 번걸아가면서 데이터셋을 구성하였다.

- **각 데이터를 학습하고 validation으로 평가를 한 다음 5개의 결과에 대해 평균을 내어 최종 성능을 구한다**. 이러한 식으로 데이터를 구성한다면 우선 모델 평가부분에서 **고정된 validation set이 아니기 때문에 신뢰성 있는 결과**를 얻을 수 있다.


### Stratified K-fold Cross Validation


- **층화 k-fold CV는 기존 k-fold CV와 비슷한 방법으로 수행되나 계층을 고려** 하는 방법이다

- 기존 k-fold 방법을 사용한다면 데이터가 임의로 섞이면서 validation set에 특정 클래스가 과하게 분포될 수 있다. 예시로 전체 데이터 중 클래스 0이 80%, 1이 20% 라고 하자.


![image](https://user-images.githubusercontent.com/61610411/128807576-27d9915c-ed27-41ce-95ab-885c771ac633.png)


- 그렇다면 어떤 flod에서는 다음과 같이 클래스 비율이 상이하게 분포되어 원치 않는 결과를 도출할 수도 있다. 그래서 층화 k-fold CV는 전체 데이터의 클래스 비율을 고려하여 set을 구성한다.

![image](https://user-images.githubusercontent.com/61610411/128807664-8291cd24-3237-4896-abe4-641fbe28baac.png)


- 일반적으로 클래스가 있는 분류 문제에서는 층화 k-fold CV를 활용하며 회귀 문제에서는 K-fold CV를 활용한다.


### Leave One Out Cross Validation(LOOCV)


- **LOOCV 방법은 하나의 데이터만 남기고 나머지 데이터로 학습을 하고 평가하는 방식**이다. 만약 총 데이터가 N개라면 (N-1)개의 데이터로 학습하고 1개의 데이터로 평가한다. 총 N개의 데이터가 있기 때문에 하나씩 반복하면 N번 학습을 진행한다.


- K-Fold와 비교하자면 K가 데이터 개수 N인 경우 N-Fold CV라고 생각하면 된다. 이 방법은 데이터가 많으면 많을수록 굉장한 시간이 걸린다. 하지만 데이터를 하나씩 보면서 학습할 수 있기 때문에 bias는 광장히 작을 수도 있을 것이고, variance는 굉장히 클 수도 있다.

- LOOCV 방법은 일반적인 경우에는 추천하지 않으나 데이터의 개수가 많이 부족할 때는 유용한 결과를 도출할 수도 있는 방법이다.


### Repeated Random Sub Sampling Validation


- 이 검증 기법은 임의로 validation set을 추출하여 평가하고 하이퍼파라미터를 튜닝할 수 있는 방법이다.


![image](https://user-images.githubusercontent.com/61610411/128808072-165640db-18d5-4d69-ae3a-130a3a5e898c.png)


- 위의 빨간색 부분으로 처리된 부분이 임의로 선택된 validation set이다. 이런식으로 임의로 validation set을 지정하고 해당 set으로 평가하고 튜닝을 할 수 있다. 이 방법의 장점은 **기존 k-fold와 다르게 데이터를 분할하는 것에 iteration이 종속되지 않는다는 것**이다. 예를 들어 10-fold의 경우 10개의 validation set을 만든다면 10번의 itertation을 수행해야 하지만 이 기법은 사용자가 설정할 수 있다. **반면 임의로 선정되는 validation set으로써 validation set이 중복될 수도 있고 한번도 선택되지 않은 데이터가 있을 수도 있다.


### Nested Cross Validation


- ested Cross Validation은 기존의 교차검증을 중첩한 방식이다. 


![image](https://user-images.githubusercontent.com/61610411/128808533-aa69d709-2222-42c4-bba2-da4bc7ef1000.png)


- 이처럼 Nested CV는 Outer Loop와 Inner Loop로 구성되어 있다.  outer loop에 구성된 각 폴드에는 train set과 test이 존재하는데 여기 존재하는 train set에서 inner loop가 작동한다. inner loop에서는 outer loop에 사용될 train set을 train subset과 validation set으로 분리하고 검증셋으로 평가하면서 파라미터를 튜닝한다. 그렇게 나온 성능의 평균이 가장 좋은 하이퍼파라미터가 나올 거고 해당 최적의 파라미터를 통해 fold1에서 test set으로 평가한다. 이러한 방식을 반복하는 방법이다.

- 결국 outer loop에서는 test set에 대한 평가를, inner loop에서는 각 폴드의 최적 파라미터를 튜닝하는 것이 목적이다.

- 굳이 이렇게 test set 또한 다양하게 나누어서 평가한 이유는 기존 방식처럼 랜덤하게 나눈 하나의 test set 또한 신뢰성이 없을 수 있다는 것이다. 그래서 다양하게 나눈 test set과 각각에 맞는 최적의 파라미터 튜닝한 모델을 통해 모델의 일반화를 추구한다.
