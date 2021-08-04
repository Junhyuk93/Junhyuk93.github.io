---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 3"
date:   2021-08-04 20:34:36 +0530
categories: TIL
---

-

## 통계학


#### 데이터를 보고 확률분포 가정하기.

- 확률 분포를 가정하는 방법 : 히스토그램을 통해 모양을 관찰
    - 데이터가 2개의 값 ( 0 또는 1 ) 만 가지는 경우 -> **베르누이분포**
    
    
    ![image](https://user-images.githubusercontent.com/61610411/128133910-e91ed48e-67d2-4d40-aaf6-42da2e06b6f2.png)


    - 데이터가 n개의 이산적인 값을 가지는 경우 -> **카테고리분포**
    
    
    ![image](https://user-images.githubusercontent.com/61610411/128149550-4cf5ed86-0169-4a06-afd7-0ca739a3ae8d.png)


    - 데이터가 (0,1) 사이에서 값을 가지는 경우 -> **베타분포**
    
    
    ![image](https://user-images.githubusercontent.com/61610411/128149845-fc7578f7-0a37-495a-b22f-a13c51767b0c.png)

    
    - 데이터가 0 이상의 값을 가지는 경우 -> **감마분포, 로그정규분포 등**
    
    
    ![image](https://user-images.githubusercontent.com/61610411/128150065-e19e83fa-8528-49df-8b1e-9ed6b0fffbdb.png)


    - 데이터가 실수 전체에서 값을 가지는 경우 -> **정규분포, 라플라스분포 등**


    ![image](https://user-images.githubusercontent.com/61610411/128150794-7208d2b8-8d33-4a52-ab16-474b5737492f.png)

- 기계적으로 확률분포를 가정해서는 안 되며, **데이터를 생성하는 원리를 먼저 고려하는것이 원칙**


---


### 중심극한정리(Central Limit Thorem)


- 동일한 확률 분포를 가진 독립 확률 변수 n개의 평균의 분포는 n개가 커짐에 따라
  모집단 분포의 모양에 관계없이 정규분포와 가까워지는 정리


![image](https://user-images.githubusercontent.com/61610411/128196019-fee55020-d5f9-4d1c-8ba9-3b2814825328.png)


![image](https://user-images.githubusercontent.com/61610411/128196246-eb1838c5-98e7-4c0f-a930-ecd7da686d90.png)

---

### 최대 가능도 추정법(Maximum Likelihood Estimation, MLE)


- 최대 가능도 추정법은 주어진 표본에 대해 가능도를 가장 크게 하는 모수 θ 를 찾는 방법.


![image](https://user-images.githubusercontent.com/61610411/128199059-234049f2-b996-4697-ab25-0e45b41700cb.png)

### Likelihood function

    - likelihood 라는 것은 지금 얻은 데이터가 분포로부터 나왔을 가능도를 말함.
    수치적으로 이 가능도를 계산히가 위해서는 **각 데이터 샘플에서 후보 분포에 대한 높이(즉, likelihood 기여도)를 계산해서 모두 곱한것**을 이용 할 수 있을 것임.

    
    계산된 높이를 더해주지 않고 곱해주는 것은 모든 데이터들의 추출이 독립적으로 연달아 일어나는 사건이기 때문.

    그렇게 해서 계산된 가능도를 생각해볼 수 있는 모든 후보군에 대해 계산하고 이것을 비교하면 우리는 지금 얻은 데이터를 가장 잘 설명할 수있는 확률분포를 얻어낼 수 있게 됨.

    이 likelihood를 조금 더 수학적으로 서술하면

![image](https://user-images.githubusercontent.com/61610411/128205382-1358c03d-c348-4818-8636-cca9bad37211.png)


    위 식의 결과 값이 가장 커지는 θ 를 추정값 θ^ 로 보는 것이 가장 근사함.


    위 식을 likelihood function 이라고 하고 보통은 자연로그를 이용해 아래와 같이 log-likelihood function $L(θ|x)$ 를 이용함.


![image](https://user-images.githubusercontent.com/61610411/128205864-2aeab6dd-6748-404f-86ad-87b6c930867e.png)



<참고자료>
https://drhongdatanote.tistory.com/57
https://datascienceschool.net/
https://angeloyeo.github.io/2020/07/17/MLE.html