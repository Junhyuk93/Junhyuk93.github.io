---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 4 - CNN"
date:   2021-08-04 23:15:36 +0530
categories: TIL
---

-

## CNN 이해하기

- Convolution 연산은 커널(kernel)을 입력 벡터 상에서 움직여가면서 선형모델과 합성함수가 적용되는 구조.


![image](https://user-images.githubusercontent.com/61610411/128285565-799639ed-6dd6-46e0-8633-8408aeb038dc.png)




#### 다양한 차원에서의 Convolution


> 1D-conv

![image](https://user-images.githubusercontent.com/61610411/128287725-55b6800f-1561-4e18-92a7-dc1520d1a37b.png)



> 2D-conv


![image](https://user-images.githubusercontent.com/61610411/128287766-897a2eed-c80e-4868-9ab8-1ba1284f65fd.png)



> 3D-conv 

![image](https://user-images.githubusercontent.com/61610411/128287798-144ff20c-37ae-4c69-9cd7-9c95b2a1b397.png)


**i,j,k가 바뀌어도 커널 $$f$$의 값은 바뀌지 않음.**


- 입력크기를(H, W), 커널 크기를 K_H,K_W), 출력크기를 (O_H,O_W)라 하면
    
    O_H = H - K_H + 1
    O_W = W - K_W + 1





(참고자료:https://zsunn.tistory.com/entry/AI-CNN%EA%B3%BC-RNN%EC%9D%98-%EC%9D%B4%ED%95%B4)