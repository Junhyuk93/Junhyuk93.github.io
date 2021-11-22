---
layout: post
title:  "부스트캠프 AI Tech 2기 Level3 모델 경량화"
date:   2021-11-22 21:30:10 +0530
categories: 모델 경량화
tag: [경량화]
---

모델 경량화


# 모델 경량화

## 1.1 경량화를 왜 해야 하나요?

### On device AI

![](https://i.imgur.com/GYACZOH.png)

* 제한된 리소스내에서 AI를 활용하기 위해서는 경량화가 필수!



### AI on cloud(or server)

- 배터리, 저장 공간, 연산능력의 제약은 줄어드나, latency와 throughtput의 제약이 존재
- 같은 자원(=돈)으로 더 적은 latency와 더 큰 throughput이 가능하다면?

### Computation as a key component of AI progress

![](https://i.imgur.com/o4mapQ4.png)


### 경량화는 ?

- 모델의 연구와는 별개로, 산업에 적용되기 위해서 거쳐야하는 과정
- 요구조건(하드웨어 종류, latency 제한, 요구 throughput, 성능)들 간의 trade-off를 고려하여 모델 경량화/최적화를 수행


## 1.2 경량화 분야 소개

### 1. Efficient architecture 

- 매년 쏟아져 나오는 블록 모듈들
- 각 모듈 블록마다의 특성이 다름! (성능, 파라미터 수, 연산횟수 등..)

![](https://i.imgur.com/D7z8V8U.png)


### Efficient architecture design; AutoML, Neural Architecture Search
- Software 1.0 : 사람이 짜는 모듈
- Software 2.0 : 알고리즘이 찾는 모듈

![](https://i.imgur.com/Jv009zx.png)


### 2. Network Pruning; 찾은 모델 줄이기

- 중요도가 낮은 파라미터를 제거하는 것
- 좋은 중요도를 정의, 찾는 것이 주요 연구 토픽 중 하나 (e.g. L2 norm이 크면, loss gradient 크면, 등등..)
- 크게 structured/unstructured pruning으로 나뉘어짐

![](https://i.imgur.com/0TK8z9B.png)


### Network Pruning; Structured pruning

- 파라미터를 그룹 단위로 pruning하는 기법들을 총칭(그룹; channel / filter,layer 등)
- Dense computation에 최적화된 소프트웨어 또는 하드웨어에 적합한 기법

![](https://i.imgur.com/AxOQ6I0.png)

* channel scaling factor = 중요도

### Network Pruning; Structured pruning

- 파라미터 각각을 독립적으로 pruning하는 기법
- Pruning을 수행할 수록 네트워크 내부의 행렬이 점차 희소(sparse)해짐
- Structured Pruning과 달리 sparse computation에 최적화된 소프트웨어 또는 하드웨어에 적합한 기법

![](https://i.imgur.com/YA0z42K.png)


### 3. Knowledge distillation
- 학습된 큰 네트워크를 작은 네트워크의 학습 보조로 사용하는 방법
- Soft target(soft outputs)에는 ground truth 보다 더 많은 정보를 담고 있음
    (e.g 특정 상황에서 레이블 간의 유사도 등등)
    
![](https://i.imgur.com/C3oKKc2.png)

 - 1) Student network와 ground truth label의 cross-entropy
 - 2) teacher network와 student network의 inference 결과에 대한 KLD loss로 구성

![](https://i.imgur.com/1w3XqWO.png)

* T는 Large teacher network의 출력을 smoothing(soften)하는 역할을 한다.
* a는 두 loss의 균형을 조절하는 파라미터다.


### 4. Matrix/Tensor decomposition

- 하나의 Tensor를 작은 Tensor들의 operation들의 조합(합, 곱)으로 표현하는 것
- Cp decomposition : rank 1 vector들의 outer product의 합으로 tensor를 approximation

![](https://i.imgur.com/khv6pNK.png)


### Network Quantization

- 일반적인 float32 데이터타입의 Network의 연산과정을 그보다 작은 크기의 데이터타입
    (e.g float16, int8, ...)으로 변환하여 연산을 수행
    
![](https://i.imgur.com/aMThiKV.png)


- 사이즈 : 감소
- 성능(Acc) : 일반적으로 약간 하락
- 속도 : Hardware 지원 여부 및 사용 라이브러리에 따라 다름(향상 추세)
- Int8 quantization 예시(CPU inference, 속도는 pixel2 스마트폰에서 측정됨)

![](https://i.imgur.com/eZ0TOsl.png)

### 6. Network Compiling 

- 학습이 완료된 Network를 deploy하려는 target hardware에서 inference가 가능하도록 compile하는 것(+ 최적화가 동반)
- 사실상 **속도에 가장 큰 영향**을 미치는 기법
- e.g TensorRT(NVIDIA), Tflite(Tensorflow), TVM(apache), ...
- 각 compile library마다 성능차이가 발생


![](https://i.imgur.com/zHqdj7x.png)

- Compile 과정에서, layer fusion(graph optimization) 등의 최적화가 수행됨
- 예를들어 tf의 경우 200개의 rule이 정의되어 있음

![](https://i.imgur.com/cpjOmtp.png)

- Framework와 hardware backends 사이의 수많은 조합
- HW마다 지원되는 core, unit 수, instruction set, 가속 라이브러리 등이 다름
- Layer fusion의 조합에 따라 성능차이가 발생 (동일 회사의 hw 임에도 불구하고!)

![](https://i.imgur.com/36YuNqt.png)

- AutoML로 graph의 좋은 fusion을 찾아내자!
- e.g. AutoTVM(Apache)

