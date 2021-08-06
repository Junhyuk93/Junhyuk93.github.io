---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 4 - CNN"
date:   2021-08-05 23:15:36 +0530
categories: TIL
---

-

## CNN 이해하기
---

- **합성곱 신경망** (Convolutional neural network, CNN)은 시각적 영상을 분석하는 데 사용되는 다층의 피드-포워드적인 인공신경망의 한 종류이다. 공유 가중치 구조와 변환 불변성 특성에 기초하여 변이 불변 또는 공간 불변 인공 신경망(SIANN)으로도 알려져 있다. Convolution 연산은 커널(kernel)을 입력 벡터 상에서 움직여가면서 선형모델과 합성함수가 적용되는 구조이다. CNN 은 크게 **합성곱 계층(Convolution layer)**과 **풀링 계층(Poolding layer)**으로 구성된다.
<center>
![image](https://user-images.githubusercontent.com/61610411/128285565-799639ed-6dd6-46e0-8633-8408aeb038dc.png)
<center>


- 합성곱 계층(Convolution layer)

  이미지와 같은 3차원 데이터를 입력 받으면 다음 계층에도 3차원 데이터로 전달한다. CNN에서 합성곱 계층의 입출력 데이터는 다차원이기에 이것을 특징 맵(Feature Map)이라고 한다. 입력 데이터를 입력 특징 맵(Input Feature), 출력데이터를 출력 특징 맵(Output Feature Map) 이라고 한다.

![image](https://user-images.githubusercontent.com/61610411/128440431-00c54a17-6bad-45d6-97c9-d3642d3571d8.png)


- 패딩(Padding)

  합성곱 연산을 수행하기 전에 입력 데이터 주변은 특정 값(0,1 등)으로 채우기도 하는데, 이를 패딩이라 하며 합성곱 연산에서 자주 이용되는 기법이다. 폭 1짜리 패딩이면 데이터 사방 1픽셀을 특정 값으로 채우는 것을 말한다.


![image](https://user-images.githubusercontent.com/61610411/128440559-54be47e2-cbef-4767-b86f-b59f02caba8d.png)


  패딩을 적용하는 이유는 출력의 크기를 조절하기 위함이다. 패딩 없이 입력 데이터에 필터를 씌워 합성곱 연산을 수행하면 출력 데이터는 입력 데이터에 비해 무조건 작아지게 되어있다. 합성곱 연산을 반복하면 어느 시점에서는 출력 크기가 1이 되고, 더이상 합성곱 연산을 수행할 수 없게 된다. 이러한 사태를 방지하기 위해 패딩을 사용하는 것이다. 패딩을 사용하면 출력 데이터의 크기를 입력 데이터의 크기와 동일하게 설정할 수 있다.


#### 다양한 차원에서의 Convolution


> 1D-conv
<center>
![image](https://user-images.githubusercontent.com/61610411/128287725-55b6800f-1561-4e18-92a7-dc1520d1a37b.png)
<center>


> 2D-conv

<center>
![image](https://user-images.githubusercontent.com/61610411/128287766-897a2eed-c80e-4868-9ab8-1ba1284f65fd.png)
<center>


> 3D-conv 

<center>
![image](https://user-images.githubusercontent.com/61610411/128287798-144ff20c-37ae-4c69-9cd7-9c95b2a1b397.png)
<center>

<center>
**i,j,k가 바뀌어도 커널 $f$ 의 값은 바뀌지 않음.**
<center>

- Convolution 연산의 역전파

    오류 역전파 알고리즘(Backpropagation Algorithm)은 정방(Feedforward) 연산 이후, 에러예측값과 실제값의 오차를 후방(Backward)으로 다시 보내 줌으로써, 많은 노드를 가진 MLP라도 최적의 Weight와 Bias를 학습할 수 있도록 한다.


<center>
![image](https://user-images.githubusercontent.com/61610411/128441981-c2063786-cf08-4815-ae8e-ccf98b9688d4.png)
<center>


---


<center>
![image](https://user-images.githubusercontent.com/61610411/128441559-e52c3725-5f65-44bd-b3ec-24459f375531.png)
<center>

- Convolution 연산은 커널이 모든 입력데이터에 공통으로 적용되기 때문에 역전파를 계산할 때도 convolution 연산이 나오게 된다.

(참고자료)

https://zsunn.tistory.com/entry/AI-CNN%EA%B3%BC-RNN%EC%9D%98-%EC%9D%B4%ED%95%B4

https://kolikim.tistory.com/53?category=733477

https://ko.wikipedia.org/wiki

http://solarisailab.com/archives/1206