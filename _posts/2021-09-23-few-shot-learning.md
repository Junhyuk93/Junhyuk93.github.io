---
layout: post
title:  "부스트캠프 AI Tech 2기 Level2 few-shot learning"
date:   2021-09-23 21:30:10 +0530
categories: fewshot-learning
tag: [fewshot-learning]
---


# few show learning

---

## 퓨샷 러닝(few show learning) 이란?

퓨샷 러닝은 말 그대로 적은(few) 양의 데이터를 가지고 학습하는 것을 의미한다. 퓨샷 러닝 모델이 완전히 새롭게 주어진 데이터에서도 잘 작동하도록 만들기 위해서는 에피소딕 훈련(episodic training) 방식의 메타러닝(Meta learning, 사람이 통제하던 기계학습 과정을 자동화함으로써 기계 스스로 학습 규칙을 익힐 수 있게 하는 방법)이 필요하다. 에피소딕 훈련은 퓨샷 작업과 유사한 형태의 훈련 작업을 통해 모델 스스로 학습 규칙을 찾아낼 수 있도록 하여 일반화 성능을 향상하는 역할을 한다.


![](https://i.imgur.com/DF2j1sj.png)

사람에게 왼쪽과 같은 support set 의 이미지를 주고 오른쪽의 Query가 Armadillo 인지 Pangolin 인지 풀게 한다면 모두 Query image는 Pangolin 이라고 대답할 것이다.

전통적인 딥러닝 모델은 이와 같이 각 클래스별 사진 단 두장을 가지고 Query 이미지를 맞출 수 있었을까? 아마도 Armadillo image 1000장, Pangolin image 1000장을 준비했어야 할지도 모른다. 그렇다면 우리는 이 문제에 어떻게 이문제에 대해서 쉽게 답할 수 있었을까? 이러한 학습시도를 **Meta Learning** 이라고 한다.

하지만 우리가 구분하는 "방법을 배우는"  과정 에서는 수많은 학습이 있었다. 사자와 호랑이가 다른 것을 배우고, 토끼와 고양이가 다른 것을 배우고와 같은 수 많은 시행착오와 학습이 바로 지금의 내가 천산갑과 아르마딜로가 다르다는 판단을 내릴 수 있게 도와주었던 것이다.


Few shot learning은 바로 이러한 점에 착안된 Meta learning의 한 종류이다. 따라서 "배우는 법을 배우려면" 어찌 되었든 많은 데이터가 필요하고 아래와 같은 데이터들로 학습이 될 것이다. 다만 다른 점은 구분하려는 문제 (Armadillo 인지 Pangolin 인지) 의 데이터는 Training set에 없어도 무방하다.

![](https://i.imgur.com/pXPrzWJ.jpg)


우리는 Few show learning을 위해 Training set, Support set, Query image가 필요하다는 점을 이해할 수 있다. 한마디로 정리하면, Training set을 통해 구분하는 법을 배우고, Query image가 input으로 들어왔을 때, 이 Query image가 Support set 중 어느 것과 같은 종류인지를 맞추는 일을 하는 것이다. 즉 **Query image가 어떤 클래스에 속하느냐의 문제를 푸는 것이 아니라 어떤 클래스와 같은 클래스냐 의 문제** 를 푼다고 생각하면 이해하기가 쉽다.

![](https://i.imgur.com/oUFNfb1.png)

## What's the difference?

먼저 Supervised learning은 아래의 그림처럼 Test image(Query image)의 클래스가 Training set에 있다. 즉, 학습에 강아지 사진을 주고 강아지를 잘 학습했는지 묻는 것이다.

![](https://i.imgur.com/OOxUdpH.png)

<center>하지만 Few sohw learning은 Training set에 없는 클래스를 맞추는 문제이다. 
</center>


![](https://i.imgur.com/iIZN2cE.png)

이 Support set의 클래스 개수와 샘플 수를 기준으로 k-way n-shot 이라는 표현을 쓴다. k-way는 Support set이 k개의 클래스로 이루어졌다는 것이고, 이는 Query image가 k개의 클래스 중 어떤 것과 같은 것인지 묻는 문제가 되므로 k가 클수록 모델의 정확도는 낮아지게 된다. n-shot은 각 클래스가 가진 sample의 개수로 비교해볼 사진이 많으면 많을수록 어떤 클래스에 속하는지 알기 쉽기 때문에 n이 클 수록 모델의 정확도가 높아지게 된다. 그리고 이 n이 1이 되면 one-shot learning 이라고 부르게 된다.

![](https://i.imgur.com/0DgefbN.png)


Transfer learning과 다른 점은 사실 굉장히 애매하다. Transfer learning은 특히 vision 분야에서도 다른 도메인으로 학습된 모델의 layer의 일부를 frezee 하고 일부를 다른 도메인의 image로 fune-tuning하는 과정을 통칭한다. 이 때 새로운 도메인의 경우 많은 라벨링된 데이터가 있을 수도 있다. 하지만 Few shot learning의 경우 꼭 일부를 frezee하고 fine-tuning 하는 것을 의미하지는 않으며(fine-tuning을 안해도 상관이 없음) 말 그대로 새로운 도메인(or unseen dataset)이 적게 있는 경우를 지칭한다.

## 학습 방법

**Few shot learning의 기본 학습 방법은 유사성을 학습하는 것이다.** 즉, 두 개의 사진이 주어졌을 때 각 사진을 잘 분석해서 두 사진이 유사한지 다른지를 판단할 수 있다면, Query image가 주어졌을 때 Support set의 사진들과 비교하여 어떤 클래스에 속하는지 알아낼 수 있다.

![](https://i.imgur.com/FXinYi3.png)

그림에서 x1과 x2 는 같은 class이고, x1과 x3는 다른 class인 것을 잘 알아내는 모델을 학습하는 것이다. 따라서 우리의 모델은 많은 training set을 통해 각 사진별로 중요한 특징들을 잘 추출해서 "같다"와 "다르다"를 학습해야 한다.


![](https://i.imgur.com/BpjsGfQ.png)

이후에 Query image에 대해 Support set의 image들과 유사성을 구하고 가장 유사한 이미지를 가진 class로 분류할 수 있게 된다.

### How to make dataset?

![](https://i.imgur.com/Kqh53KH.png)

그림과 같이 Positive set, Negative set으로 구성하여 학습이 진행된다. 이 때 Feature extraction을 잘 학습할 수 있는 모델을 디자인해야 하는데 일반적인 Conv-Relu-Pool의 구조도 충분히 적합하다.

![](https://i.imgur.com/dbzIec7.png)

기초 Few show learning에서는 샴 네트워크(Siamese Network)를 사용하는데, 같은 CNN 모델을 이용하여 hidden representation을 각각 구한 뒤 이 차이를 이용하는 방식이다.

![](https://i.imgur.com/c95O63i.png)

이후 Positive pair와 Negative pair에 대해서 번갈아가며 학습을 진행하게 된다.

![](https://i.imgur.com/GbN7GoM.png)

![](https://i.imgur.com/tFvsaDM.png)


Prediction에서는 위에서 설명한 것과 같이 Support set의 이미지의 representation과 Query image의 representation 간의 차이를 샴 네트워크를 이용하여 training에서의 방법과 같이 계산하여 유사성을 구할 수 있게 된다.

![](https://i.imgur.com/Ds4SdHf.png)

## Trend of research on fuset learning

### 1. 거리 학습 기반 방식 (metric learning)

범주별 훈련 데이터의 수가 적은 few shot task에서는 딥러닝 처럼 classification의 weight를 훈련하는 방식이 적합하지 않다. 아래 그림과 같이 task를 구성하는 N * K개의 소수 훈련 데이터에만 지나치게 적응하는 overfitting 현상이 발생할 수 있기 때문이다.

![](https://i.imgur.com/2IxD6RT.png)

그 대신 아래 그림처럼 Support data와 Query data 간의 거리(유사도)를 측정하는 방식을 활용한다. 모델은 주어진 서포트 데이터를 특징 공간(feature space)에 나타낸다(=특징 추출). 이 공간 상에서 Query data의 범주는 유클리디안 거리(Euclidean distance)가 가장 가까운 Support data의 것으로 예측된다. (거리 계산 + 최근린 선택).

![](https://i.imgur.com/aOMWFqM.png)

모델은 아래 그림처럼 두 데이터의 범주가 같으면 거리를 더 가깝게 두고, 다를 때는 거리가 더 멀게 만드는 방법을 학습한다. 이를 두고 거리 학습(distance training)이라고 한다. few shot learning 초기에 활용되던 거리 학습은 가장 간단하면서도 효과적인 것으로 알려져 있다.

![](https://i.imgur.com/nXLBZ6n.png)

#### 1.  [Siamese Neural Network for one-shot image recognition](https://www.cs.cmu.edu/~rsalakhu/papers/oneshot1.pdf)

이전에는 인간이 직접 설계한 특징으로 거리 학습을 시도했다면, 샴 네트워크(Siamese network)는 처음으로 Deep Neural Network(DNN) 를 활용했다는 점에서 의의가 있다고 본다. 대표적인 DNN 응용 알고리즘인 CNN으로 특징 추출기를 만든 것이다. 매개변수를 공유하는 동일한 구조의 특징 추출기는 두 데이터 간 거리를 학습한다.

자세히 설명하자면, CNN은 검증 손실 함수(verification loss funtion) 값을 최소화할 때까지 훈련된다. 이 검증 손실 값은 두 입력 데이터의 범주가 같은 상황에서 특징 공간상 거리가 멀면 커진다. 두 데이터의 범주가 다른 상황에서 거리가 가까워져도 마찬가지이다. 이런 검증 손실 값을 최소화함으로써 모델은 범주가 같은 두 데이터 거리가 가까워질수록, 또는 범주가 다른 데이터의 거리가 멀어지게 하는 특징을 얻게 된다.

![](https://i.imgur.com/znAhjMb.png)


#### 2. [Matching networks for one-shot learning](https://arxiv.org/abs/1606.04080)

샴 네트워크에서 특징 추출기는 두 개의 입력 데이터 간 거리를 절대적으로 0으로 만들거나 크게 만드는 훈련에 집중한다. 그러나 이는 테스트 단계에서 주어지는 N-way K-shot 문제를 푸는 데 최적화된 방법론이라고 볼 수는 없다. 쿼리의 범주는 상대적으로 더 가까운 Sopport data의 것으로 결정되기만 하면 된다. 따라서 N-way K-shot 문제에서는 데이터 간 상대적 거리를 잘 표현하는 특징 추출기를 만들 필요가 있다.

이 논문에서는 최근린 선택기를 미분이 가능한 형태로 제안함으로써 특징 추출기가 스스로 데이터 간 상대적 거리를 표현하는 방법을 익히도록 했다. 아울러 N-way K-shot training task에 기반한 에피소딕 훈련 방식을 하는 등 모델의 범주 예측 성능을 높였다.

#### 3. [Prototypical networks for few-shot learning](https://arxiv.org/abs/1703.05175)

5-way 5-shot task 가 주어졌을 때 기존 방식에서는 Support data 25개와 Query data 간 거리를 일일이 계산했다. 반면, 이 노눔네엇는 범주별 서포트 데이터의 평균 위치인 프로토타입(prototype)이라는 개념을 사용한다. 결과론적으로 모델은 5개 범주를 대표하는 프로토타입 벡터와 쿼리 벡터와의 거리만 계산하면 된다.

저자는 퓨샷 데이터가 주어진 상황에서 프로토타입 네트워크가 Matching networks보다 성능 면에서 더 유리하다고 주장한다. 쿼리 예측에 필요한 계산량을 N * K에서 N개로 줄이는 한편, 그 구조가 더 단순하다는 걸 근거로 제시하고 있다.

![](https://i.imgur.com/Ksq0N5i.png)

위 프로토타입 네트워크는 범주별 서포트 벡터 간 평균 위치인 프로토타입을 활용해 모델의 복잡도를 줄였다.

#### 4. [Learning to compare : relation network for few-shot learning](https://arxiv.org/abs/1711.06025)

한 task에서{고양이, 자동차, 사과} 처럼 서로 완전히 다른 성격의 범주를 분류하는 문제라면 물체의 모양{shape} 정보만으로도 쿼리 데이터의 범주를 쉽게 예측할 수 있을 것이다. 하지만 특징 추출기가 같은 범주의 데이터를 더 가깝게, 다른 범주의 데이터를 더 멀게 할 정도로 충분히 복잡하지 않다면 어떨까? 그렇다면 {러시안블루, 페르시안, 먼치킨}처럼 고양이의 종류를 구분하는 task를 풀기 어려울 것이다.

이 논문에서는 특징 추출기에 CNN을 적용했을 뿐만 아니라 거리 계산 함수에도 다층 퍼셉트론을 적용시켰다. 다층 퍼셉트론은 같은 범주 또는 다른 범주의 서포트 데이터와 쿼리 데이터를 분류하는 법을 배운다.

## 2. 그래프 신경망 방식(Graph Neural Network)

최근에는 적은 양의 데이터만으로도 분류 성능을 극대화하고자 데이터 간 복잡한 관계 정보를 학습에 활용하는 추세이다. 가장 많이 연구되는 게 바로 그래프 신경망이다.

우리가 흔히 아는 일반적인 인공 신경망은 입력값으로 벡터나 행렬 형태를 활용한다면, GNN 은 밀집 그래프(dense graph) 구조를 활용한다. 그래프에서 노드는 데이터를, 노드와 노드를 잇는 간선(edge)은 데이터 간 관계 정보를 나타내며 밀집 그래프는 모든 노드가 서로 완전히 연결된 것을 가리킨다. GNN은 바로 이 그래프 구조와 각 노드에 해당하는 데이터의 특징 벡터를 입력받는다.

#### 1. [Few-shot learning with graph neural networks](https://arxiv.org/abs/1711.04043)

이 논문에서는 GNN의 동작 방식은 다음과 같다. 각 노드는 해당하는 데이터의 특징 벡터로 초기화된다. 그 다음, 특정 노드 V의 이웃 노드에 노드별 거리(유사도)를 곱한 값들의 합(가중평균)을 구한다. 이를 V와 합쳐 새로운 벡터 V'를 얻는다. 다른 노드에 대해서도 같은 연산을 순차적으로 반복한다. 가장 마지막에 쿼리 노드 벡터값의 업데이트를 완료한다. 모델은 N개의 범주와 완전히 연결된 Fully connected layer층을 통해 쿼리 데이터의 범주를 예측한다.


![](https://i.imgur.com/WbIw0Ai.png)

한 task를 구성하는 데이터 간의 관계를 나타내고자 그래프 신경망 구조를 활용하고 있다.

#### 2. Transductive propagation network for few-shot learning (TPN)

TPN 또한 기존 GNN 처럼 특정 노드와 이웃 노드와의 거리를 계산해 범주 정보를 전파한다. GNN과 다른 점은 노드값을 초기화한 후 더는 업데이트하지 않는 데 있다. 이렇게 되면 범주 정보를 연쇄적으로 전퍼하는 부분을 하나의 닫힌 형태 방정싱(close form equation)으로 표현할 수 있게 된다. 이 방식에서는 범주 정보 전파 횟수에 비례해 늘어나는 연산 횟수가 단 한번으로 줄어 들고, 매 단계에서 얻은 각 노드의 범주 벡터는 메모리에 기록될 필요가 없다. 즉, 노드 사이 거리를 고정하면 계산량과 메모리 사용량을 획기적으로 줄일 수 있다는 의미이다.

TPN과 GNN의 또 다른 차이점은 한 그래프에서 참고하는 서포트 데이터의 수입니다. 5-way 5-shot 문제에 10개의 쿼리 데이터가 있다고 가정해보자. GNN은 25개의 서포트 데이터와 1개의 쿼리 데이터, 즉 26개의 노드로 이루어진 그래프를 각각 10개를 구성해 쿼리 10개에 대한 범주를 완전히 독립적으로 예측한다. 반면, TPN은 25개의 서포트 데이터와 10개의 쿼리 데이터, 즉 35개의 노드로 이루어진 단 하나의 그래프를 구성해 쿼리 10개의 범주를 한꺼번에 예측한다.

이처럼 TPN은 모든 쿼리 데이터를 학습에 활용함으로써 그 범주를 더 정확하게 예측하게 된다. 데이터가 지극히 적은 상황에서 쿼리 데이터도 활용하면 저차원의 매니폴드(manifold) 공간에서 결정 경계(dicision boundary)를 더욱 수월하게 찾을 수 있기 때문이다. 이처럼 labeling data와 test data의 분포를 고려해 test data의 범주를 추론하는 방식을 변환 학습(transductive learning)이라고 한다.

![](https://i.imgur.com/s09FQoj.png)

TPN은 주어진 서포트 데이터와 쿼리 데이터를 단 하나의 그래프로 구성하고 쿼리의 범주를 동시에 예측한다. 쿼리의 매니폴드를 활용하면 범주 분류 성능을 높일 수 있어서다.



---

참고자료

https://www.kakaobrain.com/blog/106

[https://www.youtube.com/watch?v=hE7eGew4eeg](https://www.youtube.com/watch?v=hE7eGew4eeg)