---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 4"
date:   2021-08-04 23:15:36 +0530
categories: TIL
---

-

## CNN 이해하기

- Convolution 연산은 커널(kernel)을 입력 벡터 상에서 움직여가면서 선형모델과 합성함수가 적용되는 구조.


![image](https://user-images.githubusercontent.com/61610411/128285565-799639ed-6dd6-46e0-8633-8408aeb038dc.png)




#### 다양한 차원에서의 Convolution


> 1D-conv -> $$[f * g](i,j) = \displaystyle\sum_{p,q,r}{f(p,)g(i+p)}$$


> 2D-conv -> $$[f * g](i,j) = \displaystyle\sum_{p,q,r}{f(p,q)g(i+p,j+q)}$$


> 3D-conv -> $$[f * g](i,j,k) = \displaystyle\sum_{p,q,r}{f(p,q,r)g(i+p,j+q,k+r)}$$

**i,j,k가 바뀌어도 커널 $$f$$의 값은 바뀌지 않음.**

(참고자료:https://zsunn.tistory.com/entry/AI-CNN%EA%B3%BC-RNN%EC%9D%98-%EC%9D%B4%ED%95%B4)