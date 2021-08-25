---
layout: post
title:  "부스트캠프 AI Tech 2기 Day - 5 - RNN"
date:   2021-08-06 13:15:36 +0530
categories: TIL
---

RNN

## RNN 이해하기


### RNN


- **순환 신경망(Recurrent Neural Network, RNN)** 은 인공 신경망의 한 종류로, 유닛간의 연결이 순환적 구조를 갖는 특징을 가지고 있다. 이러한 구조는 사변적 동적 특징을  모델링 할 수 있도록 신경망 내부에 상태를 저장할 수 있게 해주므로 순방향 신경망(feedforward neural network)과 달리 내부의 메모리를 이용해 시퀀스 형태의 입력을 처리할 수 있다. 따라서 순환 인공 신경망은 시변적 특징을 지니는 데이터를 처리하는데 적용할 수 있다.

![image](https://user-images.githubusercontent.com/61610411/128448274-3545b680-1328-4060-bcb1-b01eb296cce6.png)

---
#### 기본적인 RNN 수식


![image](https://user-images.githubusercontent.com/61610411/128448833-a5ecef81-16f3-4ebd-9914-d2c760c9bb22.png)

- **시퀀스 데이터(sequence data)** 란?
 각각의 데이터가 순서가 있는 데이터를 말하며, 시퀀스 원소들은 특정 순서를 가지므로 독립적이지 않다.
 대표적인 시퀀스 데이터로 시계열 데이터(시간의 흐름에 따라 기록된 데이터)와 텍스트 데이터(문맥이 존재), 주가 데이터 등 이 있다.

---
### RNN 방식

#### 1. 양방향 순환 신경망(Bi-directional RNN)
- 양방향 순환 신경망은 길이가 정해진 데이터 순열을 통해 어떤 값이 들어오기 전과 후의 정보를 모두 학습하는 방식의 알고리즘이다. 이를 위해 순열을 왼쪽에서 오른쪽으로 읽을 RNN 하나와, 오른쪽에서 왼쪽으로 읽을 RNN 하나를 필요로 한다. 이 둘의 출력값을 조합한 뒤 지도된 결과와 비교하여 학습하는 것이다. LSTM과 병용할 때 특히 좋은 성능을 낸다는 사실이 증명되었다.

![image](https://user-images.githubusercontent.com/61610411/128449766-485faf0d-5c8c-4dad-9717-d3a0f8f7e3b4.png)


#### 2. LSTM(Long Short-Term Memory)
- LSTM은 **기울기 소실 문제**(vanishing gradient problem)를 해결하기 위해 고안된 딥 러닝 시스템이다. LSTM은 망각 게이트(forget gate)라 부르는 게이트를 추가적으로 가진다. 이 게이트를 통해 역전파시 기울기값이 급격하게 사라지거나 증가하는 문제를 방지할 수 있다. 이로써 기존의 RNN은 먼 과거의 일로부터 학습하는 것이 산술적으로 거의 불가능했지만, LSTM은 수백만 단위 시간 전의 사건으로부터도 학습할 수 있음으로서 고주파 신호뿐 아니라 저주파 신호까지도 다룰 수 있게 되었다.


![image](https://user-images.githubusercontent.com/61610411/128453202-17a26968-a9e5-4eac-88df-46826c6e4828.png)


#### 3. GRU(Gated Recurrent Units, 게이트된 순환 유닛)
 - GRU는 2014년에 처음으로 발명된 구조이고 출력게이트가 존재하지 않으므로, LSTM에 비해 더 적은 수의 매개변수를 가짐에도 불구하고 다성음악 학습이나 음성 인식 분야에서 LSTM과 유사한 성능을 가진다.


![image](https://user-images.githubusercontent.com/61610411/128453464-e0605a7c-98ee-4815-ac24-1c29e2b329d4.png)


#### LSTM과 GRU의 차이점



- 망각게이트(forget gate)의 유무
> 망각게이트(forget gate) : 과거 정보를 버릴지 말지 결정하는 과정. 즉, 망각게이트는 현재 입력과 이전 출력을 고려해서 cell state의 어떤 값을 버릴지 결정하는 역할.

![image](https://user-images.githubusercontent.com/61610411/128453619-77c1f37d-ea78-4124-8e70-594cb01a9dce.png)

---
 
참고자료


<https://ko.wikipedia.org/>


<https://excelsior-cjh.tistory.com/154>


<http://colah.github.io/posts/2015-09-NN-Types-FP/>


<https://wegonnamakeit.tistory.com/7>