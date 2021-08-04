---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 3(2)"
date:   2021-08-04 23:15:36 +0530
categories: TIL
---

-

## 확률과 통계

---

### 최대 가능도 추정법(Maximum Likelihood Estimation, MLE)


- 최대 가능도 추정법은 주어진 표본에 대해 가능도를 가장 크게 하는 모수 θ 를 찾는 방법.


![image](https://user-images.githubusercontent.com/61610411/128199059-234049f2-b996-4697-ab25-0e45b41700cb.png)

### Likelihood function


- likelihood 라는 것은 지금 얻은 데이터가 분포로부터 나왔을 가능도를 말함.
  수치적으로 이 가능도를 계산히가 위해서는 **각 데이터 샘플에서**
  **후보 분포에 대한 높이(즉, likelihood 기여도)를 계산해서 모두 곱한것**을 이용 할 수 있을 것임.

    
- 계산된 높이를 더해주지 않고 곱해주는 것은 모든 데이터들의 추출이 독립적으로 연달아 일어나는 사건이기 때문.


- 그렇게 해서 계산된 가능도를 생각해볼 수 있는 모든 후보군에 대해 계산하고
  이것을 비교하면 우리는 지금 얻은 데이터를 가장 잘 설명할 수있는 확률분포를 얻어낼 수 있게 됨.


이 likelihood를 조금 더 수학적으로 서술하면


![image](https://user-images.githubusercontent.com/61610411/128205382-1358c03d-c348-4818-8636-cca9bad37211.png)


- 위 식을 likelihood function 이라고 하고 보통은 자연로그를 이용해 아래와 같이 log-likelihood function L(θ|x) 를 이용함.


![image](https://user-images.githubusercontent.com/61610411/128205864-2aeab6dd-6748-404f-86ad-87b6c930867e.png)



<참고자료>
https://drhongdatanote.tistory.com/57
https://datascienceschool.net/
https://angeloyeo.github.io/2020/07/17/MLE.html