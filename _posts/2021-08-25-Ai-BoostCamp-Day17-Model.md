---
layout: post
title:  "부스트캠프 AI Tech 2기 day - 17 Model"
date:   2021-08-25 13:15:10 +0530
categories: TIL
tag: [Pytorch, Model]
---

-



![KakaoTalk_20210825_130723576](https://user-images.githubusercontent.com/61610411/130725188-26e6448f-deac-4cc3-8cbb-0d118cacd3e9.jpg){: height="500"}

## Model

- Model 에서는 nn.Module을 상속받아 init 으로 초기 설정을 하고 모델이 호출되었을 때 실행되는 함수인 forward를 실행하게 된다. 모든 nn.module은 child module을 가질 수 있고, 모델을 정의하는 순간 그 모델에 연결된 모든 module을 확인할 수 있다.

- 모델에 정의되어 있는 모듈들이 가지고 있는 계산에 쓰일 파라미터들을 확인할 수 있다. 각 모델 파라미터들은 data,grad,require_grad 변수 등을 가지고 있으므로, trainning precess 과정에서 유용하다.

![KakaoTalk_20210825_130723932](https://user-images.githubusercontent.com/61610411/130725192-53462ecc-498a-4f5f-aa64-96f2c2b4252c.jpg){: height="500"}

![KakaoTalk_20210825_130724326](https://user-images.githubusercontent.com/61610411/130725199-7833b148-1506-43f4-bdc3-d5eb0d815399.jpg){: height="500"}

